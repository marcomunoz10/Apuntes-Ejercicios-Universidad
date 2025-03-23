SPMD (Single Program, Multiple Data) es un modelo de programación paralela en el que múltiples procesadores ejecutan el mismo programa, pero operan sobre diferentes conjuntos de datos. Es un enfoque común en computación de alto rendimiento y se usa en arquitecturas de memoria compartida y distribuida.

### Características clave del SPMD:

- **Un solo código fuente**: Todos los procesadores ejecutan el mismo programa.
- **Datos diferentes**: Cada procesador trabaja con un subconjunto distinto de datos.
- **Paralelismo implícito**: Se utilizan estructuras condicionales (como `if` o `switch`) y variables de identificación de procesos (`rank` en MPI) para determinar qué parte de los datos maneja cada procesador.
- **Uso en entornos distribuidos**: Es común en MPI (Message Passing Interface) y CUDA (para programación en GPU).

### Ejemplo en MPI:

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);  

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    printf("Hola desde el proceso %d de %d\n", rank, size);

    MPI_Finalize();
    return 0;
}
```

En este caso, todos los procesos ejecutan el mismo código, pero cada uno tiene un identificador (`rank`) que le permite operar sobre diferentes datos.