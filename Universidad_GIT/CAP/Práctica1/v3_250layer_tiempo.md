
| Threads | Epoch 1 | Epoch 10 |
| ------- | ------- | -------- |
| 2       | 0,7325  | 6,3506   |
| 4       | 0,3486  | 2,8972   |
| 6       | 0,2728  | 2,1057   |
| 8       | 0,2472  | 1,8660   |
| 10      | 0,2364  | 1,7752   |
| 12      | 0,2355  | 1,9460   |

# 1 epoch encerts 810
```c

[cap-30@clus-login openmp-torn2_grup6]$ cat resultat.txt
Programa amb  2  threads.

Training...

Testing...

Total encerts = 810


Goodbye! (0.714958 sec)


 Performance counter stats for './exec':

          1.411,64 msec task-clock:u                     #    1,927 CPUs utilized
                 0      context-switches:u               #    0,000 /sec        
                 0      cpu-migrations:u                 #    0,000 /sec        
             1.125      page-faults:u                    #  796,945 /sec        
     3.595.905.411      cycles:u                         #    2,547 GHz                         (66,62%)
        64.470.993      stalled-cycles-frontend:u        #    1,79% frontend cycles idle        (66,59%)
     1.450.251.278      stalled-cycles-backend:u         #   40,33% backend cycles idle         (66,66%)
     5.563.247.452      instructions:u                   #    1,55  insn per cycle
                                                  #    0,26  stalled cycles per insn     (66,72%)
       868.373.500      branches:u                       #  615,152 M/sec                       (66,73%)
         2.385.575      branch-misses:u                  #    0,27% of all branches             (66,70%)

       0,732458709 seconds time elapsed

       1,383304000 seconds user
       0,014812000 seconds sys




Programa amb  4  threads.

Training...

Testing...

Total encerts = 810


Goodbye! (0.332830 sec)


 Performance counter stats for './exec':

          1.261,71 msec task-clock:u                     #    3,620 CPUs utilized
                 0      context-switches:u               #    0,000 /sec        
                 0      cpu-migrations:u                 #    0,000 /sec        
             1.134      page-faults:u                    #  898,779 /sec        
     3.206.668.103      cycles:u                         #    2,542 GHz                         (66,56%)
       104.664.538      stalled-cycles-frontend:u        #    3,26% frontend cycles idle        (66,70%)
       921.192.773      stalled-cycles-backend:u         #   28,73% backend cycles idle         (66,66%)
     5.708.558.033      instructions:u                   #    1,78  insn per cycle
                                                  #    0,16  stalled cycles per insn     (66,73%)
       902.479.620      branches:u                       #  715,282 M/sec                       (66,79%)
         2.589.567      branch-misses:u                  #    0,29% of all branches             (66,57%)

       0,348578918 seconds time elapsed

       1,235982000 seconds user
       0,013838000 seconds sys




Programa amb  6  threads.

Training...

Testing...

Total encerts = 810


Goodbye! (0.256681 sec)


 Performance counter stats for './exec':

          1.419,07 msec task-clock:u                     #    5,203 CPUs utilized
                 0      context-switches:u               #    0,000 /sec        
                 0      cpu-migrations:u                 #    0,000 /sec        
             1.144      page-faults:u                    #  806,159 /sec        
     3.603.847.136      cycles:u                         #    2,540 GHz                         (66,39%)
       128.147.204      stalled-cycles-frontend:u        #    3,56% frontend cycles idle        (66,69%)
     1.175.453.292      stalled-cycles-backend:u         #   32,62% backend cycles idle         (66,89%)
     5.832.744.855      instructions:u                   #    1,62  insn per cycle
                                                  #    0,20  stalled cycles per insn     (66,90%)
       947.077.489      branches:u                       #  667,391 M/sec                       (66,82%)
         2.641.134      branch-misses:u                  #    0,28% of all branches             (66,61%)

       0,272757019 seconds time elapsed

       1,382898000 seconds user
       0,024534000 seconds sys




Programa amb  8  threads.

Training...

Testing...

Total encerts = 810


Goodbye! (0.231267 sec)


 Performance counter stats for './exec':

          1.670,83 msec task-clock:u                     #    6,759 CPUs utilized
                 0      context-switches:u               #    0,000 /sec        
                 0      cpu-migrations:u                 #    0,000 /sec        
             1.154      page-faults:u                    #  690,676 /sec        
     4.232.201.121      cycles:u                         #    2,533 GHz                         (66,83%)
       165.647.690      stalled-cycles-frontend:u        #    3,91% frontend cycles idle        (66,73%)
     1.583.090.010      stalled-cycles-backend:u         #   37,41% backend cycles idle         (66,77%)
     6.073.943.627      instructions:u                   #    1,44  insn per cycle
                                                  #    0,26  stalled cycles per insn     (66,76%)
     1.016.208.189      branches:u                       #  608,207 M/sec                       (66,59%)
         2.772.804      branch-misses:u                  #    0,27% of all branches             (66,71%)

       0,247201895 seconds time elapsed

       1,623224000 seconds user
       0,034543000 seconds sys




Programa amb  10  threads.

Training...

Testing...

Total encerts = 810


Goodbye! (0.220273 sec)


 Performance counter stats for './exec':

          1.970,55 msec task-clock:u                     #    8,334 CPUs utilized
                 0      context-switches:u               #    0,000 /sec        
                 0      cpu-migrations:u                 #    0,000 /sec        
             1.175      page-faults:u                    #  596,279 /sec        
     5.013.573.253      cycles:u                         #    2,544 GHz                         (66,27%)
       199.733.833      stalled-cycles-frontend:u        #    3,98% frontend cycles idle        (66,49%)
     2.148.757.414      stalled-cycles-backend:u         #   42,86% backend cycles idle         (66,85%)
     6.299.637.998      instructions:u                   #    1,26  insn per cycle
                                                  #    0,34  stalled cycles per insn     (66,85%)
     1.076.352.504      branches:u                       #  546,219 M/sec                       (66,91%)
         2.825.169      branch-misses:u                  #    0,26% of all branches             (66,69%)

       0,236445567 seconds time elapsed

       1,915429000 seconds user
       0,038521000 seconds sys




Programa amb  12  threads.

Training...

Testing...

Total encerts = 810


Goodbye! (0.219451 sec)


 Performance counter stats for './exec':

          2.347,35 msec task-clock:u                     #    9,968 CPUs utilized
                 0      context-switches:u               #    0,000 /sec        
                 0      cpu-migrations:u                 #    0,000 /sec        
             1.181      page-faults:u                    #  503,121 /sec        
     5.922.290.716      cycles:u                         #    2,523 GHz                         (66,75%)
       261.524.558      stalled-cycles-frontend:u        #    4,42% frontend cycles idle        (66,80%)
     2.723.952.029      stalled-cycles-backend:u         #   45,99% backend cycles idle         (66,80%)
     6.811.040.739      instructions:u                   #    1,15  insn per cycle
                                                  #    0,40  stalled cycles per insn     (66,59%)
     1.193.810.607      branches:u                       #  508,578 M/sec                       (66,68%)
         3.025.967      branch-misses:u                  #    0,25% of all branches             (66,79%)

       0,235495401 seconds time elapsed

       2,290382000 seconds user
       0,039381000 seconds sys


```


# epoch 10 encerts 893
```c
[cap-30@clus-login openmp-torn2_grup6]$ cat resultat.txt
Programa amb  2  threads.

Training...

Testing...

Total encerts = 893


Goodbye! (6.333116 sec)


 Performance counter stats for './exec':

         12.647,14 msec task-clock:u                     #    1,991 CPUs utilized
                 0      context-switches:u               #    0,000 /sec        
                 0      cpu-migrations:u                 #    0,000 /sec        
             4.044      page-faults:u                    #  319,756 /sec        
    32.276.517.619      cycles:u                         #    2,552 GHz                         (66,66%)
       589.747.623      stalled-cycles-frontend:u        #    1,83% frontend cycles idle        (66,66%)
    12.823.504.531      stalled-cycles-backend:u         #   39,73% backend cycles idle         (66,67%)
    50.306.377.889      instructions:u                   #    1,56  insn per cycle
                                                  #    0,25  stalled cycles per insn     (66,68%)
     7.793.199.318      branches:u                       #  616,202 M/sec                       (66,67%)
        20.092.519      branch-misses:u                  #    0,26% of all branches             (66,66%)

       6,350594422 seconds time elapsed

      12,425540000 seconds user
       0,094030000 seconds sys




Programa amb  4  threads.

Training...

Testing...

Total encerts = 893


Goodbye! (2.880620 sec)


 Performance counter stats for './exec':

         11.452,06 msec task-clock:u                     #    3,953 CPUs utilized
                 0      context-switches:u               #    0,000 /sec        
                 0      cpu-migrations:u                 #    0,000 /sec        
             3.467      page-faults:u                    #  302,740 /sec        
    29.180.635.000      cycles:u                         #    2,548 GHz                         (66,64%)
       982.821.006      stalled-cycles-frontend:u        #    3,37% frontend cycles idle        (66,67%)
     8.163.880.355      stalled-cycles-backend:u         #   27,98% backend cycles idle         (66,69%)
    51.792.916.589      instructions:u                   #    1,77  insn per cycle
                                                  #    0,16  stalled cycles per insn     (66,68%)
     8.194.803.215      branches:u                       #  715,575 M/sec                       (66,68%)
        21.477.993      branch-misses:u                  #    0,26% of all branches             (66,66%)

       2,897182215 seconds time elapsed

      11,186660000 seconds user
       0,151130000 seconds sys




Programa amb  6  threads.

Training...

Testing...

Total encerts = 893


Goodbye! (2.089586 sec)


 Performance counter stats for './exec':

         12.415,36 msec task-clock:u                     #    5,896 CPUs utilized
                 0      context-switches:u               #    0,000 /sec        
                 0      cpu-migrations:u                 #    0,000 /sec        
             4.404      page-faults:u                    #  354,722 /sec        
    31.622.344.998      cycles:u                         #    2,547 GHz                         (66,66%)
     1.167.936.240      stalled-cycles-frontend:u        #    3,69% frontend cycles idle        (66,64%)
     9.768.159.755      stalled-cycles-backend:u         #   30,89% backend cycles idle         (66,66%)
    52.805.758.952      instructions:u                   #    1,67  insn per cycle
                                                  #    0,18  stalled cycles per insn     (66,69%)
     8.461.284.245      branches:u                       #  681,517 M/sec                       (66,69%)
        21.901.076      branch-misses:u                  #    0,26% of all branches             (66,68%)

       2,105721505 seconds time elapsed

      12,185327000 seconds user
       0,113597000 seconds sys




Programa amb  8  threads.

Training...

Testing...

Total encerts = 893


Goodbye! (1.849620 sec)


 Performance counter stats for './exec':

         14.624,65 msec task-clock:u                     #    7,837 CPUs utilized
                 0      context-switches:u               #    0,000 /sec        
                 0      cpu-migrations:u                 #    0,000 /sec        
             5.263      page-faults:u                    #  359,872 /sec        
    37.247.659.721      cycles:u                         #    2,547 GHz                         (66,66%)
     1.487.806.840      stalled-cycles-frontend:u        #    3,99% frontend cycles idle        (66,66%)
    13.391.168.146      stalled-cycles-backend:u         #   35,95% backend cycles idle         (66,69%)
    55.111.015.076      instructions:u                   #    1,48  insn per cycle
                                                  #    0,24  stalled cycles per insn     (66,67%)
     9.101.961.192      branches:u                       #  622,371 M/sec                       (66,67%)
        22.706.855      branch-misses:u                  #    0,25% of all branches             (66,69%)

       1,866008632 seconds time elapsed

      14,336722000 seconds user
       0,157884000 seconds sys




Programa amb  10  threads.

Training...

Testing...

Total encerts = 893


Goodbye! (1.759434 sec)


 Performance counter stats for './exec':

         17.362,70 msec task-clock:u                     #    9,781 CPUs utilized
                 0      context-switches:u               #    0,000 /sec        
                 0      cpu-migrations:u                 #    0,000 /sec        
             6.150      page-faults:u                    #  354,208 /sec        
    44.335.166.916      cycles:u                         #    2,553 GHz                         (66,66%)
     1.835.381.367      stalled-cycles-frontend:u        #    4,14% frontend cycles idle        (66,67%)
    18.419.225.472      stalled-cycles-backend:u         #   41,55% backend cycles idle         (66,67%)
    57.329.440.875      instructions:u                   #    1,29  insn per cycle
                                                  #    0,32  stalled cycles per insn     (66,65%)
     9.717.506.622      branches:u                       #  559,677 M/sec                       (66,70%)
        23.871.474      branch-misses:u                  #    0,25% of all branches             (66,70%)

       1,775225292 seconds time elapsed

      17,053527000 seconds user
       0,166885000 seconds sys




Programa amb  12  threads.

Training...

Testing...

Total encerts = 893


Goodbye! (1.930303 sec)


 Performance counter stats for './exec':

         22.039,56 msec task-clock:u                     #   11,326 CPUs utilized
                 0      context-switches:u               #    0,000 /sec        
                 0      cpu-migrations:u                 #    0,000 /sec        
             6.995      page-faults:u                    #  317,384 /sec        
    56.313.737.941      cycles:u                         #    2,555 GHz                         (66,67%)
     2.972.182.476      stalled-cycles-frontend:u        #    5,28% frontend cycles idle        (66,67%)
    22.830.728.649      stalled-cycles-backend:u         #   40,54% backend cycles idle         (66,68%)
    65.146.227.438      instructions:u                   #    1,16  insn per cycle
                                                  #    0,35  stalled cycles per insn     (66,68%)
    11.952.937.316      branches:u                       #  542,340 M/sec                       (66,67%)
        24.914.023      branch-misses:u                  #    0,21% of all branches             (66,68%)

       1,945980842 seconds time elapsed

      21,671341000 seconds user
       0,193707000 seconds sys




```