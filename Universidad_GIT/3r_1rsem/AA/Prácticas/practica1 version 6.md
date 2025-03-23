
Explicación Función:
Claro, vamos a aclarar el propósito de la búsqueda binaria en el contexto de la función `UpdateReversed`.



La versión 6 he hecho un loop unroll en la funcion de update reversed:
Así es como estaba antes

```
for (int xy=0; xy<d*d; xy++) { 
unsigned v = board[xy]; 
unsigned pos = freq[v]+localid[xy]; 
board[xy] = binsearch( freq, valmax, d_d-pos)-1; 
} 
```

Así es cómo está ahora:

```
void __attribute__ ((noinline)) 
UpdateReversed(unsigned BOARD[], unsigned Freq[], unsigned LocalId[], int D, int ValMax) {
    int a = D * D;

    // Bucle desenrollado para procesar de a dos elementos por iteración
    #pragma omp parallel for
    for (int xy = 0; xy < a; xy += 2) {
        // Procesar el primer elemento
        unsigned V1 = BOARD[xy];
        unsigned pos1 = Freq[V1] + LocalId[xy];
        BOARD[xy] = BinSearch(Freq, ValMax, a - pos1) - 1;

        // Procesar el segundo elemento
        if (xy + 1 < a) {
            unsigned V2 = BOARD[xy + 1];
            unsigned pos2 = Freq[V2] + LocalId[xy + 1];
            BOARD[xy + 1] = BinSearch(Freq, ValMax, a - pos2) - 1;
        }
    }
}
```


# versión 7
He modificado una linea por
```
#pragma omp parallel for(V1, pos1)
```
cosa que ha hecho bajar más de un segundo.
También lo he probado dentro del if pero no ha funcionado.



# versión 8
Para esta versión he intentado poner inline la función de [[Búsqueda binaria]]. Lo cual reduce 20 milésimas de segundo.

# versión 9
El cuello de botella sigue siendo updatereversed y se intenta optimizar la búsqueda binaria ya que ahi está el cuello de botella. 
En esta versión se realiza la búsqueda binaria una vez por cada posible valor del `target` y después actualizar `Board[]`. 
Como no sabemos el tamaño del array, utilizaremos [[Memoria dinámica]] para guardar los resultados de la búsqueda binaria:
```cpp
unsigned* binarySearchResults = (unsigned*) malloc(a * sizeof(unsigned));
```

función acabada:

```cpp

void __attribute__ ((noinline)) 
UpdateReversed(unsigned BOARD[], unsigned Freq[], unsigned LocalId[], int D, int ValMax) {
    int a = D * D;

    // Array para almacenar los resultados de la búsqueda binaria
    unsigned* binarySearchResults = (unsigned*) malloc(a * sizeof(unsigned));

    // Paralelización para realizar las búsquedas binarias fuera del bucle principal
    #pragma omp parallel for
    for (int x = 0; x < a; x++) {
        unsigned target = a - x;       
        unsigned L = 0, R = ValMax;


        while (L < R) {
            unsigned M = (L + R) / 2;
            if (Freq[M] < target) {
                L = M + 1;
            } else {
                R = M;
            }
        }

        binarySearchResults[x] = R - 1;   // Almacenar el resultado de la búsqueda binaria
    }

    // Ahora recorremos el tablero y utilizamos los resultados de la búsqueda binaria
    #pragma omp parallel for
    for (int xy = 0; xy < a; xy++) {
        unsigned V = BOARD[xy];                // Obtener valor del tablero
        unsigned pos = Freq[V] + LocalId[xy];  // Calcular posición en base a Freq y LocalId
        BOARD[xy] = binarySearchResults[pos];  // Utilizar el resultado precomputado
    }

    // Liberar la memoria del array de resultados
    free(binarySearchResults);
}

```

# versión 10
Lo primero que he visto es que el cuello de botella está otra vez en updateBoard, así que he mirado el código ensamblador. Me he dado cuenta que utiliza mediante SIMD registros del tamaño de ymm1. Así que lo primero que he probado ha sido cambiar de tamaño los vectores a int. Al hacer `perf stat` veo que tarda lo mismo que antes.

### Resumen de Cambios

1. **Nuevo parámetro `blockSize`**: Añadí un nuevo parámetro para definir el tamaño de los bloques en los que se divide el tablero para su actualización.
   
2. **Modificación de `UpdateBOARD`**: La función `UpdateBOARD` ahora recibe el parámetro `blockSize` y actualiza el tablero por bloques en lugar de por celdas individuales. Este cambio optimiza la ejecución en paralelo.

3. **Liberación de memoria**: Aseguramos que toda la memoria asignada dinámicamente sea liberada al final del programa.

4. **Impresión de valores**: Agregué una línea para imprimir el valor del `blockSize` usado en la ejecución, lo cual facilita la depuración.


### 1. **Incorporación del Tamaño de Bloque (`blockSize`)**

#### Cambio:
He agregado una nueva variable `blockSize` en el `main` que define el tamaño del bloque para la actualización del tablero. Este valor es utilizado por la nueva versión de la función `UpdateBOARD` que opera por bloques.

```c
int blockSize = 32;
```

Además, se añade la posibilidad de pasar este valor como un argumento en la línea de comandos:

```c
if (argc > 4) { blockSize = atoi(argv[4]); }
```

De esta manera, puedes especificar el tamaño del bloque cuando ejecutes el programa:

```bash
./programa 4002 10 50 64  // Aquí el bloque sería de tamaño 64
```

#### Razón:
Permite controlar la granularidad de la paralelización. La computación por bloques mejora la distribución del trabajo en los hilos cuando se usa paralelismo, y puede reducir el costo asociado a cachés y optimizar la memoria.

---

### 2. **Uso de la Nueva Función `UpdateBOARD` con Bloques**

#### Cambio:
He sustituido la llamada original a la función `UpdateBOARD` por la nueva versión que maneja actualizaciones en bloques:

```c
UpdateBOARD(BOARD, TMP, D, MAX, blockSize);
```

La nueva función `UpdateBOARD` toma el parámetro `blockSize`, que controla el tamaño de cada bloque en el que se divide el tablero para su actualización. Esta función divide el área interna del tablero en bloques más pequeños y paraleliza la actualización de cada bloque.

#### Razón:
En lugar de paralelizar directamente la actualización de celdas individuales, el nuevo método actualiza bloques completos, lo que mejora la coherencia de la memoria y el rendimiento general en sistemas paralelos. Al hacer el trabajo en bloques más grandes, los hilos pueden trabajar en áreas contiguas de la memoria, mejorando la eficiencia de la caché.

---

### 3. **Liberación de Memoria Dinámica**

#### Cambio:
He agregado llamadas a `free` para liberar la memoria asignada dinámicamente:

```c
free(BOARD);
free(TMP);
free(Freq);
free(LocID);
```

#### Razón:
Siempre es una buena práctica liberar la memoria que se ha asignado dinámicamente con `malloc` una vez que ya no se necesita. Esto evita fugas de memoria, que pueden ser críticas en programas que manejan grandes volúmenes de datos o se ejecutan durante mucho tiempo.

---

### 4. **Impresión del Tamaño de Bloque en la Salida**

#### Cambio:
En el mensaje de salida, he añadido la impresión del valor de `blockSize` para verificar que el programa está usando el tamaño correcto de los bloques:

```c
printf("DIM = %d, N = %d, Iter = %d, BlockSize = %d\n", D, N, Iter, blockSize);
```

#### Razón:
Esto facilita el seguimiento de los valores de los parámetros usados en la ejecución. Es útil para depurar y verificar si el tamaño de bloque se está pasando correctamente.

---


# versión 11
Para esta versión he intentado añadir [[Prefetching]] de manera manual pero ha augmentado 3 segundos.
También he probado a cambiar el orden de los bucles en la función `CopyBoard`, lo cual también ha augmentado el tiempo.
Otra opción seria intentar eliminar copyboard porque consume mucho tiempo, pero cambia el checksum.