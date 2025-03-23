
1. **`gang`**: grupo de **threads** que trabajan juntos para ejecutar una parte del código en paralelo en un dispositivo de aceleración, como una GPU. La idea de los _gangs_ es similar al concepto de **bloques (blocks)** en CUDA.

2. **`vector`**: Dentro de cada _gang_, los hilos se dividen en _vectores_. Representa un conjunto de **threads** que trabajan juntos para realizar una tarea de manera paralela. No se pueden comunicar vectores entre sí. En CUDA serían threads dentro de un bloque.

### ¿Qué hace `#pragma acc loop gang vector`?
Cuando se aplica a un bucle, esta directiva indica que:
- El bucle se paralelizará tanto a nivel de *gangs* como de *vectores*.
- Cada iteración del bucle se distribuirá entre varios *gangs* y *vectores* dentro del dispositivo.
  
Esto permite explotar al máximo el paralelismo disponible en un acelerador como la GPU, utilizando tanto niveles de trabajo por bloques como dentro de cada bloque.

### Ejemplo
```c
#pragma acc parallel
{
    #pragma acc loop gang vector
    for (int i = 0; i < N; i++) {
        a[i] = b[i] + c[i];
    }
}
```

En este ejemplo:
- El bucle `for` se paraleliza en múltiples *gangs* (grupos de hilos) y en múltiples *vectores* (hilos individuales dentro de cada gang).
- Cada elemento `a[i]` se calcula en paralelo, maximizando el uso de los recursos de la GPU.

### Resumen de niveles
- **`gang`**: Paralelización entre grupos independientes de hilos.
- **`vector`**: Paralelización dentro de cada grupo de hilos.

Esta combinación permite aprovechar al máximo el paralelismo jerárquico en el dispositivo, logrando una ejecución más eficiente en GPUs.