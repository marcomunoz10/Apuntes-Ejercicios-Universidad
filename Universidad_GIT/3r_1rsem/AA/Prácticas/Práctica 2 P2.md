### Programa 0

```shell
[aa-29@aolin-login pr2]$ gcc p0.c -Ofast -march=native -fno-inline -o p0
[aa-29@aolin-login pr2]$ perf stat -e cycles,instructions p0
AA-PR2: N= 500000, Rep= 2000
v= 2.40151358e+00, II[500000]= 2675700

 Performance counter stats for 'p0':

     9.432.910.066      cpu_core/cycles:u/
    13.037.533.634      cpu_core/instructions:u/

       2,214510666 seconds time elapsed

       2,209944000 seconds user
       0,000998000 seconds sys

```

IPC = 1,38
**perf report**
```shell

  32,13%  p0       p0             [.] loop1
  21,19%  p0       p0             [.] loop6
  14,18%  p0       p0             [.] loop5
  12,08%  p0       p0             [.] loop2
  10,75%  p0       p0             [.] loop4
   9,56%  p0       p0             [.] loop3
```


#### Análisis compilador

**loop1**
```shell

  1,29 │ 90:   vmovupd      (%rcx,%rdx,1),%ymm4
  0,07 │       vsubpd       0x8(%rcx,%rdx,1),%ymm4,%ymm0
  8,14 │       vfmadd132pd  %ymm0,%ymm2,%ymm0
 40,79 │       vsqrtpd      %ymm0,%ymm0
 32,85 │       vmulpd       %ymm3,%ymm0,%ymm0
  9,52 │       vmovupd      %ymm0,(%rax,%rdx,1)
  0,07 │       add          $0x20,%rdx
       │       cmp          %rdx,%rdi
  7,26 │     ↑ jne          90

```


Las instrucciones AVX (`vmovupd`, `vsubpd`, `vfmadd132pd`, `vsqrtpd`, `vmulpd`) muestran que el compilador ha vectorizado las operaciones sobre datos de **doble precisión (64 bits)**, utilizando registros YMM de 256 bits, los cuales permiten procesar **4 datos de 64 bits** por iteración del bucle.

La instrucción `add $0x20,%rdx` incrementa de 32 bytes en 32 bytes.

**loop2**
```shell

```
**`vroundpd $0x3, (%rax), %ymm0`**:
- Redondea valores empaquetados en doble precisión que están en la memoria apuntada por `%rax` y los coloca en el registro **YMM0**.

**Vectorizadas**: Las instrucciones que usan registros **YMM** (instrucciones con el prefijo `v`), como `vmovupd`, `vroundpd`, `vsubpd`, y `vmovupd` nuevamente. Estas son instrucciones SIMD (Single Instruction, Multiple Data) que procesan **4 valores de doble precisión (64 bits)** en paralelo en cada iteración del bucle.


AVX es una versión avanzada de SIMD.


**loop3**

```c
void loop3 ( double *IN, int *OUT, int n)
{
  for ( int i=0; i<n; i++ )
    OUT[i+1] = 10.0*IN[i];
}
```

```shell
 3,41 │ 30:   vmulpd       (%rcx,%rax,2),%ymm2,%ymm0
 50,34 │       vmulpd       0x20(%rcx,%rax,2),%ymm2,%ymm1
  1,29 │       vcvttpd2dq   %ymm0,%xmm0
 23,01 │       vcvttpd2dq   %ymm1,%xmm1
  5,40 │       vinserti128  $0x1,%xmm1,%ymm0,%ymm0
  4,70 │       vmovdqu      %ymm0,0x4(%rsi,%rax,1)
  0,12 │       add          $0x20,%rax
       │       cmp          %rax,%rdi
 11,73 │     ↑ jne          30

```

Igual q lo otro pero:
**`vcvttpd2dq %ymm0, %xmm0`**:

- Convierte valores empaquetados de doble precisión (64 bits) en **%ymm0** a enteros empaquetados de 32 bits, truncando los resultados, y almacena el resultado en **%xmm0**.
- **Vectorizada parcialmente** Aquí se realiza una operación de conversión vectorizada, pero el resultado se almacena en **%xmm0**, que es un registro más pequeño (128 bits), lo que indica que solo se convierten 2 valores de doble precisión en cada llamada (no todos los 4).
- 
**`vinserti128 $0x1, %xmm1, %ymm0, %ymm0`**:

- Inserta el contenido de **%xmm1** en la parte superior de **%ymm0**, construyendo un registro de 256 bits a partir de dos registros de 128 bits.

**loop4**

```c
void loop4 ( int *V, int n)
{
  for ( int i= 0; i<n; i++ )
    V[i+1] = V[i] + V[i+1];
}
```

```shell
       │      movslq %esi,%rsi
       │      mov    (%rdi),%eax
       │      lea    (%rdi,%rsi,4),%rdx
       │      nop
  1,15 │10:┌─→add    0x4(%rdi),%eax
       │   │  add    $0x4,%rdi
  0,10 │   │  mov    %eax,(%rdi)
       │   ├──cmp    %rdi,%rdx
 98,75 │   └──jne    10


```
**`add 0x4(%rdi), %eax`**:
- Aquí se está **sumando el valor en `V[i + 1]` (que está en la dirección `0x4(%rdi)`) con el valor en `%eax`** (que contiene el valor de `V[i]`).

Todas las instrucciones en este fragmento son operaciones escalares, ya que solo operan sobre un valor a la vez (32 bits para **%eax** o direcciones de memoria en **%rdi**). 

**loop5**

```c
void loop5 (double *OUT, int *IN, int n)
{
  for ( int i=1; i<= n; i++ )
    OUT[i] += IN[i] / (double) IN[n];
}
```

```shell

  2,68 │ 50:   vmovdqu      0x4(%rdx),%ymm5
  1,03 │       vcvtdq2pd    0x4(%rdx),%ymm1
       │       add          $0x20,%rdx
  9,80 │       add          $0x40,%rax
  1,11 │       vfmadd213pd  -0x40(%rax),%ymm2,%ymm1
  0,08 │       vextracti128 $0x1,%ymm5,%xmm0
  8,86 │       vcvtdq2pd    %xmm0,%ymm0
 17,93 │       vfmadd213pd  -0x20(%rax),%ymm2,%ymm0
       │       vmovupd      %ymm1,-0x40(%rax)
 50,14 │       vmovupd      %ymm0,-0x20(%rax)
       │       cmp          %rdx,%rcx
  8,37 │     ↑ jne          50

```

I**nstrucciones vectorizadas**:

- **`vmovdqu`** mueve **32 bytes** (8 enteros de 32 bits).
- **`vcvtdq2pd`** convierte **4 enteros de 32 bits** en **4 valores de punto flotante de 64 bits** (dado que usa YMM).
- **`vfmadd213pd`** opera sobre **4 valores de doble precisión** (64 bits cada uno) en cada invocación.
- `vmovupd` en este caso ha movido registro de 256 bits que contiene datos de 64 bits, mueve 4 valores pq 0x20 son 32 bytes, que son 4 variables de 8 bytes.
- **Cada bucle procesa 4 elementos en cada operación vectorizada (32 bytes en total por iteración)**.

**loop6**
```shell

```

P3 como hacerlo en murcojp8

### Loop 6 antes de for reduction
```shell

       │       shr          $0x3,%edx
       │       vmovd        %esi,%xmm4
       │       shl          $0x5,%rdx
       │       vpbroadcastd %xmm4,%ymm4
       │       add          %rdi,%rdx
       │ 40:   vpaddd       0x4(%rax),%ymm4,%ymm0
       │       add          $0x20,%rax
  0,10 │       cmp          %rax,%rdx
 14,90 │       vcvtdq2pd    %xmm0,%ymm1
  8,51 │       vdivpd       %ymm1,%ymm3,%ymm1
  0,05 │       vextracti128 $0x1,%ymm0,%xmm0
  5,46 │       vcvtdq2pd    %xmm0,%ymm0
 37,51 │       vdivpd       %ymm0,%ymm3,%ymm0
 10,43 │       vaddpd       %ymm0,%ymm1,%ymm0
  8,85 │       vaddpd       %ymm0,%ymm2,%ymm2
 14,20 │     ↑ jne          40
       │       vextractf128 $0x1,%ymm2,%xmm3
       │       mov          %ecx,%edx
       │       vaddpd       %xmm2,%xmm3,%xmm1
       │       and          $0xfffffff8,%edx
       │       vaddpd       %xmm3,%xmm2,%xmm2
       │       cmp          %edx,%ecx

```

después:
```shell

       │       vpbroadcastd %xmm4,%ymm4
       │       shl          $0x5,%r8
       │       add          %rcx,%r8
       │       nop
  0,10 │ 98:   vpaddd       (%rcx),%ymm4,%ymm0
  0,03 │       add          $0x20,%rcx
       │       cmp          %rcx,%r8
  3,61 │       vcvtdq2pd    %xmm0,%ymm1
 41,75 │       vdivpd       %ymm1,%ymm3,%ymm1
       │       vextracti128 $0x1,%ymm0,%xmm0
  0,96 │       vcvtdq2pd    %xmm0,%ymm0
 34,31 │       vdivpd       %ymm0,%ymm3,%ymm0
  9,43 │       vaddpd       %ymm0,%ymm1,%ymm0
  6,47 │       vaddpd       %ymm0,%ymm2,%ymm2
  2,91 │     ↑ jne          98
       │       vextractf128 $0x1,%ymm2,%xmm3
       │       mov          %eax,%ecx
  0,03 │       vaddpd       %xmm2,%xmm3,%xmm1
       │       and          $0xfffffff8,%ecx
       │       vaddpd       %xmm3,%xmm2,%xmm2

```
Se paralelizan todas las funciones menos `loop 4` por las dependencias que puede generar .
En `loop6` se ha paralelizado de la siguiente manera: 
```cpp
double loop6(int *V, int n) { 
	double sum = 0.0; 
	#pragma omp parallel for reduction(+:sum) 
	for (int i = 1; i <= n; i++) 
	{ 
		sum += 1.0 / (1 + V[i]); 
	} 
	return sum; 
}
```

La cláusula `reduction(+:sum)` asegura que cada hilo tenga su propia copia de la variable `sum`. Al final del bucle, las copias locales se combinan en la variable `sum` global utilizando la operación especificada (`+` en este caso).







