```

[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=1
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions HT4
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 1 threads
Hash[0]: key= 98302, count= 1
Hash[1]: key= 98531, count= 1
Hash[2]: key= 12055, count= 1
Hash[3]: key= 24208, count= 1
Hash[4]: key= 96536, count= 1
Hash[5]: key= 2744, count= 1
Hash[6]: key= 88028, count= 1
Ok
Summary: Total errors found = 59494
Size of NODE =16= 2x4 + 8 + 4

 Performance counter stats for 'HT4':

    31.775.706.194      cpu_core/cycles:u/
    50.356.871.188      cpu_core/instructions:u/

       7,330817061 seconds time elapsed

       7,309394000 seconds user
       0,006718000 seconds sys


[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=6
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions HT4
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 6 threads
Hash[0]: key= 98302, count= 1
Hash[1]: key= 98531, count= 1
Hash[2]: key= 12055, count= 1
Hash[3]: key= 24208, count= 1
Hash[4]: key= 96536, count= 1
Hash[5]: key= 2744, count= 1
Hash[6]: key= 88028, count= 1
Ok
Summary: Total errors found = 28366
Size of NODE =16= 2x4 + 8 + 4

 Performance counter stats for 'HT4':

     4.611.097.744      cpu_core/cycles:u/
    13.292.882.167      cpu_core/instructions:u/

       0,195189951 seconds time elapsed

       1,160028000 seconds user
       0,001946000 seconds sys


[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=12
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions HT4
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 12 threads
Hash[0]: key= 98302, count= 1
Hash[1]: key= 98531, count= 1
Hash[2]: key= 12055, count= 1
Hash[3]: key= 24208, count= 1
Hash[4]: key= 96536, count= 1
Hash[5]: key= 2744, count= 1
Hash[6]: key= 88028, count= 1
Ok
Summary: Total errors found = 15353
Size of NODE =16= 2x4 + 8 + 4

 Performance counter stats for 'HT4':

     3.713.709.540      cpu_core/cycles:u/
     7.471.796.058      cpu_core/instructions:u/

       0,080480253 seconds time elapsed

       0,933109000 seconds user
       0,000992000 seconds sys


```