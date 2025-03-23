Las opciones de `-Minfo` en OpenACC proporcionan distintos niveles de detalles sobre la compilación y optimización del código:

1. **`accel`**: Información sobre aceleración en GPU (mapeo de recursos).
2. **`all`**: Toda la información disponible sobre optimización y paralelización.
3. **`copy`**: Detalles sobre la transferencia de datos entre CPU y GPU.
4. **`opt`**: Información sobre optimizaciones generales (desenrollado de bucles, etc.).
5. **`par`**: Detalles sobre la paralelización de bucles.

Estas opciones se pueden combinar (e.g., `-Minfo=accel,copy`) para obtener información específica, lo que permite optimizar el código y mejorar el rendimiento en sistemas de CPU/GPU.

Ejemplo de uso:

```bash
pgcc -acc -Minfo=accel,copy programa.c
```
