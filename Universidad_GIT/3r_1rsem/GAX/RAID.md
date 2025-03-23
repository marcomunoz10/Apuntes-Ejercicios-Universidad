### RAID (Redundant Array of Independent Disks)

RAID es un conjunto de tecnologías que combinan **múltiples discos duros** o unidades de almacenamiento en una única unidad lógica, con el objetivo de mejorar el rendimiento, proporcionar **tolerancia a fallos** o ambas. Las configuraciones de RAID permiten distribuir los datos entre varios discos para aumentar la seguridad (redundancia), la velocidad, o ambas, dependiendo del tipo de RAID. 

Als discos hi ha EOL (end of life)

- **RAID 1 (Mirroring)**:
    
    - **Objetivo**: Redundancia y tolerancia a fallos.
    - **Cómo funciona**: Hace una copia exacta (mirroring) de los datos en dos o más discos. Todos los discos tienen la misma información.
    - **Ventajas**: Si un disco falla, los datos aún están seguros en el otro disco.
    - **Desventajas**: Se pierde capacidad de almacenamiento efectivo, ya que la mitad o más de los discos están dedicados a la redundancia.
- **RAID 1 (Mirroring)**:
    
    - **Objetivo**: Redundancia y tolerancia a fallos.
    - **Cómo funciona**: Hace una copia exacta (mirroring) de los datos en dos o más discos. Todos los discos tienen la misma información.
    - **Ventajas**: Si un disco falla, los datos aún están seguros en el otro disco.
    - **Desventajas**: Se pierde capacidad de almacenamiento efectivo, ya que la mitad o más de los discos están dedicados a la redundancia.
- **RAID 5 (Striping con Paridad Distribuida)**:
    
    - **Objetivo**: Equilibrio entre rendimiento y redundancia.
    - **Cómo funciona**: Los datos se dividen en varias unidades (striping), pero también se almacena información de paridad (para la recuperación de datos) de manera distribuida entre los discos. Se necesita un mínimo de 3 discos.
    - **Ventajas**: Mejora el rendimiento de lectura y proporciona tolerancia a fallos. Si un disco falla, los datos se pueden reconstruir a partir de la paridad.
    - **Desventajas**: El proceso de recuperación tras el fallo de un disco es lento, y si fallan dos discos al mismo tiempo, se pierde toda la información.
- **RAID 6 (Striping con Doble Paridad Distribuida)**:
    
    - **Objetivo**: Mayor redundancia que RAID 5.
    - **Cómo funciona**: Similar a RAID 5, pero almacena dos bloques de paridad, permitiendo que se puedan perder hasta dos discos sin perder datos.
    - **Ventajas**: Mayor tolerancia a fallos (pueden fallar dos discos).
    - **Desventajas**: Rendimiento de escritura más lento que RAID 5 debido a la necesidad de calcular dos paridades.
