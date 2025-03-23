Antes de modificar nada
```shell
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions HT2
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 1 threads
Hash[0]: key= 28256, count= 10
Hash[1]: key= 50384, count= 9
Hash[2]: key= 48694, count= 9
Hash[3]: key= 34817, count= 9
Hash[4]: key= 89619, count= 9
Hash[5]: key= 88802, count= 10
Hash[6]: key= 85644, count= 9
Ok
Size of NODE =24= 2x4 + 8 + 4

 Performance counter stats for 'HT2':

    49.371.680.956      cpu_core/cycles:u/
    26.842.939.544      cpu_core/instructions:u/

      11,332954495 seconds time elapsed

      11,319309000 seconds user
       0,002975000 seconds sys


[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=6
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions HT2
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 6 threads
Hash[0]: key= 28256, count= 10
Hash[1]: key= 50384, count= 9
Hash[2]: key= 48694, count= 9
Hash[3]: key= 34817, count= 9
Hash[4]: key= 89619, count= 9
Hash[5]: key= 88802, count= 10
Hash[6]: key= 85644, count= 9
Ok
Size of NODE =24= 2x4 + 8 + 4

 Performance counter stats for 'HT2':

   101.583.816.948      cpu_core/cycles:u/
    26.867.687.996      cpu_core/instructions:u/

       4,261438140 seconds time elapsed

      25,536772000 seconds user
       0,000980000 seconds sys


[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=12
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions HT2
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 12 threads
Hash[0]: key= 44253, count= 10
Hash[1]: key= 50384, count= 9
Hash[2]: key= 48694, count= 9
Hash[3]: key= 34817, count= 9
Hash[4]: key= 89619, count= 9
Hash[5]: key= 88802, count= 10
Hash[6]: key= 85644, count= 9
Ok
Size of NODE =24= 2x4 + 8 + 4

 Performance counter stats for 'HT2':

   110.214.920.642      cpu_core/cycles:u/
    26.918.553.870      cpu_core/instructions:u/

       2,312096001 seconds time elapsed

      27,670602000 seconds user
       0,003989000 seconds sys
```



version 2HT2

compilacion = `[aa-29@aolin-login PR5]$ g++ -ofast -fopenmp 2HT2.cpp -o 2HT2
`

```shell

[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=1
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions 2HT2
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 1 threads
Hash[0]: key= 28256, count= 10
Hash[1]: key= 50384, count= 9
Hash[2]: key= 48694, count= 9
Hash[3]: key= 34817, count= 9
Hash[4]: key= 89619, count= 9
Hash[5]: key= 88802, count= 10
Hash[6]: key= 85644, count= 9
Ok
Size of NODE =24= 2x4 + 8 + 4

 Performance counter stats for '2HT2':

    48.998.028.983      cpu_core/cycles:u/
    26.832.089.665      cpu_core/instructions:u/

      11,268770328 seconds time elapsed

      11,255239000 seconds user
       0,002998000 seconds sys


[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=6
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions 2HT2
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 6 threads
Hash[0]: key= 22017, count= 24
Hash[1]: key= 11170, count= 24
Hash[2]: key= 38110, count= 24
Hash[3]: key= 45984, count= 24
Hash[4]: key= 58566, count= 24
Hash[5]: key= 47454, count= 24
Hash[6]: key= 45957, count= 30
Ok
Size of NODE =24= 2x4 + 8 + 4

 Performance counter stats for '2HT2':

    26.680.814.582      cpu_core/cycles:u/
     7.184.789.378      cpu_core/instructions:u/

       1,121072889 seconds time elapsed

       6,702342000 seconds user
       0,002989000 seconds sys


[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=12
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions 2HT2
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 12 threads
Hash[0]: key= 22017, count= 48
Hash[1]: key= 62374, count= 36
Hash[2]: key= 43134, count= 36
Hash[3]: key= 42512, count= 36
Hash[4]: key= 51708, count= 36
Hash[5]: key= 68163, count= 36
Hash[6]: key= 68704, count= 36
Ok
Size of NODE =24= 2x4 + 8 + 4

 Performance counter stats for '2HT2':

    15.792.349.585      cpu_core/cycles:u/
     3.850.274.570      cpu_core/instructions:u/

       0,332762936 seconds time elapsed

       3,967085000 seconds user
       0,000956000 seconds sys


```