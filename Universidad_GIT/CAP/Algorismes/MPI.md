### **MPI (Message Passing Interface)**

MPI es un estándar de programación para computación paralela que permite la comunicación entre múltiples procesos en un sistema distribuido o en un clúster de computadoras. Es ampliamente utilizado en supercomputación y en aplicaciones científicas que requieren procesamiento de alto rendimiento.

---

## **Características principales de MPI:**

✅ **Modelo de paso de mensajes**: Los procesos se comunican enviando y recibiendo datos a través de mensajes.  
✅ **Paralelismo distribuido**: Funciona en arquitecturas donde los procesos se ejecutan en diferentes nodos de una red.  
✅ **Portabilidad**: No está ligado a un hardware o sistema operativo específico.  
✅ **Escalabilidad**: Se puede usar en sistemas con miles o incluso millones de procesadores.

---

## **Ejemplo básico en C con MPI**

Este programa inicializa MPI, obtiene el número total de procesos y el identificador de cada proceso (`rank`), y luego imprime un mensaje desde cada proceso.

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);  // Inicializa MPI

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);  // Obtiene el ID del proceso
    MPI_Comm_size(MPI_COMM_WORLD, &size);  // Obtiene el número total de procesos

    printf("Hola desde el proceso %d de %d\n", rank, size);

    MPI_Finalize();  // Finaliza MPI
    return 0;
}
```

---

## **Funciones comunes de MPI**

📌 `MPI_Init(&argc, &argv)`: Inicializa MPI.  
📌 `MPI_Comm_rank(MPI_COMM_WORLD, &rank)`: Obtiene el identificador único de cada proceso.  
📌 `MPI_Comm_size(MPI_COMM_WORLD, &size)`: Obtiene el número total de procesos.  
📌 `MPI_Send()` y `MPI_Recv()`: Envían y reciben mensajes entre procesos.  
📌 `MPI_Bcast()`: Difunde un mensaje a todos los procesos.  
📌 `MPI_Reduce()`: Realiza operaciones de reducción (sumas, productos, etc.) entre procesos.  
📌 `MPI_Finalize()`: Termina la ejecución de MPI.

---

## **¿Dónde se usa MPI?**

🔹 **Simulaciones científicas** (clima, dinámica molecular, física de partículas).  
🔹 **Análisis de Big Data en entornos distribuidos**.  
🔹 **Entrenamiento de modelos de inteligencia artificial en supercomputadoras**.  
🔹 **Computación en la nube y sistemas de alto rendimiento (HPC)**.




