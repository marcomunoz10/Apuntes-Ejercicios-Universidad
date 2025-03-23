### P2
```shell
AA-PR2: N= 500000, Rep= 2000
v= 2.40151358e+00, II[500000]= 2675700

 Performance counter stats for 'p2':

    61.059.813.784      cpu_core/cycles:u/
    15.796.040.892      cpu_core/instructions:u/

       1,286472917 seconds time elapsed

      15,338292000 seconds user
       0,009921000 seconds sys


```

### P3
```shell
[aa-29@aolin-login pr2]$ perf stat -e cycles,instructions p3
AA-PR2: N= 500000, Rep= 2000
v= 2.40151358e+00, II[500000]= 2675700

 Performance counter stats for 'p3':

    46.439.060.966      cpu_core/cycles:u/
    15.155.220.851      cpu_core/instructions:u/

       0,979128058 seconds time elapsed

      11,658650000 seconds user
       0,019911000 seconds sys


```


cambios: poner en *loop 5* `#pragma omp parallel for schedule(static)`

### P4

estoy probando cosas en loop 6
veo que hacer esto no me supone ningun cambio de tiempo:
```c++
double loop6(int* V, int n)
{
    double sum = 0;
    double N[n+1];

#pragma omp parallel for shared(V,n)
    for (int i = 1; i <= n; i++)
        N[i] =  1.0 / (1 + V[i]);

#pragma omp parallel for reduction(+:sum) 
    for (int i = 1; i <= n; i++)
        sum = sum + N[i];
    return sum;
}
```


### P8

```c++

```