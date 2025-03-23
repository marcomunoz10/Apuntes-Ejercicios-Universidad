## Introduction and matrix multiplication (Pol Muñoz)

#### La optimización del rendimiento
Nos ayuda a aprovechar mejor nuestros recursos HW. Antes de 2004 se optimizaba a partir del HW hasta q se llegó al límite. La optimización dejó de ser automática y se pasó a optimizar en SW.
Pytjon 21k, java 3k, c=761 pq en C se traduce directamente a ensamblador.
Flag -O3 y pone speedup respecto a python.
Ya q se accedia por columnas habia fallos de caché --> hacer interchange loops.
Si se pone paralelización al bucle exterior mejor, 
Estrategia tiling --> divide por bloques y mira a ver q bloque tarda menos y hace más for.
-march=native
Estrategia divide and conquer --> divide por secciones pequeñas de forma recursiva, accede a bloques pequeños y luego los multiplica entre ellos. Lo hace con tamaño de bloques.
Mirar mecanismo de Staghen.
La versión Ofast utiliza fast  math y da diferencia en el resultado pq puede interferir.
Terminal fondo blanco y q las capturas se vean más grandes.
Python no va más lento pq esté interpretado, con python no declaras el tipo de datos y aunq compilaras no sería mejor. Si declaras como lista no es contiguo en memoria. Si coges numpy sí.
En java se traduce a binario y luego se compila.  Cuanto más especificas mejor. 


## Optimización de Merge Sort (Martí Armengod)

Divide and Conquer
### Bubble sort
Recorre la lista y ordena el utimo valor. complejidad O(n^2)

### Estrategia mixta
Divide el problema, ordeno con bubble sort. por cada ordenación ordea un elemento O(n)
### Merge sort
caso trivial lista de un elemento.
función recursiva q divide hasta q llega al elemento de una lista y hace merge. complejidad O(n · logn --> tendrá problema para paralelizar.

omp single solo un thread pasa por el bucle

caso tamaño de lista
evitamos paralaleizar donde es mejor el problema base --> si mayor que 1000 se trata de forma paralela.

Se debe evitar q threads utilicen operaciones muy pequeñas.

Mejor poner tabla incremental

## Bentley Rules for Optimizing Work (Anthony Limarino)
writing efficient programs jon louis bentley
optimizing loops

1. Hoisting
Extraer código invariable de un bucle.
2. Sentinels
Valores especiales para controlar la salida de los bucles si no sabemos cuántas iteraciones va a hacer el bucle cuando acabe.
Puede q haya desbordamiento, entonces él hace un bucle mientras no se ha desbordado, devuelve true o false si se ha desbordado o no.
3. Loop unrolling
4. Loop fusion
Fusionar dos bucles q tengan misma N.
5. Eliminating wasted iterations
Evitar utilizar instrucciones con partes de variables vacías.

## Bentley Rules for Optimizing Work
1. empaquetado y Codificación
2. Precomputación
Triángulo de Pascal


## 09/10
1ª
El trabajo se puede definir como tarea o como tiempo.
regla de bentley minimizar operaciones para incrementar el rendimiento.

lookup table
bona presentació, tranquil, fluid

2ª jan
bona presnetació, fluid
mirar video stack y heap pq es importante
ballgring q detecta cuando te has dejado bloques q has pedido a memoria


---
Ángel Blanco
AMD fidelity fx super resolution

colocar capa de software para mejorar rendimiento sin tocar el hardware.

Super Resolution --> rescala imágenes. los datos se guardan en la overRam. si se ejecuta en resolucion menor no se encapsula la memoria. 

FSR proporciona diferentes tipos de escala.

Ampilar: modifica los archivos para reducir el tiempo de memoria.
Preferible tener menos calidad y mejor latencia.



# Patron scan en OPENMP

més unitats de treball.

reduction (inscan, +:s)

inclusive(s)

tranquil, bona presentació, algunos fallos en presentación

# Queen problem

# Optimizaciones Dani

insertion sort para arrays pequeños, antes hacer bucle con quicksort hasta cierto umbral.
muy tranquilo, conocimiento, presentacion visual.

# Compilacion LLVM e IR Martí

LLVM IR lenguaje intermedio entre C y código máq.

Tranquil, bona presentació, parla alt, coneixement.


# Daniel Perfmormance modeling

9

dificultad, conocimiento, buena explicación

oratoria