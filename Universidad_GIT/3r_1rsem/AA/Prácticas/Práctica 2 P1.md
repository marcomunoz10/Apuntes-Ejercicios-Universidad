
## Versión 0:
```console

[aa-29@aolin-login practica2]$ gcc p1.c -Ofast -march=native -fno-inline -o p1
[aa-29@aolin-login practica2]$ perf stat -e cycles,instructions p1
N= 500000, Rep= 10000
pi= 3.141592653590e+00, A[500000]= 4.521607626903e+05

 Performance counter stats for 'p1':

    29.323.689.566      cpu_core/cycles:u/
    82.537.873.297      cpu_core/instructions:u/

       6,694884282 seconds time elapsed

       6,687650000 seconds user
       0,000000000 seconds sys


```

1. **Perfila la ejecución con el tamaño de problema por defecto (N=500.000) para identificar los cuellos de botella del rendimiento. Analiza el código ensamblador de cada función**
Al hacer perf report tenemos estos resultados:

  66,68%  P        P              [.] compute_scan
  33,29%  P        P              [.] calculate_pi
### Cosas a tener en cuenta
1. **Registros**

	`eax` registro de 32 bits, 4 bytes
	`rax` registro de 64 bits, 8 bytes
	`xmm` registro de 128 bits, 16 bytes
	`ymm` resgistro de 256 bits, 32 bytes

2. **Tipo de sufijo**
	`ps`  (Packed Single-Precision): Instrucciones que operan sobre valores empaquetados de precisión simple (32 bits, tipo `float`), trabajando en paralelo sobre múltiples valores.
	
	`pd`(Packed Double-Precision): Instrucciones que operan sobre varios valores empaquetados de doble precisión (64 bits, tipo `double`) de manera paralela.
	
	`ss` (Scalar Single-Precision): Instrucciones escalares que operan sobre valores de precisión simple (32 bits, tipo `float`)
	
	`sd` scalar double, trabaja con registros de 64 bits, 8 bytes.
	
*compute_scan*
```bash

       │    compute_scan():
       │      test        %edx,%edx
       │    ↓ jle         58
       │      sub         $0x1,%edx
       │      vmovsd      (%rdi),%xmm2
       │      lea         0x8(%rdi),%rax
       │      add         $0x8,%rsi
       │      vmovsd      __dso_handle+0xd8,%xmm4
       │      lea         0x10(%rdi,%rdx,8),%rdx
  1,31 │20:   vmovsd      -0x8(%rsi),%xmm3
  0,14 │      vmovsd      (%rsi),%xmm1
  0,01 │      add         $0x8,%rax
 25,20 │      add         $0x8,%rsi
  0,44 │      vsubsd      %xmm1,%xmm3,%xmm0 ;; xmm3 - xmm1 = xmm0
  0,03 │      vsubsd      %xmm3,%xmm1,%xmm1
  0,27 │      vfmadd132sd %xmm1,%xmm4,%xmm0 ;;xmm0 = xmm0*xmm1 + xmm4
 21,79 │      vcvtsd2ss   %xmm0,%xmm0,%xmm0 ;;convierte de sd(doble) a ss(simple)
  1,01 │      vsqrtss     %xmm0,%xmm0,%xmm0 ;;hace raíz cuadrada
 24,43 │      vcvtss2sd   %xmm0,%xmm0,%xmm0 ;;vuelve a convertir a doble
  0,57 │      vaddsd      %xmm0,%xmm2,%xmm2
  0,11 │      vmovsd      %xmm2,-0x8(%rax)
       │      cmp         %rax,%rdx
 24,70 │    ↑ jne         20

```


No está vectorizado porque no pone la `p` de `packed`.

Miramos a ver la función:
```c
void compute_scan ( double *A, double *B, int n)
{
  int i;
  for ( i=0; i<n; i++ )
    A[i+1] = A[i] + sqrtf( 1.0 + (B[i]-B[i+1])*(B[i+1]-B[i]) );
}
```

#### Analizamos código ensamblador:
Se registra en `rsi` el valor de la primera posición de B[] y luego apunta a la siguiente posición antes de entrar al bucle. 
Registra en rax A[i] y hace lo mismo

Entonces:
xmm3 = b[i]
xmm1 = b[i+1]
xmm2 = a[i +1]

*calculate_pi*
```bash
     
     
       
     shr          $0x3,%edx
       │       vbroadcastsd %xmm12,%ymm9
       │       vmovapd      0x357(%rip),%ymm7        # 400c80 <__dso_handle+0x78>
       │       vmovapd      0x36f(%rip),%ymm6        # 400ca0 <__dso_handle+0x98>
       │       vmovapd      0x387(%rip),%ymm5        # 400cc0 <__dso_handle+0xb8>
       │       vbroadcastsd %xmm10,%ymm8
       │       xchg         %ax,%ax
       │ 80:   vextracti128 $0x1,%ymm4,%xmm1
       │       vcvtdq2pd    %xmm4,%ymm2
  6,65 │       vaddpd       %ymm7,%ymm2,%ymm2
       │       add          $0x1,%eax
       │       vcvtdq2pd    %xmm1,%ymm1
  6,31 │       vaddpd       %ymm7,%ymm1,%ymm1
       │       vpaddd       %ymm13,%ymm4,%ymm4
       │       vmulpd       %ymm9,%ymm2,%ymm2
       │       vmulpd       %ymm9,%ymm1,%ymm1
  6,11 │       vfmadd132pd  %ymm2,%ymm6,%ymm2
       │       vfmadd132pd  %ymm1,%ymm6,%ymm1
       │       vmulpd       %ymm8,%ymm2,%ymm2
       │       vmulpd       %ymm8,%ymm1,%ymm1
 21,99 │       vdivpd       %ymm2,%ymm5,%ymm2
 19,08 │       vdivpd       %ymm1,%ymm5,%ymm1
 21,14 │       vaddpd       %ymm1,%ymm2,%ymm2
 12,73 │       vaddpd       %ymm2,%ymm3,%ymm3
       │       cmp          %eax,%edx
  5,97 │     ↑ jne          80


```

Primeras impresiones.
1. Utiliza SIMD, se ve la `p` de `packed`.
2. Se utilizan registros `ymm` de 256 bits, 32 bytes y el problema utiliza `double` de 8 bytes, por tanto hay 4 carriles.
3. `vcvtdq2pd` se desglosa en:
	`v` vector
	`cvt` converted
	`dq` Doubleword (32 bits)
	`2` to
	`pd` packed double (64 bits) 
4. 
Función:
```c
double calculate_pi(int num_steps)
{
  volatile double init=0;
  double x, sum;

  sum = init;
  for ( int i=1; i<= num_steps; i++ )
  {
    x= (i-0.5)/(double) num_steps;
    sum = sum + 4.0/((1.0+x*x)*(double) num_steps);
  }
  return sum;
}
```



2. Optimizad
Al perfilar el programa veo que el 99% va destinado a compilar `compute_scan`. 
Veo la función en ensamblador y me doy cuenta que hay una conversión a double en cada linea. Al hacer sqrtf se convierte a float la operación. Al cambiarlo por `sqrt` ya no hace el cambio de tipo de variable pero el programa ahora tarda lo mismo que en la versión 0. 

En el examen de 2022 de AC hay un programa parecido el cual guarda el valor en un vector como V[i + 1], entonces decido aplicar la mejora propuesta para poder mejorar el código.
*el bucle no es vectorizable, debido a que existe una loop-carried dependence  escribir en Y[i+1] en la iteración i-ésima y después leer Y[i] en la iteración i+1*
Así que decido probarlo en el codigo de la siguiente manera:
```cpp
void compute_scan(double *A, double *B, int n)
{
    int i;

    // Primer bucle: calcula los valores intermedios y los almacena en A
    #pragma omp parallel for private(i)
    for (i = 0; i < n; i++)
    {
        double diff1 = (B[i] - B[i + 1]);
        double diff2 = (B[i + 1] - B[i]);
        A[i + 1] = sqrt(1.0 + diff1 * diff2);
    }

    // Segundo bucle: acumula los resultados en A
    #pragma omp parallel for private(i)
    for (i = 0; i < n; i++)
    {
        A[i + 1] += A[i];
    }
}
```


## Versión 3

En esta versión se ha añadido paralelización utilizando `openMP`.
Se añade la libreria `omp.h` 
El código es el siguiente:

```cpp
void compute_scan(double *A, double *B, int n)
{
    int i;

    #pragma omp parallel for private(i) 
    for (i = 0; i < n; i++)
    {
        float diff1 = (B[i] - B[i + 1]);
        float diff2 = (B[i + 1] - B[i]);
        A[i + 1] = sqrtf(1.0f + diff1 * diff2);
    }

    for (i = 0; i < n; i++)
    {
        A[i + 1] += A[i];
    }
}
```
Nota: el segundo bucle no se ha paralelizado debido a las dependencias que puede generar ya que se trata de una suma acumulativa.


## Versión 4
Al hacer perf report veo que se utiliza mucho tiempo en OpenMP. El uso de utilizar más hilos de los necesarios podría ser problemático, así que con la instrucción `omp_set_num_threads(4);` para regular el número de threads. En cuanto al tiempo no veo mejora, sin embargo, el número de instrucciones baja de 150G a 50G.


## Calculate_pi

### Versión 1
En la función `calculate_pi` el valor que se pasa por parámetro `N` hace una conversión en cada iteración a `double`. Para ahorrar tener que hacer esta conversión todo el rato se saca este cambio fuera del bucle. Con esta simple modificación se logra bajar dos segundos.

```cpp
double calculate_pi(int N)
{
  double x, sum=0;
  double num_steps = (double)N;
  for ( int i=1; i<= num_steps; i++ )
  {
    x= (i-0.5)/num_steps;
    sum = sum + 4.0/((1.0+x*x)*num_steps);
  }
  return sum;
}
```
## Versión 2
### Diferencias clave:

|Instrucción|Tipo de datos|Operación|Precisión|Uso principal|
|---|---|---|---|---|
|**`vaddpd`**|Doble precisión (64 bits)|Suma en punto flotante|Punto flotante|Cálculos en punto flotante de alta precisión (científicos, gráficos, financieros)|
|**`vpaddd`**|Enteros de 32 bits|Suma de enteros|Enteros|Operaciones aritméticas en enteros (procesamiento multimedia, imágenes, criptografía)|
## P0

```console

[aa-29@aolin-login practica2]$ gcc p1.c -Ofast -march=native -fno-inline -o p1
[aa-29@aolin-login practica2]$ perf stat -e cycles,instructions p1
N= 500000, Rep= 10000
pi= 3.141592653590e+00, A[500000]= 4.521607626903e+05

 Performance counter stats for 'p1':

    29.323.689.566      cpu_core/cycles:u/
    82.537.873.297      cpu_core/instructions:u/

       6,694884282 seconds time elapsed

       6,687650000 seconds user
       0,000000000 seconds sys


```
ipc = 
## P1
```console
pi= 3.141592653590e+00, A[500000]= 4.521607537574e+05

 Performance counter stats for 'P1':

    27.656.900.740      cpu_core/cycles:u/
    59.413.059.751      cpu_core/instructions:u/

       6,322288383 seconds time elapsed

       6,314191000 seconds user
       0,000999000 seconds sys

```

ipc = 2.15
```c
void compute_scan ( double *A, double *B, int n)
{
  int i;
  for ( i=0; i<n; i++ )
    A[i+1] = sqrtf( 1.0 + (B[i]-B[i+1])*(B[i+1]-B[i]) );

 for ( i=0; i<n; i++ )
    A[i+1] = A[i+1] + A[i];
}
```


## P2
se paraleliza el primer bucle

```console

N= 500000, Rep= 10000
pi= 3.141592653590e+00, A[500000]= 4.521607537578e+05

 Performance counter stats for 'P2':

   259.751.872.616      cpu_core/cycles:u/
    67.581.148.788      cpu_core/instructions:u/

       5,435561857 seconds time elapsed

      65,091700000 seconds user
       0,007983000 seconds sys


```


```c
void compute_scan ( double *A, double *B, int n)
{
  int i;
  #pragma omp parallel for
  for ( i=0; i<n; i++ )
    A[i+1] = sqrtf( 1.0 + (B[i]-B[i+1])*(B[i+1]-B[i]) );


 for ( i=0; i<n; i++ )
    A[i+1] = A[i+1] + A[i];
}
```


## P3
ahora añadimos dos threads al primer bucle


## P4

```console
pi= 3.141592653590e+00, A[500000]= 4.521607537574e+05

 Performance counter stats for 'P4':

    48.623.979.370      cpu_core/cycles:u/
    48.197.513.534      cpu_core/instructions:u/

       3,054302611 seconds time elapsed

      12,183290000 seconds user
       0,004944000 seconds sys


```


```c++
double calculate_pi(int num_steps)
{
  double x, sum = 0;
  double N = (double) num_steps;

  for ( int i=1; i<= N; i++ )
  {
    x= (i-0.5)/N;
    sum = sum + 4.0/((1.0+x*x)*N);
  }
  return sum;
}

```


### Clase miércoles

*calculate_pi*
Patrón de cómputo: **reduce**
Tendría q declarar la X como variable privada, sum es una variable global q le tengo q poner reduction.


*compute_scan*

```c++
int i = 0;
for(i=0;i<n;i++)
{
 double Y = B[i] - B[i+1];
 A
}
double S=A[0];
for (i=0; i<n; i++)
{
 S=A[i+1] +S;
 A[i+1] =S;
}
```

web uops.info 
poner Alder Lake-P