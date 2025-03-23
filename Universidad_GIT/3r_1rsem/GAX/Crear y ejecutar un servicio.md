El comando proporcionado en la miniTarera1:

```bash
systemd-run --scope -p MemoryMax=500M /usr/bin/top
```

hace lo siguiente:

### Desglose del Comando

1. **`systemd-run`**:
   - Este comando se utiliza para crear y ejecutar un servicio o una unidad en **systemd**.

2. **`--scope`**:
   - Este parámetro indica que se va a crear una unidad de **scope**. Las unidades de scope son utilizadas para ejecutar un proceso sin crear un nuevo servicio persistente. Se gestionan de manera similar a las unidades de servicio, pero son más temporales y están diseñadas para agrupar uno o más procesos en ejecución.

3. **`-p MemoryMax=500M`**:
   - Aquí se establece una propiedad para la unidad que se está creando. En este caso, `MemoryMax=500M` limita el uso de memoria del proceso a **500 megabytes**. Si el proceso intenta usar más de esta cantidad de memoria, systemd lo detendrá automáticamente.

4. **`/usr/bin/top`**:
   - Este es el comando que se ejecutará. `top` es una herramienta de monitoreo de procesos que muestra en tiempo real los procesos que se están ejecutando en el sistema, junto con información sobre el uso de la CPU, memoria y más.

### ¿Qué Hace el Comando?

- **Ejecuta `top` como un proceso temporal**: Al ejecutar el comando, se abre la interfaz de `top`, pero el proceso está encapsulado dentro de una unidad de scope gestionada por systemd.
- **Limita el uso de memoria**: La propiedad `MemoryMax=500M` impone una restricción de memoria al proceso `top`. Si `top` intenta usar más de 500 MB de memoria, systemd lo finalizará.

