Es una [[Base de datos en memoria]], guarda datos en la RAM, lo q lo hace muy veloz, se utiliza como cache.

Ofrece persistencia con _Redis Database File_ el cual toma `snapshots` de manera periódica y AOF que registra cada operación permitiendo recuperación de datos. Los snapshots y AOF se guarda en disco.

### Snapshots
Instantáneas, están guardadas en ficheros binarios. RDS (_Redis Database Fyle_) se puede configurar para tomar `snapshots` cada cierto intervalo de tiempo. Cualquier dato que ocurra entre instantáneas no se va a guardar. 
Ventajas:
- Compacto (se almacena en par-valor).
- La toma de snapshots no influye en el rendimiento de escritura. (Pero el proceso `forking` sí).
- Reinicios Rápidos
Desventajas:
- Durabilidad (pérdida de info entre snapshots)
- Influye en el rendimiento a la hora de hacer forks del server.

### Append Only File (AOF)
Registra cada operación de escritura en un archivo (éste contiene una secuencia de comandos para reconstruir la BD).
Ventajas:
- Minimiza las posible perdida de datos de Snapshots (solo si `fsync` está configurado)
- Transparencia. El archivo appendonly.aof es fácil de inspeccionar. (Se puede abrir con editor y debuguear).
Desventajas:
-  Aunque pueda compactar registros, el fichero es más pesado que `dump.rdb`
- Tiempo de arranque. Requiere reproducir todos los comandos del archivo.
- Más lento que Snapshot

# Arquitectura de Redis

### Standalone
Redis se ejecuta en un nodo. Es más fácil de configurar i desarrollar. 
Permite hacer pequeñas instancias para q vayan más rápidos los programas.

Desventajas:
- Rendimiento limitado. Puede causar cuellos de botella.
- Los datos pueden perderse fácilmente. No hay redundancia (no se copian los datos en otro servidor)

### Redis High Availability
Mantiene una réplica dentro de la misma o en varias regiones.
Base de datos secundaria que se sincroniza con replicación al escribir un dato en la principal.
Puede haber más de una instancia réplica para ayudar a la escalabilidad de lectura y para caso de fallos.

### Redis Sentinel
Tiene nodo maestro y nodos esclavos (se usan en caso de q falle el maestro).
Ofrece:
- Monitoreo de correcto funcionamiento.
- Notificación en caso de fallo.
- [[Failover]] automático en caso de q falle el maestro.

### Redis Cluster
Los datos se dividen en múltiples instancias, cada una tiene un conjunto de llaves.
Concepto `Consistent Hashing`: los datos se distribuyen de manera uniforme entre todos los nodos.
Hay 16386 hash slots.

# Tipo de datos que almacena REDIS

### Strings
Por defecto maximo 512 MB, el más utilizado.
### Listas
Para implementar colas, pilas, colas de trabajo en segundo plano.
Útil para mantener **orden de elementos**: registro de eventos.
Para guardar valores duplicados.
Longitud máxima de 2^31 -1 elementos.
### Sets
Colección desordenada de strings únicas.
### Hash
Almacenan un conjunto de pares campo-valor de manera compacta para representar objetos de datos.
Se puede:
- Agregar, obtener o eliminar elementos individuales dentro del hash.
- Recuperar todo el hash.
- Utilizar los campos como contadores.
Up to store (2³² - 1) field-value pairs.

### Stored Sets
Datos que se ordenan por su puntuación.


---
# AWS caching

Dos tipos de AWS caching: 
`CloudFront` y `ElastiCache`

## CloudFront
Es un `static content delivery netwokr` (CDN) que recupera contenido de la ubicación perimetral más cercana al usuario. Acelera la entrega de contenido como por ejemplo copias de webs.

## ElastiCache
Permite escalar el almacenamiento de datos en memoria (es más rápido que cloudfront)

Se posiciona entre la aplicación y la base de datos. Beneficios:
- Automatiza tareas comunes.
- Es escalable.
- Rendimiento pq se almacena en memoria.
- Pago eficiente.

Dos tipos:
- ElastiCache for Memcached
- ElastiCache for Redis
![[Pasted image 20250114181738.png]]


Se puede asignar un TTL a una llave para que se quite de la RAM.

Dos estrategias de sincronización de Datos:
- Lazy loading strategie: Se actualiza dsps de q un cliente solicite los datos (cuando hay un cache miss) --> útil cuando lees frecuentemente y escribes de vez en cuando. Ventaja: solo los datos solicitados se guardan en cache.
- Write-through strategy: Se actualiza al cambiar la primary db. Los datos siempre están actualizados.