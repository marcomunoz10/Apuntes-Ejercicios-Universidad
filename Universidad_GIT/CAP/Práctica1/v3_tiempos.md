
| Threads | 1 Epoch | 10 Epochs | 100 Epochs | 1000 Epochs |
| ------- | ------- | --------- | ---------- | ----------- |
| 2       | 0.39 s  | 3.27 s    | 31.15 s    | 319.29 s    |
| 4       | 0.23 s  | 1.80 s    | 17.35 s    | 173.07 s    |
| 6       | 0.20 s  | 1.47 s    | 14.21 s    | 141.44 s    |
| 8       | 0.19 s  | 1.36 s    | 13.12 s    | 129.97 s    |
| 10      | 0.19 s  | 1.38 s    | 13.35 s    | 132.26 s    |
| 12      | 0.20 s  | 1.39 s    | 13.35 s    | 143.25 s    |
| epoch   | 787     | 881       | 908        | 911         |
# epoch 1000 num encerts 911

```shell
Programa amb  2  threads.

Training...

Testing...

Total encerts = 911


Goodbye! (319.289879 sec)


 Performance counter stats for './exec':

        638.548,25 msec task-clock:u                     #    2,000 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            14.249      page-faults:u                    #   22,315 /sec
 1.624.700.003.936      cycles:u                         #    2,544 GHz                         (66,67%)
    42.180.370.259      stalled-cycles-frontend:u        #    2,60% frontend cycles idle        (66,67%)
   552.418.287.619      stalled-cycles-backend:u         #   34,00% backend cycles idle         (66,67%)
 2.717.021.675.790      instructions:u                   #    1,67  insn per cycle
                                                  #    0,20  stalled cycles per insn     (66,67%)
   422.866.459.476      branches:u                       #  662,231 M/sec                       (66,67%)
     1.166.795.655      branch-misses:u                  #    0,28% of all branches             (66,67%)

     319,302925821 seconds time elapsed

     626,515141000 seconds user
       6,065012000 seconds sys




Programa amb  4  threads.

Training...

Testing...

Total encerts = 911


Goodbye! (173.073723 sec)


 Performance counter stats for './exec':

        692.208,67 msec task-clock:u                     #    3,999 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            15.633      page-faults:u                    #   22,584 /sec
 1.760.391.132.116      cycles:u                         #    2,543 GHz                         (66,67%)
    59.009.009.261      stalled-cycles-frontend:u        #    3,35% frontend cycles idle        (66,67%)
   611.726.695.197      stalled-cycles-backend:u         #   34,75% backend cycles idle         (66,67%)
 2.797.241.855.466      instructions:u                   #    1,59  insn per cycle
                                                  #    0,22  stalled cycles per insn     (66,67%)
   443.898.810.748      branches:u                       #  641,279 M/sec                       (66,67%)
     1.281.919.241      branch-misses:u                  #    0,29% of all branches             (66,67%)

     173,085591725 seconds time elapsed

     678,068793000 seconds user
       7,836587000 seconds sys




Programa amb  6  threads.

Training...

Testing...

Total encerts = 911


Goodbye! (141.449857 sec)


 Performance counter stats for './exec':

        848.548,80 msec task-clock:u                     #    5,998 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            22.004      page-faults:u                    #   25,931 /sec
 2.160.782.229.798      cycles:u                         #    2,546 GHz                         (66,66%)
    80.703.633.430      stalled-cycles-frontend:u        #    3,73% frontend cycles idle        (66,67%)
   881.388.907.701      stalled-cycles-backend:u         #   40,79% backend cycles idle         (66,67%)
 2.945.592.795.585      instructions:u                   #    1,36  insn per cycle
                                                  #    0,30  stalled cycles per insn     (66,66%)
   484.151.719.428      branches:u                       #  570,564 M/sec                       (66,67%)
     1.384.660.226      branch-misses:u                  #    0,29% of all branches             (66,67%)

     141,461982306 seconds time elapsed

     832,300556000 seconds user
       8,758831000 seconds sys




Programa amb  8  threads.

Training...

Testing...

Total encerts = 911


Goodbye! (129.970724 sec)


 Performance counter stats for './exec':

      1.039.554,07 msec task-clock:u                     #    7,998 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            24.176      page-faults:u                    #   23,256 /sec
 2.650.162.914.458      cycles:u                         #    2,549 GHz                         (66,66%)
   107.194.484.518      stalled-cycles-frontend:u        #    4,04% frontend cycles idle        (66,66%)
 1.208.744.086.174      stalled-cycles-backend:u         #   45,61% backend cycles idle         (66,67%)
 3.114.888.864.543      instructions:u                   #    1,18  insn per cycle
                                                  #    0,39  stalled cycles per insn     (66,67%)
   531.108.234.979      branches:u                       #  510,900 M/sec                       (66,67%)
     1.475.369.218      branch-misses:u                  #    0,28% of all branches             (66,67%)

     129,982649627 seconds time elapsed

    1020,452213000 seconds user
      10,164283000 seconds sys




Programa amb  10  threads.

Training...

Testing...

Total encerts = 911


Goodbye! (132.267286 sec)


 Performance counter stats for './exec':

      1.322.399,70 msec task-clock:u                     #    9,997 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            34.436      page-faults:u                    #   26,041 /sec
 3.375.583.641.910      cycles:u                         #    2,553 GHz                         (66,67%)
   151.219.498.195      stalled-cycles-frontend:u        #    4,48% frontend cycles idle        (66,67%)
 1.692.684.650.964      stalled-cycles-backend:u         #   50,14% backend cycles idle         (66,66%)
 3.396.940.636.098      instructions:u                   #    1,01  insn per cycle
                                                  #    0,50  stalled cycles per insn     (66,67%)
   609.667.927.496      branches:u                       #  461,032 M/sec                       (66,67%)
     1.608.671.900      branch-misses:u                  #    0,26% of all branches             (66,67%)

     132,278714757 seconds time elapsed

    1299,648747000 seconds user
      11,320153000 seconds sys




Programa amb  12  threads.

Training...

Testing...

Total encerts = 911


Goodbye! (143.254601 sec)


 Performance counter stats for './exec':

      1.706.435,23 msec task-clock:u                     #   11,911 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            89.652      page-faults:u                    #   52,538 /sec
 4.364.232.862.608      cycles:u                         #    2,558 GHz                         (66,67%)
   236.775.019.847      stalled-cycles-frontend:u        #    5,43% frontend cycles idle        (66,67%)
 2.102.459.669.119      stalled-cycles-backend:u         #   48,17% backend cycles idle         (66,67%)
 3.962.756.616.636      instructions:u                   #    0,91  insn per cycle
                                                  #    0,53  stalled cycles per insn     (66,67%)
   769.184.148.636      branches:u                       #  450,755 M/sec                       (66,67%)
     1.716.201.220      branch-misses:u                  #    0,22% of all branches             (66,67%)

     143,266753770 seconds time elapsed

    1680,332782000 seconds user
      12,850356000 seconds sys

```


# Epoch 100 num encerts 908

```c
Programa amb  2  threads.

Training...

Testing...

Total encerts = 908


Goodbye! (31.153865 sec)


 Performance counter stats for './exec':

         62.284,75 msec task-clock:u                     #    1,998 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             5.786      page-faults:u                    #   92,896 /sec
   158.357.133.565      cycles:u                         #    2,542 GHz                         (66,66%)
     4.396.420.609      stalled-cycles-frontend:u        #    2,78% frontend cycles idle        (66,67%)
    50.949.012.206      stalled-cycles-backend:u         #   32,17% backend cycles idle         (66,67%)
   271.849.238.020      instructions:u                   #    1,72  insn per cycle
                                                  #    0,19  stalled cycles per insn     (66,67%)
    42.295.980.216      branches:u                       #  679,074 M/sec                       (66,67%)
       115.777.350      branch-misses:u                  #    0,27% of all branches             (66,67%)

      31,166989953 seconds time elapsed

      60,981476000 seconds user
       0,700174000 seconds sys




Programa amb  4  threads.

Training...

Testing...

Total encerts = 908


Goodbye! (17.346579 sec)


 Performance counter stats for './exec':

         69.303,63 msec task-clock:u                     #    3,992 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             9.545      page-faults:u                    #  137,727 /sec
   176.237.218.315      cycles:u                         #    2,543 GHz                         (66,66%)
     5.871.616.982      stalled-cycles-frontend:u        #    3,33% frontend cycles idle        (66,66%)
    61.210.425.026      stalled-cycles-backend:u         #   34,73% backend cycles idle         (66,67%)
   280.170.783.560      instructions:u                   #    1,59  insn per cycle
                                                  #    0,22  stalled cycles per insn     (66,67%)
    44.472.934.302      branches:u                       #  641,711 M/sec                       (66,67%)
       127.396.117      branch-misses:u                  #    0,29% of all branches             (66,66%)

      17,358959974 seconds time elapsed

      67,893294000 seconds user
       0,797573000 seconds sys




Programa amb  6  threads.

Training...

Testing...

Total encerts = 908


Goodbye! (14.214994 sec)


 Performance counter stats for './exec':

         85.154,67 msec task-clock:u                     #    5,986 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             9.973      page-faults:u                    #  117,116 /sec
   216.777.596.921      cycles:u                         #    2,546 GHz                         (66,67%)
     8.123.716.714      stalled-cycles-frontend:u        #    3,75% frontend cycles idle        (66,66%)
    88.369.643.605      stalled-cycles-backend:u         #   40,77% backend cycles idle         (66,65%)
   295.518.450.911      instructions:u                   #    1,36  insn per cycle
                                                  #    0,30  stalled cycles per insn     (66,67%)
    48.632.022.545      branches:u                       #  571,102 M/sec                       (66,68%)
       137.820.917      branch-misses:u                  #    0,28% of all branches             (66,67%)

      14,226805380 seconds time elapsed

      83,530448000 seconds user
       0,875616000 seconds sys




Programa amb  8  threads.

Training...

Testing...

Total encerts = 908


Goodbye! (13.121192 sec)


 Performance counter stats for './exec':

        104.642,96 msec task-clock:u                     #    7,968 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            13.152      page-faults:u                    #  125,685 /sec
   266.604.602.198      cycles:u                         #    2,548 GHz                         (66,66%)
    10.941.912.536      stalled-cycles-frontend:u        #    4,10% frontend cycles idle        (66,67%)
   121.484.794.316      stalled-cycles-backend:u         #   45,57% backend cycles idle         (66,68%)
   313.909.964.343      instructions:u                   #    1,18  insn per cycle
                                                  #    0,39  stalled cycles per insn     (66,68%)
    53.780.671.465      branches:u                       #  513,944 M/sec                       (66,67%)
       147.243.282      branch-misses:u                  #    0,27% of all branches             (66,65%)

      13,132906021 seconds time elapsed

     102,664429000 seconds user
       1,074163000 seconds sys




Programa amb  10  threads.

Training...

Testing...

Total encerts = 908


Goodbye! (13.359765 sec)


 Performance counter stats for './exec':

        133.232,18 msec task-clock:u                     #    9,964 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            15.059      page-faults:u                    #  113,028 /sec
   340.031.834.979      cycles:u                         #    2,552 GHz                         (66,66%)
    15.208.471.639      stalled-cycles-frontend:u        #    4,47% frontend cycles idle        (66,66%)
   168.662.642.020      stalled-cycles-backend:u         #   49,60% backend cycles idle         (66,68%)
   339.548.134.659      instructions:u                   #    1,00  insn per cycle
                                                  #    0,50  stalled cycles per insn     (66,68%)
    60.899.797.904      branches:u                       #  457,095 M/sec                       (66,66%)
       159.320.383      branch-misses:u                  #    0,26% of all branches             (66,66%)

      13,371573097 seconds time elapsed

     130,946075000 seconds user
       1,194226000 seconds sys




Programa amb  12  threads.

Training...

Testing...

Total encerts = 908


Goodbye! (13.350227 sec)


 Performance counter stats for './exec':

        159.626,43 msec task-clock:u                     #   11,946 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            23.026      page-faults:u                    #  144,249 /sec
   408.214.464.489      cycles:u                         #    2,557 GHz                         (66,66%)
    20.082.549.844      stalled-cycles-frontend:u        #    4,92% frontend cycles idle        (66,67%)
   205.321.742.994      stalled-cycles-backend:u         #   50,30% backend cycles idle         (66,67%)
   372.094.135.528      instructions:u                   #    0,91  insn per cycle
                                                  #    0,55  stalled cycles per insn     (66,67%)
    69.975.781.421      branches:u                       #  438,372 M/sec                       (66,67%)
       170.930.611      branch-misses:u                  #    0,24% of all branches             (66,67%)

      13,361982185 seconds time elapsed

     157,186569000 seconds user
       1,208760000 seconds sys

```


# Epoch 10 num encerts 881

```c
[cap-30@clus-login openmp-torn2_grup6]$ cat resultat.txt
Programa amb  2  threads.

Training...

Testing...

Total encerts = 881


Goodbye! (3.259860 sec)


 Performance counter stats for './exec':

          6.496,70 msec task-clock:u                     #    1,985 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             3.340      page-faults:u                    #  514,108 /sec
    16.480.596.894      cycles:u                         #    2,537 GHz                         (66,64%)
       454.400.504      stalled-cycles-frontend:u        #    2,76% frontend cycles idle        (66,67%)
     5.482.746.588      stalled-cycles-backend:u         #   33,27% backend cycles idle         (66,67%)
    27.687.206.137      instructions:u                   #    1,68  insn per cycle
                                                  #    0,20  stalled cycles per insn     (66,68%)
     4.334.562.691      branches:u                       #  667,195 M/sec                       (66,70%)
        12.595.367      branch-misses:u                  #    0,29% of all branches             (66,64%)

       3,273301763 seconds time elapsed

       6,359087000 seconds user
       0,073231000 seconds sys




Programa amb  4  threads.

Training...

Testing...

Total encerts = 881


Goodbye! (1.787317 sec)


 Performance counter stats for './exec':

          7.075,62 msec task-clock:u                     #    3,933 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             3.060      page-faults:u                    #  432,471 /sec
    17.968.821.309      cycles:u                         #    2,540 GHz                         (66,67%)
       599.599.280      stalled-cycles-frontend:u        #    3,34% frontend cycles idle        (66,64%)
     6.308.482.636      stalled-cycles-backend:u         #   35,11% backend cycles idle         (66,62%)
    28.346.514.006      instructions:u                   #    1,58  insn per cycle
                                                  #    0,22  stalled cycles per insn     (66,68%)
     4.508.271.728      branches:u                       #  637,156 M/sec                       (66,71%)
        13.135.980      branch-misses:u                  #    0,29% of all branches             (66,69%)

       1,798890091 seconds time elapsed

       6,904627000 seconds user
       0,105811000 seconds sys




Programa amb  6  threads.

Training...

Testing...

Total encerts = 881


Goodbye! (1.462815 sec)


 Performance counter stats for './exec':

          8.652,16 msec task-clock:u                     #    5,866 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             3.896      page-faults:u                    #  450,292 /sec
    21.993.678.896      cycles:u                         #    2,542 GHz                         (66,69%)
       820.298.700      stalled-cycles-frontend:u        #    3,73% frontend cycles idle        (66,70%)
     8.980.568.206      stalled-cycles-backend:u         #   40,83% backend cycles idle         (66,63%)
    29.900.369.024      instructions:u                   #    1,36  insn per cycle
                                                  #    0,30  stalled cycles per insn     (66,61%)
     4.920.630.455      branches:u                       #  568,717 M/sec                       (66,68%)
        14.101.480      branch-misses:u                  #    0,29% of all branches             (66,70%)

       1,474940866 seconds time elapsed

       8,474879000 seconds user
       0,098799000 seconds sys




Programa amb  8  threads.

Training...

Testing...

Total encerts = 881


Goodbye! (1.353168 sec)


 Performance counter stats for './exec':

         10.645,25 msec task-clock:u                     #    7,800 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             4.838      page-faults:u                    #  454,475 /sec
    27.051.862.793      cycles:u                         #    2,541 GHz                         (66,67%)
     1.136.338.111      stalled-cycles-frontend:u        #    4,20% frontend cycles idle        (66,67%)
    12.329.158.450      stalled-cycles-backend:u         #   45,58% backend cycles idle         (66,67%)
    31.788.190.840      instructions:u                   #    1,18  insn per cycle
                                                  #    0,39  stalled cycles per insn     (66,67%)
     5.458.739.775      branches:u                       #  512,786 M/sec                       (66,68%)
        15.143.803      branch-misses:u                  #    0,28% of all branches             (66,68%)

       1,364796458 seconds time elapsed

      10,436552000 seconds user
       0,112592000 seconds sys




Programa amb  10  threads.

Training...

Testing...

Total encerts = 881


Goodbye! (1.366155 sec)


 Performance counter stats for './exec':

         13.425,45 msec task-clock:u                     #    9,748 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             5.787      page-faults:u                    #  431,047 /sec
    34.189.549.692      cycles:u                         #    2,547 GHz                         (66,63%)
     1.535.512.617      stalled-cycles-frontend:u        #    4,49% frontend cycles idle        (66,66%)
    17.107.375.245      stalled-cycles-backend:u         #   50,04% backend cycles idle         (66,71%)
    34.458.751.293      instructions:u                   #    1,01  insn per cycle
                                                  #    0,50  stalled cycles per insn     (66,71%)
     6.197.268.888      branches:u                       #  461,606 M/sec                       (66,70%)
        16.391.960      branch-misses:u                  #    0,26% of all branches             (66,65%)

       1,377266912 seconds time elapsed

      13,142721000 seconds user
       0,169429000 seconds sys




Programa amb  12  threads.

Training...

Testing...

Total encerts = 881


Goodbye! (1.379139 sec)


 Performance counter stats for './exec':

         16.250,00 msec task-clock:u                     #   11,686 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             6.477      page-faults:u                    #  398,585 /sec
    41.474.656.364      cycles:u                         #    2,552 GHz                         (66,65%)
     2.089.010.242      stalled-cycles-frontend:u        #    5,04% frontend cycles idle        (66,69%)
    21.166.906.058      stalled-cycles-backend:u         #   51,04% backend cycles idle         (66,70%)
    38.119.257.212      instructions:u                   #    0,92  insn per cycle
                                                  #    0,56  stalled cycles per insn     (66,70%)
     7.222.963.256      branches:u                       #  444,490 M/sec                       (66,67%)
        17.522.697      branch-misses:u                  #    0,24% of all branches             (66,63%)

       1,390600307 seconds time elapsed

      15,978030000 seconds user
       0,145508000 seconds sys



```

# Epoch 1 num encerts 787

```c
[cap-30@clus-login openmp-torn2_grup6]$ cat resultat.txt
Programa amb  2  threads.

Training...

Testing...

Total encerts = 787


Goodbye! (0.376084 sec)


 Performance counter stats for './exec':

            730,21 msec task-clock:u                     #    1,875 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
               891      page-faults:u                    #    1,220 K/sec
     1.845.378.240      cycles:u                         #    2,527 GHz                         (66,55%)
        46.748.213      stalled-cycles-frontend:u        #    2,53% frontend cycles idle        (66,62%)
       638.128.672      stalled-cycles-backend:u         #   34,58% backend cycles idle         (66,76%)
     3.080.289.670      instructions:u                   #    1,67  insn per cycle
                                                  #    0,21  stalled cycles per insn     (66,89%)
       484.996.152      branches:u                       #  664,186 M/sec                       (66,72%)
         1.546.614      branch-misses:u                  #    0,32% of all branches             (66,52%)

       0,389473712 seconds time elapsed

       0,708227000 seconds user
       0,015846000 seconds sys




Programa amb  4  threads.

Training...

Testing...

Total encerts = 787


Goodbye! (0.221980 sec)


 Performance counter stats for './exec':

            813,65 msec task-clock:u                     #    3,484 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
               900      page-faults:u                    #    1,106 K/sec
     2.058.831.532      cycles:u                         #    2,530 GHz                         (66,78%)
        66.884.532      stalled-cycles-frontend:u        #    3,25% frontend cycles idle        (66,27%)
       741.193.981      stalled-cycles-backend:u         #   36,00% backend cycles idle         (66,32%)
     3.186.491.464      instructions:u                   #    1,55  insn per cycle
                                                  #    0,23  stalled cycles per insn     (66,98%)
       515.665.930      branches:u                       #  633,765 M/sec                       (66,98%)
         1.638.167      branch-misses:u                  #    0,32% of all branches             (66,92%)

       0,233507168 seconds time elapsed

       0,783260000 seconds user
       0,023635000 seconds sys




Programa amb  6  threads.

Training...

Testing...

Total encerts = 787


Goodbye! (0.189012 sec)


 Performance counter stats for './exec':

          1.006,17 msec task-clock:u                     #    5,017 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
               911      page-faults:u                    #  905,417 /sec
     2.544.497.620      cycles:u                         #    2,529 GHz                         (66,61%)
        92.457.036      stalled-cycles-frontend:u        #    3,63% frontend cycles idle        (66,64%)
     1.057.734.910      stalled-cycles-backend:u         #   41,57% backend cycles idle         (66,82%)
     3.365.555.913      instructions:u                   #    1,32  insn per cycle
                                                  #    0,31  stalled cycles per insn     (66,81%)
       565.007.623      branches:u                       #  561,545 M/sec                       (66,80%)
         1.743.359      branch-misses:u                  #    0,31% of all branches             (66,66%)

       0,200536299 seconds time elapsed

       0,977888000 seconds user
       0,019736000 seconds sys




Programa amb  8  threads.

Training...

Testing...

Total encerts = 787


Goodbye! (0.177420 sec)


 Performance counter stats for './exec':

          1.235,62 msec task-clock:u                     #    6,532 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
               921      page-faults:u                    #  745,372 /sec
     3.135.025.728      cycles:u                         #    2,537 GHz                         (66,62%)
       121.086.053      stalled-cycles-frontend:u        #    3,86% frontend cycles idle        (66,78%)
     1.449.772.181      stalled-cycles-backend:u         #   46,24% backend cycles idle         (66,72%)
     3.563.894.238      instructions:u                   #    1,14  insn per cycle
                                                  #    0,41  stalled cycles per insn     (66,76%)
       621.628.757      branches:u                       #  503,089 M/sec                       (66,79%)
         1.867.998      branch-misses:u                  #    0,30% of all branches             (66,69%)

       0,189164806 seconds time elapsed

       1,205134000 seconds user
       0,020539000 seconds sys




Programa amb  10  threads.

Training...

Testing...

Total encerts = 787


Goodbye! (0.180787 sec)


 Performance counter stats for './exec':

          1.569,61 msec task-clock:u                     #    8,165 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
               932      page-faults:u                    #  593,779 /sec
     3.939.789.692      cycles:u                         #    2,510 GHz                         (66,51%)
       176.881.183      stalled-cycles-frontend:u        #    4,49% frontend cycles idle        (66,12%)
     2.026.750.255      stalled-cycles-backend:u         #   51,44% backend cycles idle         (66,31%)
     3.889.031.334      instructions:u                   #    0,99  insn per cycle
                                                  #    0,52  stalled cycles per insn     (66,96%)
       708.204.583      branches:u                       #  451,199 M/sec                       (67,42%)
         1.970.858      branch-misses:u                  #    0,28% of all branches             (67,14%)

       0,192236965 seconds time elapsed

       1,515596000 seconds user
       0,041396000 seconds sys




Programa amb  12  threads.

Training...

Testing...

Total encerts = 787


Goodbye! (0.183637 sec)


 Performance counter stats for './exec':

          1.909,93 msec task-clock:u                     #    9,784 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
               939      page-faults:u                    #  491,641 /sec
     4.826.347.883      cycles:u                         #    2,527 GHz                         (66,44%)
       242.086.601      stalled-cycles-frontend:u        #    5,02% frontend cycles idle        (66,67%)
     2.507.406.816      stalled-cycles-backend:u         #   51,95% backend cycles idle         (66,76%)
     4.307.856.842      instructions:u                   #    0,89  insn per cycle
                                                  #    0,58  stalled cycles per insn     (66,76%)
       830.720.859      branches:u                       #  434,949 M/sec                       (66,82%)
         2.115.162      branch-misses:u                  #    0,25% of all branches             (66,59%)

       0,195218779 seconds time elapsed

       1,870238000 seconds user
       0,025574000 seconds sys

```