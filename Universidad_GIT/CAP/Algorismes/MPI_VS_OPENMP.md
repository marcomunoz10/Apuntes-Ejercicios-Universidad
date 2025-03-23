### **Diferencias entre MPI y OpenMP**

MPI (Message Passing Interface) y OpenMP (Open Multi-Processing) son dos modelos de programación paralela ampliamente utilizados, pero tienen diferencias clave en su arquitectura, uso y aplicación.

|**Característica**|**MPI (Message Passing Interface)**|**OpenMP (Open Multi-Processing)**|
|---|---|---|
|**Modelo de paralelismo**|Paso de mensajes (distribuido)|Compartición de memoria (multihilo)|
|**Arquitectura**|Se usa en sistemas distribuidos (clusters, supercomputadoras).|Se usa en sistemas de memoria compartida (CPU con múltiples núcleos).|
|**Comunicación entre procesos**|Requiere intercambio de mensajes entre procesos (MPI_Send, MPI_Recv).|Usa memoria compartida, evitando la necesidad de mensajes explícitos.|
|**Escalabilidad**|Escala mejor en supercomputadoras con miles de nodos.|Limitado a un solo nodo con múltiples núcleos.|
|**Facilidad de uso**|Más complejo, requiere gestión manual de procesos y comunicación.|Más fácil, basado en directivas `#pragma omp`.|
|**Lenguajes compatibles**|C, C++, Fortran.|C, C++, Fortran.|
|**Ejemplo de uso**|Simulaciones científicas, computación en la nube, Big Data.|Aplicaciones en CPU de múltiples núcleos, como optimización de código en procesadores modernos.|

---


### **Ejemplo en MPI (Distribuido)**

Cada proceso ejecuta el mismo código pero maneja datos distintos.

```c
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);
    
    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    printf("Proceso %d de %d\n", rank, size);

    MPI_Finalize();
    return 0;
}
```

⏩ **Ejecución típica** en un clúster:

```sh
mpicc programa.c -o programa
mpirun -np 4 ./programa
```

---

### **Ejemplo en OpenMP (Memoria Compartida)**

Se ejecuta en múltiples hilos dentro de una CPU.

```c
#include <stdio.h>
#include <omp.h>

int main() {
    #pragma omp parallel
    {
        int id = omp_get_thread_num();
        printf("Hola desde el hilo %d\n", id);
    }
    return 0;
}
```

⏩ **Compilación y ejecución:**

```sh
gcc -fopenmp programa.c -o programa
./programa
```

---

### **¿Cuándo usar cada uno?**

✅ **Usa MPI si…**

- Necesitas ejecutar en múltiples nodos o servidores.
- Requieres escalar a cientos o miles de procesos.
- La memoria compartida no es viable (por ejemplo, en un clúster).

✅ **Usa OpenMP si…**

- Trabajas en una sola máquina con múltiples núcleos.
- Buscas una forma sencilla de paralelizar código sin manejar mensajes.
- Quieres mejorar el rendimiento de una aplicación en CPU sin grandes cambios.

📌 **¿Y si necesito ambos?** Puedes combinarlos en **programación híbrida (MPI + OpenMP)** para aprovechar lo mejor de ambos mundos. 🚀