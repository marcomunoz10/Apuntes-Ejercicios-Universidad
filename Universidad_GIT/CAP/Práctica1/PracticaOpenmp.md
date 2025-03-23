


|**Threads**|**Tiempo (10 Epochs)**|**Tiempo (100 Epochs)**|
|---|---|---|
|2|7.828264|76.982920|
|4|4.249593|41.719038|
|6|3.226601|31.344605|
|8|2.664625|26.041493|
|10|2.846885|28.052988|
|12|2.877581|28.066408|

## main.c
```c
/**
 *  main.c
 *
 *  Arxiu reutilitzat de l'assignatura de Computació d'Altes Prestacions de
 *  l'Escola d'Enginyeria de la Universitat Autònoma de Barcelona Created on: 31
 *  gen. 2019 Last modified: fall 24 (curs 24-25) Author: ecesar, asikora
 *  Modified: Blanca Llauradó, Christian Germer
 *
 *  Descripció:
 *  Funció que entrena la xarxa neuronal definida + Funció que fa el test del
 *  model entrenat + programa principal.
 *
 */

#include "main.h"

#include <fcntl.h>
#include <limits.h>
#include <math.h>
#include <stdio.h>
#include <stdlib.h>
#include <sys/time.h>

#include <omp.h>
//-----------FREE INPUT------------
void freeInput(int np, char** input) {
    for (int i = 0; i < np; i++)
        free(input[i]);
    free(input);
}

//-----------PRINTRECOGNIZED------------
void printRecognized(int p, layer Output) {
    int imax = 0;

    for (int i = 1; i < num_out_layer; i++)
        if (Output.actv[i] > Output.actv[imax])
            imax = i;

    if (imax == Validation[p])
        total++;

    if (debug == 1) {
        printf("El patró %d sembla un %c\t i és un %d", p, '0' + imax,
               Validation[p]);
        for (int k = 0; k < num_out_layer; k++)
            printf("\t%f\t", Output.actv[k]);
        printf("\n");
    }
}

/**
 * @brief Entrena la xarxa neuronal en base al conjunt d'entrenament
 *
 * @details Primer carrega tots els patrons d'entrenament (loadPatternSet)
 *          Després realitza num_epochs iteracions d'entrenament.
 *          Cada epoch fa:
 *              - Determina aleatòriament l'ordre en que es consideraran els
 * patrons (per evitar overfitting)
 *              - Per cada patró d'entrenament fa el forward_prop (reconeixament
 * del patró pel model actual) i el back_prop i update_weights (ajustament de
 * pesos i biaxos per provar de millorar la precisió del model)
 * 
 * forward_propagation calcula l'activació de cada neurona del hidden i del output
 * back_prop calcula els errors dels pesos
 *
 *
 * @see loadPatternSet, feed_input, forward_prop, back_prop, update_weights,
 * freeInput
 *
 */
void train_neural_net() {
    printf("\nTraining...\n");

    if ((input = loadPatternSet(num_training_patterns, dataset_training_path,
                                1)) == NULL) {
        printf("Loading Patterns: Error!!\n");
        exit(-1);
    }

    int ranpat[num_training_patterns];

    #pragma omp parallel
    {
    
    // Gradient Descent
    for (int it = 0; it < num_epochs; it++) {
        // Train patterns randomly
 
        #pragma omp for
        for (int p = 0; p < num_training_patterns; p++)
            {
                ranpat[p] = p;
            }
            
        #pragma omp single    
        for (int p = 0; p < num_training_patterns; p++) {
            int x = rando();
            int np = (x * x) % num_training_patterns;
            int op = ranpat[p];
            ranpat[p] = ranpat[np];
            ranpat[np] = op;
        }

        for (int i = 0; i < num_training_patterns; i++) {
            int p = ranpat[i];

            feed_input(p);;
            forward_prop();
            back_prop(p);
            update_weights();
        }
    }
}
freeInput(num_training_patterns, input);
}


//-----------TEST THE TRAINED NETWORK------------
void test_nn() {
    char** rSet;

    printf("\nTesting...\n");
    fflush(stdout);
    if ((rSet = loadPatternSet(num_test_patterns, dataset_test_path, 0)) ==
        NULL) {
        printf("Error!!\n");
        exit(-1);
    }

    #pragma omp parallel
    {
    for (int i = 0; i < num_test_patterns; i++) 
    {
        #pragma omp for
        for (int j = 0; j < num_neurons[0]; j++)
        {
            lay[0].actv[j] = rSet[i][j];
        }
        forward_prop();
        #pragma omp single
        {
        printRecognized(i, lay[num_layers - 1]);
        }
    }
    }
	float accuracy = (float) total / num_test_patterns;
//	printf("\nTotal encerts = %d, Accuracy = %.3f\n", total, accuracy);
   	printf("\nTotal encerts = %d\n", total);
    	freeInput(num_test_patterns, rSet);
}

//-----------MAIN-----------//
int main(int argc, char** argv) {
    if (debug == 1)
        printf("argc = %d \n", argc);
    if (argc <= 1)
        readConfiguration("configuration/configfile.txt");
    else
        readConfiguration(argv[1]);

    if (debug == 1)
        printf("FINISH CONFIG \n");

    // Initialize the neural network module
    if (init() != SUCCESS_INIT) {
        printf("Error in Initialization...\n");
        exit(0);
    }

    // Start measuring time
    struct timeval begin;
    gettimeofday(&begin, 0);

    // Train
    train_neural_net();

    // Test
    test_nn();

    // Stop measuring time and calculate the elapsed time
    double elapsed = elapsedT(begin);

    if (dinit() != SUCCESS_DINIT)
        printf("Error in Dinitialization...\n");

    printf("\n\nGoodbye! (%f sec)\n\n", elapsed);

    return 0;
}
```

## Training.c

```c
/*
 *  training.c
 *
 *  Arxiu reutilitzat de l'assignatura de Computació d'Altes Prestacions de l'Escola d'Enginyeria de la Universitat Autònoma de Barcelona
 *  Created on: 31 gen. 2019
 *  Last modified: fall 24 (curs 24-25)
 *  Author: ecesar, asikora
 *  Modified: Blanca Llauradó, Christian Germer
 *
 *  Descripció:
 *  Funcions per entrenar la xarxa neuronal.
 *
 */

#include "training.h"

#include <math.h>

/**
 * @brief Iniciatlitza la capa incial de la xarxa (input layer) amb l'entrada
 * que volem reconeixer.
 *
 * @param i Índex de l'element del conjunt d'entrenament que farem servir.
 **/
void feed_input(int i) 
{
    #pragma omp for
    for (int j = 0; j < num_neurons[0]; j++)
        lay[0].actv[j] = input[i][j];
}

/**
 * @brief Propagació dels valors de les neurones de l'entrada (valors a la input
 * layer) a la resta de capes de la xarxa fins a obtenir una predicció (sortida)
 *
 * @details La capa d'entrada (input layer = capa 0) ja ha estat inicialitzada
 * amb els valors de l'entrada que volem reconeixer. Així, el for més extern
 * (sobre i) recorre totes les capes de la xarxa a partir de la primera capa
 * hidden (capa 1). El for intern (sobre j) recorre les neurones de la capa i
 * calculant el seu valor d'activació [lay[i].actv[j]]. El valor d'activació de
 * cada neurona depén de l'exitació de la neurona calculada en el for més intern
 * (sobre k) [lay[i].z[j]]. El valor d'exitació s'inicialitza amb el biax de la
 * neurona corresponent [j] (lay[i].bias[j]) i es calcula multiplicant el valor
 * d'activació de les neurones de la capa anterior (i-1) pels pesos de
 * les connexions (out_weights) entre les dues capes. Finalment, el valor
 * d'activació de la neurona (j) es calcula fent servir la funció RELU
 * (REctified Linear Unit) si la capa (j) és una capa oculta (hidden) o la
 * funció Sigmoid si es tracte de la capa de sortida.
 *
 */
void forward_prop() {
    for (int i = 1; i < num_layers; i++) 
    {
        #pragma omp for
        for (int j = 0; j < num_neurons[i]; j++) {
            lay[i].z[j] = lay[i].bias[j];
            for (int k = 0; k < num_neurons[i - 1]; k++)
                lay[i].z[j] +=
                    ((lay[i - 1].out_weights[j * num_neurons[i - 1] + k]) *
                     (lay[i - 1].actv[k]));

            if (i <
                num_layers - 1)  // Relu Activation Function for Hidden Layers
                lay[i].actv[j] = ((lay[i].z[j]) < 0) ? 0 : lay[i].z[j];
            else  // Sigmoid Activation Function for Output Layer
                lay[i].actv[j] = 1 / (1 + exp(-lay[i].z[j]));
        }
    }
}

/**
 * @brief Calcula el gradient que es necessari aplicar als pesos de les
 * connexions entre neurones per corregir els errors de predicció
 *
 * @details Calcula dos vectors de correcció per cada capa de la xarxa, un per
 * corregir els pesos de les connexions de la neurona (j) amb la capa anterior
 *          (lay[i-1].dw[j]) i un segon per corregir el biax de cada neurona de
 * la capa actual (lay[i].bias[j]). Hi ha un tractament diferent per la capa de
 * sortida (num_layesr -1) perquè aquest és l'única cas en el que l'error es
 * conegut (lay[num_layers-1].actv[j] - desired_outputs[p][j]). Això es pot
 * veure en els dos primers fors. Per totes les capes ocultes (hidden layers) no
 * es pot saber el valor d'activació esperat per a cada neurona i per tant es fa
 * una estimació. Aquest càlcul es fa en el doble for que recorre totes les
 * capes ocultes (sobre i) neurona a neurona (sobre j). Es pot veure com en cada
 * cas es fa una estimació de quines hauríen de ser les activacions de les
 * neurones de la capa anterior (lay[i-1].dactv[k] = lay[i-1].out_weights[j*
 * num_neurons[i-1] + k] * lay[i].dz[j];), excepte pel cas de la capa d'entrada
 * (input layer) que és coneguda (imatge d'entrada).
 *
 */
void back_prop(int p) {
    // Output Layer
   

    #pragma omp for
    for (int j = 0; j < num_neurons[num_layers - 1]; j++) {
        lay[num_layers - 1].dz[j] =
            (lay[num_layers - 1].actv[j] - desired_outputs[p][j]) *
            (lay[num_layers - 1].actv[j]) * (1 - lay[num_layers - 1].actv[j]);
        lay[num_layers - 1].dbias[j] = lay[num_layers - 1].dz[j];
    }

    
    for (int j = 0; j < num_neurons[num_layers - 1]; j++) 
    {
        #pragma omp for
        for (int k = 0; k < num_neurons[num_layers - 2]; k++) {
            lay[num_layers - 2].dw[j * num_neurons[num_layers - 2] + k] =
                (lay[num_layers - 1].dz[j] * lay[num_layers - 2].actv[k]);
            lay[num_layers - 2].dactv[k] =
                lay[num_layers - 2]
                    .out_weights[j * num_neurons[num_layers - 2] + k] *
                lay[num_layers - 1].dz[j];
        }
    }

    // Hidden Layers
    for (int i = num_layers - 2; i > 0; i--) 
    {
        #pragma omp for
        for (int j = 0; j < num_neurons[i]; j++) {
            lay[i].dz[j] = (lay[i].z[j] >= 0) ? lay[i].dactv[j] : 0;

            for (int k = 0; k < num_neurons[i - 1]; k++) {
                lay[i - 1].dw[j * num_neurons[i - 1] + k] =
                    lay[i].dz[j] * lay[i - 1].actv[k];

                if (i > 1)
                    lay[i - 1].dactv[k] =
                        lay[i - 1].out_weights[j * num_neurons[i - 1] + k] *
                        lay[i].dz[j];
            }
            lay[i].dbias[j] = lay[i].dz[j];
        }
    }
}

/**
 * @brief Actualitza els vectors de pesos (out_weights) i de biax (bias) de cada
 * etapa d'acord amb els càlculs fet a la funció de back_prop i el factor
 * d'aprenentatge alpha
 *
 * @see back_prop
 */
void update_weights(void) 
{
    for (int i = 0; i < num_layers - 1; i++) 
    {
        #pragma omp for collapse(2)
        for (int j = 0; j < num_neurons[i + 1]; j++)
        {
          for (int k = 0; k < num_neurons[i]; k++)  // Update Weights
          {
                  lay[i].out_weights[j * num_neurons[i] + k] =
                    (lay[i].out_weights[j * num_neurons[i] + k]) -
                    (alpha * lay[i].dw[j * num_neurons[i] + k]);
          }
        }
        #pragma omp for  
        for (int j = 0; j < num_neurons[i]; j++)  // Update Bias
            lay[i].bias[j] = lay[i].bias[j] - (alpha * lay[i].dbias[j]);
    }
}

```

10 epoch
```shell

[cap-30@clus-login openmp-torn2_grup6]$ cat resultat.txt
Programa amb  2  threads.

Training...

Testing...

Total encerts = 885


Goodbye! (7.828264 sec)


 Performance counter stats for './exec':

         15.627,78 msec task-clock:u                     #    1,993 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             4.761      page-faults:u                    #  304,650 /sec
    39.996.106.885      cycles:u                         #    2,559 GHz                         (66,66%)
     2.843.603.955      stalled-cycles-frontend:u        #    7,11% frontend cycles idle        (66,65%)
     8.220.389.514      stalled-cycles-backend:u         #   20,55% backend cycles idle         (66,66%)
    80.108.436.929      instructions:u                   #    2,00  insn per cycle
                                                  #    0,10  stalled cycles per insn     (66,68%)
    11.135.263.660      branches:u                       #  712,530 M/sec                       (66,68%)
        11.683.545      branch-misses:u                  #    0,10% of all branches             (66,67%)

       7,840728816 seconds time elapsed

      15,394958000 seconds user
       0,087078000 seconds sys




Programa amb  4  threads.

Training...

Testing...

Total encerts = 885


Goodbye! (4.249593 sec)


 Performance counter stats for './exec':

         16.919,96 msec task-clock:u                     #    3,971 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             5.306      page-faults:u                    #  313,594 /sec
    43.324.345.725      cycles:u                         #    2,561 GHz                         (66,67%)
     2.994.989.658      stalled-cycles-frontend:u        #    6,91% frontend cycles idle        (66,67%)
    10.517.187.200      stalled-cycles-backend:u         #   24,28% backend cycles idle         (66,65%)
    81.297.555.027      instructions:u                   #    1,88  insn per cycle
                                                  #    0,13  stalled cycles per insn     (66,65%)
    11.455.863.682      branches:u                       #  677,062 M/sec                       (66,68%)
        12.991.302      branch-misses:u                  #    0,11% of all branches             (66,69%)

       4,260777097 seconds time elapsed

      16,685705000 seconds user
       0,087150000 seconds sys




Programa amb  6  threads.

Training...

Testing...

Total encerts = 885


Goodbye! (3.226601 sec)


 Performance counter stats for './exec':

         19.219,35 msec task-clock:u                     #    5,936 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             7.100      page-faults:u                    #  369,419 /sec
    49.195.530.351      cycles:u                         #    2,560 GHz                         (66,65%)
     3.329.717.920      stalled-cycles-frontend:u        #    6,77% frontend cycles idle        (66,68%)
    14.070.296.867      stalled-cycles-backend:u         #   28,60% backend cycles idle         (66,67%)
    83.996.607.580      instructions:u                   #    1,71  insn per cycle
                                                  #    0,17  stalled cycles per insn     (66,67%)
    12.212.265.484      branches:u                       #  635,415 M/sec                       (66,69%)
        14.136.043      branch-misses:u                  #    0,12% of all branches             (66,66%)

       3,237933118 seconds time elapsed

      18,909030000 seconds user
       0,144470000 seconds sys




Programa amb  8  threads.

Training...

Testing...

Total encerts = 885


Goodbye! (2.664625 sec)


 Performance counter stats for './exec':

         21.126,72 msec task-clock:u                     #    7,894 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             4.952      page-faults:u                    #  234,395 /sec
    54.071.671.565      cycles:u                         #    2,559 GHz                         (66,67%)
     3.539.001.619      stalled-cycles-frontend:u        #    6,55% frontend cycles idle        (66,65%)
    17.819.743.385      stalled-cycles-backend:u         #   32,96% backend cycles idle         (66,65%)
    85.172.343.811      instructions:u                   #    1,58  insn per cycle
                                                  #    0,21  stalled cycles per insn     (66,69%)
    12.537.241.337      branches:u                       #  593,431 M/sec                       (66,69%)
        15.575.755      branch-misses:u                  #    0,12% of all branches             (66,66%)

       2,676153441 seconds time elapsed

      20,824926000 seconds user
       0,118649000 seconds sys




Programa amb  10  threads.

Training...

Testing...

Total encerts = 885


Goodbye! (2.846885 sec)


 Performance counter stats for './exec':

         28.206,80 msec task-clock:u                     #    9,868 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             5.886      page-faults:u                    #  208,673 /sec
    72.277.403.476      cycles:u                         #    2,562 GHz                         (66,68%)
     4.573.801.218      stalled-cycles-frontend:u        #    6,33% frontend cycles idle        (66,67%)
    29.225.990.221      stalled-cycles-backend:u         #   40,44% backend cycles idle         (66,66%)
    91.969.738.583      instructions:u                   #    1,27  insn per cycle
                                                  #    0,32  stalled cycles per insn     (66,66%)
    14.445.424.149      branches:u                       #  512,126 M/sec                       (66,67%)
        16.465.875      branch-misses:u                  #    0,11% of all branches             (66,68%)

       2,858268000 seconds time elapsed

      27,828701000 seconds user
       0,135563000 seconds sys




Programa amb  12  threads.

Training...

Testing...

Total encerts = 885


Goodbye! (2.877581 sec)


 Performance counter stats for './exec':

         34.226,09 msec task-clock:u                     #   11,847 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             6.742      page-faults:u                    #  196,984 /sec
    87.857.737.325      cycles:u                         #    2,567 GHz                         (66,64%)
     5.936.197.475      stalled-cycles-frontend:u        #    6,76% frontend cycles idle        (66,64%)
    36.757.950.095      stalled-cycles-backend:u         #   41,84% backend cycles idle         (66,67%)
   101.413.836.477      instructions:u                   #    1,15  insn per cycle
                                                  #    0,36  stalled cycles per insn     (66,70%)
    17.139.039.994      branches:u                       #  500,759 M/sec                       (66,71%)
        18.321.251      branch-misses:u                  #    0,11% of all branches             (66,68%)

       2,889112196 seconds time elapsed

      33,814279000 seconds user
       0,147766000 seconds sys



```


100 epoch

```shell
[cap-30@clus-login openmp-torn2_grup6]$ cat resultat.txt
Programa amb  2  threads.

Training...

Testing...

Total encerts = 903


Goodbye! (76.982920 sec)


 Performance counter stats for './exec':

        153.938,50 msec task-clock:u                     #    1,999 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
             8.395      page-faults:u                    #   54,535 /sec
   394.355.788.860      cycles:u                         #    2,562 GHz                         (66,67%)
    28.306.783.316      stalled-cycles-frontend:u        #    7,18% frontend cycles idle        (66,67%)
    80.191.564.573      stalled-cycles-backend:u         #   20,33% backend cycles idle         (66,66%)
   791.402.585.441      instructions:u                   #    2,01  insn per cycle
                                                  #    0,10  stalled cycles per insn     (66,66%)
   109.842.371.032      branches:u                       #  713,547 M/sec                       (66,67%)
       115.047.195      branch-misses:u                  #    0,10% of all branches             (66,67%)

      76,995938357 seconds time elapsed

     151,811179000 seconds user
       0,705298000 seconds sys




Programa amb  4  threads.

Training...

Testing...

Total encerts = 903


Goodbye! (41.719038 sec)


 Performance counter stats for './exec':

        166.727,45 msec task-clock:u                     #    3,995 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            22.285      page-faults:u                    #  133,661 /sec
   427.228.671.378      cycles:u                         #    2,562 GHz                         (66,65%)
    29.851.093.788      stalled-cycles-frontend:u        #    6,99% frontend cycles idle        (66,66%)
   102.664.916.659      stalled-cycles-backend:u         #   24,03% backend cycles idle         (66,67%)
   803.135.862.155      instructions:u                   #    1,88  insn per cycle
                                                  #    0,13  stalled cycles per insn     (66,67%)
   113.017.295.583      branches:u                       #  677,857 M/sec                       (66,67%)
       126.446.943      branch-misses:u                  #    0,11% of all branches             (66,66%)

      41,730685130 seconds time elapsed

     164,457124000 seconds user
       0,847763000 seconds sys




Programa amb  6  threads.

Training...

Testing...

Total encerts = 903


Goodbye! (31.344605 sec)


 Performance counter stats for './exec':

        187.864,19 msec task-clock:u                     #    5,991 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            27.612      page-faults:u                    #  146,979 /sec
   481.280.164.484      cycles:u                         #    2,562 GHz                         (66,67%)
    32.663.896.151      stalled-cycles-frontend:u        #    6,79% frontend cycles idle        (66,66%)
   135.909.200.046      stalled-cycles-backend:u         #   28,24% backend cycles idle         (66,65%)
   827.118.562.412      instructions:u                   #    1,72  insn per cycle
                                                  #    0,16  stalled cycles per insn     (66,67%)
   119.611.458.517      branches:u                       #  636,691 M/sec                       (66,68%)
       137.180.899      branch-misses:u                  #    0,11% of all branches             (66,67%)

      31,355805149 seconds time elapsed

     185,318948000 seconds user
       0,904434000 seconds sys




Programa amb  8  threads.

Training...

Testing...

Total encerts = 903


Goodbye! (26.041493 sec)


 Performance counter stats for './exec':

        208.102,91 msec task-clock:u                     #    7,988 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            16.476      page-faults:u                    #   79,172 /sec
   532.808.408.176      cycles:u                         #    2,560 GHz                         (66,67%)
    35.193.086.429      stalled-cycles-frontend:u        #    6,61% frontend cycles idle        (66,67%)
   174.429.290.579      stalled-cycles-backend:u         #   32,74% backend cycles idle         (66,66%)
   841.805.066.448      instructions:u                   #    1,58  insn per cycle
                                                  #    0,21  stalled cycles per insn     (66,66%)
   123.749.870.827      branches:u                       #  594,657 M/sec                       (66,67%)
       153.513.804      branch-misses:u                  #    0,12% of all branches             (66,67%)

      26,053335881 seconds time elapsed

     205,057987000 seconds user
       1,219312000 seconds sys




Programa amb  10  threads.

Training...

Testing...

Total encerts = 903


Goodbye! (28.052988 sec)


 Performance counter stats for './exec':

        280.148,48 msec task-clock:u                     #    9,982 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            34.529      page-faults:u                    #  123,252 /sec
   718.156.868.672      cycles:u                         #    2,563 GHz                         (66,66%)
    45.978.373.137      stalled-cycles-frontend:u        #    6,40% frontend cycles idle        (66,67%)
   289.746.920.478      stalled-cycles-backend:u         #   40,35% backend cycles idle         (66,67%)
   909.912.987.575      instructions:u                   #    1,27  insn per cycle
                                                  #    0,32  stalled cycles per insn     (66,67%)
   142.974.047.507      branches:u                       #  510,351 M/sec                       (66,67%)
       161.406.007      branch-misses:u                  #    0,11% of all branches             (66,66%)

      28,064323585 seconds time elapsed

     276,503939000 seconds user
       1,232789000 seconds sys




Programa amb  12  threads.

Training...

Testing...

Total encerts = 903


Goodbye! (28.066408 sec)


 Performance counter stats for './exec':

        335.652,88 msec task-clock:u                     #   11,955 CPUs utilized
                 0      context-switches:u               #    0,000 /sec
                 0      cpu-migrations:u                 #    0,000 /sec
            24.171      page-faults:u                    #   72,012 /sec
   861.839.026.915      cycles:u                         #    2,568 GHz                         (66,67%)
    58.305.934.983      stalled-cycles-frontend:u        #    6,77% frontend cycles idle        (66,67%)
   353.789.596.134      stalled-cycles-backend:u         #   41,05% backend cycles idle         (66,67%)
   988.863.499.233      instructions:u                   #    1,15  insn per cycle
                                                  #    0,36  stalled cycles per insn     (66,67%)
   165.343.952.932      branches:u                       #  492,604 M/sec                       (66,66%)
       177.927.790      branch-misses:u                  #    0,11% of all branches             (66,66%)

      28,077365039 seconds time elapsed

     331,720203000 seconds user
       1,356983000 seconds sys




```