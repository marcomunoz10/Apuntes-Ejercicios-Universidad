### Ejemplos de uso de directivas con `#pragma acc`
- **`#pragma acc data`**: Define una región de datos para controlar la transferencia de datos entre CPU y GPU.
- **`#pragma acc parallel`**: Indica que el código dentro de esta región debe ejecutarse en paralelo en el device.
- **`#pragma acc kernels`**: Permite al compilador analizar el código y decidir qué partes pueden paralelizarse automáticamente.
- **`#pragma acc loop`**: Indica que un bucle debe ejecutarse en paralelo en el device.

### Ejemplo completo
Para paralelizar un bucle y gestionar la transferencia de datos, podría usarse así:
```c
#pragma acc data copy(a, b)
{
    #pragma acc parallel
    {
        #pragma acc loop
        for (int i = 0; i < N; i++) {
            b[i] = a[i] * 2;
        }
    }
}
```

En este ejemplo, cada `#pragma acc` está seguido de una directiva específica (`data`, `parallel`, `loop`) que indica al compilador cómo manejar los datos y la paralelización.

### Resumen
Siempre se debe especificar una directiva o cláusula junto con `#pragma acc` para indicar qué acción realizar en OpenACC.