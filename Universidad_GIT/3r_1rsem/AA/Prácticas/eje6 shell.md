
```shell
[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=1
[aa-29@aolin-login PR5]$ perf stat HT3
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
Size of NODE =24= 2x4 + 8 + 4

 Performance counter stats for 'HT3':

          7.360,65 msec task-clock:u              #    1,000 CPUs utilized
                 0      context-switches:u        #    0,000 /sec
                 0      cpu-migrations:u          #    0,000 /sec
               804      page-faults:u             #  109,229 /sec
    32.043.163.511      cpu_core/cycles:u/        #    4,353 G/sec
     9.001.639.439      cpu_core/instructions:u/  #    1,223 G/sec
     1.636.626.250      cpu_core/branches:u/      #  222,348 M/sec
           322.488      cpu_core/branch-misses:u/ #   43,812 K/sec

       7,362172190 seconds time elapsed

       7,343278000 seconds user
       0,006965000 seconds sys


[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=6
[aa-29@aolin-login PR5]$ perf stat HT3
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 6 threads
Hash[0]: key= 11550, count= 6
Hash[1]: key= 11170, count= 4
Hash[2]: key= 52109, count= 2
Hash[3]: key= 24208, count= 2
Hash[4]: key= 96536, count= 6
Hash[5]: key= 37252, count= 5
Hash[6]: key= 41214, count= 3
Ok
Summary: Total errors found = 21498
Size of NODE =24= 2x4 + 8 + 4

 Performance counter stats for 'HT3':

          1.185,15 msec task-clock:u              #    5,959 CPUs utilized
                 0      context-switches:u        #    0,000 /sec
                 0      cpu-migrations:u          #    0,000 /sec
               365      page-faults:u             #  307,979 /sec
     4.709.627.267      cpu_core/cycles:u/        #    3,974 G/sec
     2.398.101.553      cpu_core/instructions:u/  #    2,023 G/sec
       436.329.763      cpu_core/branches:u/      #  368,165 M/sec
           319.963      cpu_core/branch-misses:u/ #  269,977 K/sec

       0,198874244 seconds time elapsed

       1,182704000 seconds user
       0,002001000 seconds sys


[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=12
[aa-29@aolin-login PR5]$ perf stat HT3
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 12 threads
Hash[0]: key= 49709, count= 4
Hash[1]: key= 13080, count= 5
Hash[2]: key= 8197, count= 5
Hash[3]: key= 92230, count= 3
Hash[4]: key= 91076, count= 2
Hash[5]: key= 85620, count= 2
Hash[6]: key= 4970, count= 3
Ok
Summary: Total errors found = 15353
Size of NODE =24= 2x4 + 8 + 4

 Performance counter stats for 'HT3':

            653,83 msec task-clock:u              #   11,652 CPUs utilized
                 0      context-switches:u        #    0,000 /sec
                 0      cpu-migrations:u          #    0,000 /sec
               285      page-faults:u             #  435,896 /sec
     2.597.307.642      cpu_core/cycles:u/        #    3,972 G/sec
     1.290.924.148      cpu_core/instructions:u/  #    1,974 G/sec
       235.448.445      cpu_core/branches:u/      #  360,109 M/sec
           438.344      cpu_core/branch-misses:u/ #  670,430 K/sec

       0,056111823 seconds time elapsed

       0,652839000 seconds user
       0,001001000 seconds sys

```

