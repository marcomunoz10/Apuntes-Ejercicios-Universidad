
# EJERCICIO 2
### Versión divide & conquer
```shell
[aa-29@aolin-login practica3]$ perf stat -e cycles,instructions MMcv 1024 512
Matrix Multiply 1024 x 1024 Recursive up to 512 x 512 blocks
Result= -1518797661

 Performance counter stats for 'MMcv 1024 512':

     9.466.083.744      cpu_core/cycles:u/
     8.732.776.502      cpu_core/instructions:u/

       2,170147296 seconds time elapsed

       2,164055000 seconds user
       0,003977000 seconds sys
       
IPC = 0,91

```


v1 

```shell


[aa-29@aolin-login practica3]$ perf stat -e cycles,instructions MMpar1 1024 512
Matrix Multiply 1024 x 1024 Recursive up to 512 x 512 blocks
Result= -1518797661

 Performance counter stats for 'MMpar1 1024 512':

    10.052.335.382      cpu_core/cycles:u/
     8.758.522.486      cpu_core/instructions:u/

       0,594207437 seconds time elapsed

       2,419023000 seconds user
       0,002992000 seconds sys

IPC = 0,875
```
### Classical
```shell
[aa-29@aolin-login practica3]$ perf stat -e cycles,instructions MMcv 1024 512
Matrix Multiply 1024 x 1024 Recursive up to 512 x 512 blocks
Result= -1518797661

 Performance counter stats for 'MMcv 1024 512':

     9.466.083.744      cpu_core/cycles:u/
     8.732.776.502      cpu_core/instructions:u/

       2,170147296 seconds time elapsed

       2,164055000 seconds user
       0,003977000 seconds sys

```

v1.

Poniendo `#pragma omp parallel for schedule (static)`
```c++

[aa-29@aolin-login practica3]$ perf stat -e cycles,instructions MMpar1 1024 0
Matrix Multiply 1024 x 1024 Classical
Result= -1518797661

 Performance counter stats for 'MMpar1 1024 0':

    13.191.755.248      cpu_core/cycles:u/
     8.746.593.988      cpu_core/instructions:u/

       0,304436121 seconds time elapsed

       3,307263000 seconds user
       0,002984000 seconds sys

```

Verificad con el comando perf stat el número de threads que se crean para la ejecución de la versión iterativa paralela

perf stat -e cycles,instructions,task-clock MMpar1 1024 0


```shell

[aa-29@aolin-login practica3]$ perf stat -e cycles,instructions,task-clock MMpar1 1024 0
Matrix Multiply 1024 x 1024 Classical
Result= -1518797661

 Performance counter stats for 'MMpar1 1024 0':

    12.475.350.580      cpu_core/cycles:u/        #    3,979 G/sec
     8.743.502.438      cpu_core/instructions:u/  #    2,789 G/sec
          3.135,35 msec task-clock:u              #   11,325 CPUs utilized

       0,276858492 seconds time elapsed

       3,128253000 seconds user
       0,005001000 seconds sys

```


### DataRaces:

Versión clasica:

```c++
// Classical Matrix Multiply
template <class type>
void MM(const type* a, const type* b, type* c, const int N)
{

    for (int i = 0; i < N; i++)
        for (int j = 0; j < N; j++)
#pragma omp parallel for schedule (static)
            for (int k = 0; k < N; k++)
                c[i * N + j] += a[i * N + k] * b[k * N + j];
}
```

```shell

[aa-29@aolin-login practica3]$ perf stat -e cycles,instructions MMpar1 1024 0
Matrix Multiply 1024 x 1024 Classical
Result= -1582678602

 Performance counter stats for 'MMpar1 1024 0':

    69.518.186.761      cpu_core/cycles:u/
    13.084.183.937      cpu_core/instructions:u/

       1,485376626 seconds time elapsed

      17,472053000 seconds user
       0,170687000 seconds sys


```

Esto crea una situación en la que múltiples threads pueden actualizar la misma posición `c[i * N + j]` al mismo tiempo, provocando una condición de carrera.

Versión divide and conquer no sé como hacerlo


# EJERCICIO 3

Con n = 2048

Sin paralelismo
```shell
[aa-29@aolin-login practica3]$ g++ -Ofast -fopenmp -DTYPE=int -include MMcampus.h maincampus.cpp -o MMcv
[aa-29@aolin-login practica3]$ perf stat -e cycles,instructions MMcv 2048 0
Matrix Multiply 2048 x 2048 Classical
Result= 1426545896

 Performance counter stats for 'MMcv 2048 0':

    86.149.323.917      cpu_core/cycles:u/
    60.661.791.749      cpu_core/instructions:u/

      19,728805096 seconds time elapsed

IPC = 0,70
```

```shell
[aa-29@aolin-login practica3]$ perf stat -e cycles,instructions MMcv 2048 512
Matrix Multiply 2048 x 2048 Recursive up to 512 x 512 blocks
Result= 1426545896

 Performance counter stats for 'MMcv 2048 512':

    77.213.244.731      cpu_core/cycles:u/
    69.357.063.421      cpu_core/instructions:u/

      17,680457152 seconds time elapsed

      17,664697000 seconds user
       0,003994000 seconds sys
   IPC= 0,89

```

MMpar1

```shell
[aa-29@aolin-login practica3]$ g++ -Ofast -fopenmp -DTYPE=int -include MMpar1.h MM_main.cpp -o MMpar1
[aa-29@aolin-login practica3]$ perf stat -e cycles,instructions MMpar1 2048 0
Matrix Multiply 2048 x 2048 Classical
Result= 1426545896

 Performance counter stats for 'MMpar1 2048 0':

   123.847.040.770      cpu_core/cycles:u/
    69.266.815.970      cpu_core/instructions:u/

       2,741657211 seconds time elapsed

      31,069565000 seconds user
       0,008944000 seconds sys
   IPC = 0,55


[aa-29@aolin-login practica3]$ perf stat -e cycles,instructions MMpar1 2048 512
Matrix Multiply 2048 x 2048 Recursive up to 512 x 512 blocks
Result= 1426545896

 Performance counter stats for 'MMpar1 2048 512':

    78.218.199.855      cpu_core/cycles:u/
    69.415.814.432      cpu_core/instructions:u/

       4,796914204 seconds time elapsed

      18,793990000 seconds user
       0,017895000 seconds sys

IPC = 0,88

```
