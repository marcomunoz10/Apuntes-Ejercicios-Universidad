###### Apuntes sobre la entrega de problemas

## *Redis Database Fyle*

El mecanismo **RDB (Redis Database File)** en Redis permite tomar instantáneas(snapshots) del estado completo de la base de datos a intervalos regulares, guardándolas en un archivo binario comprimido. Esto es útil para copias de seguridad, ya que los archivos RDB son compactos y eficientes en almacenamiento. Los snapshots se realizan en segundo plano, sin afectar el rendimiento de las operaciones de escritura, y permiten reinicios rápidos de Redis al cargar directamente el conjunto de datos completo.

**Ventajas**:
- **Compacidad**: Los archivos RDB son pequeños y eficientes, con la opción de compresión LZF para reducir aún más el tamaño.
- **Rendimiento en escritura**: Las instantáneas no afectan las escrituras, aunque el proceso de forking podría impactar mínimamente.
- **Reinicio rápido**: Permiten reinicios más ágiles al cargar toda la base de datos de una vez.

**Desventajas**:
- **Durabilidad**: RDB puede provocar pérdida de datos entre instantáneas, lo cual es problemático si se requiere una recuperación total.
- **Impacto en rendimiento**: La creación de instantáneas es intensiva en recursos, especialmente con bases de datos grandes, y puede afectar el rendimiento durante la actualización.

El **modo de persistencia AOF (Append Only File)** en Redis registra todas las operaciones de modificación de datos en un archivo de registro, de manera que cada comando que cambia el estado de la base de datos queda guardado en secuencia. Este archivo permite reconstruir el estado completo de la base de datos al volver a ejecutar cada operación en el orden en que se recibió.

## *Append Only Fyle*

Registra en un archivo cada operación de escritura que recibe el servidor, lo que permite reconstruir el estado completo de la base de datos al reproducir las operaciones en secuencia. Esto ayuda a la recuperación en caso de pérdida de datos.    


**Desventajas**:
- **Tamaño y gestión**: El archivo AOF puede ser significativamente más grande que una instantánea RDB, especialmente si hay cambios frecuentes en los datos. Redis ofrece un proceso de compactación (re-escritura) para reducir su tamaño, pero aún así suele ser mayor.
- **Rendimiento**: La restauración del estado es más lenta que con RDB, ya que necesita reproducir cada operación en lugar de cargar una imagen completa. Además, bajo cargas de escritura intensa, AOF puede tener un rendimiento menos predecible comparado con RDB.