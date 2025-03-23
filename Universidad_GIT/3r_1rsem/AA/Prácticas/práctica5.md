El archivo detalla un programa que utiliza una tabla hash para realizar inserciones de claves generadas aleatoriamente, con una implementación paralelizada que enfrenta problemas de sincronización. Las variables principales y su significado son:

1. **N**: Número total de claves generadas aleatoriamente que se insertarán en la tabla hash.
2. **H**: Número de entradas (o cubetas) en la tabla hash.
3. **MAX**: Rango máximo de valores posibles para las claves generadas (entre 0 y MAX).
4. **HashT**: La tabla hash propiamente dicha, implementada como un vector de nodos, donde cada nodo contiene una lista enlazada ordenada para manejar colisiones.
5. **Insert()**: Función que realiza la inserción en la tabla:
    - Genera un índice para la tabla usando una función hash.
    - Busca en la lista enlazada asociada a ese índice si la clave ya existe.
    - Incrementa un contador si la clave existe o inserta un nuevo nodo en la lista si no existe.
    - Reorganiza los datos si el nuevo nodo tiene más ocurrencias que otros nodos existentes.

El programa realiza dos tareas principales:

1. **Generar claves aleatorias.** Estas claves son números enteros dentro de un rango determinado (de 0 a **MAX**).
2. **Insertar estas claves en la tabla hash.** Para cada clave:
    - Se calcula el índice del _bucket_ usando una función de hash.
    - La clave se inserta en la lista enlazada correspondiente, actualizando contadores si la clave ya existe o creando un nuevo nodo si no está presente.
    
La paralelización presenta condiciones de carrera debido a la falta de sincronización adecuada en la función `Insert()`.

## Ejercicio 2

![[Pasted image 20241127141400.png]]

`perf_report`
![[Pasted image 20241127141441.png]]
	![[Pasted image 20241127141458.png]]

`test`: test checks, without blocking the caller thread, whether the lock is acquired by a thread or not.

![[Pasted image 20241127141642.png]]

Conclusión ejercicio 2:

El cuello de botella seguramente esté en la creación de nuevos nodos. Ya que se hace de manera secuencial, se ha de esperar hasta que finalice el anterior.
Si asignas memoria para otros datos entre la adición de nodos, los bloques de memoria para los nodos podrían terminar en muchas páginas diferentes. Para acceder a los datos en la lista, el sistema podría tener que intercambiar muchas páginas, lo que puede ralentizar significativamente tu programa.

link que lo explica:
https://www.ibm.com/docs/en/xl-c-and-cpp-aix/16.1?topic=heaps-managing-memory-multiple

# Ejercicio 3

Se muestra la llave que más veces a aparecido de cada bucket con el nnumero de contador al lado.
Cuando utilizo `2 threads`
![[Pasted image 20241127143626.png]]
Se puede observar como supera el tamaño de la lista.

![[Pasted image 20241127143903.png]]

hay un overhead por la cración de tareas `GOMP_task`, tambien se observa `malloc`, lo cual nos indica que invierte más tiempo en gestionar memoria. Se incrementa un `12%` las instrucciones, hay `3,2G` instrucciones más

Cuando utilizo `6 threads`:
![[Pasted image 20241127143716.png]]

De igual manera supera el tamaño de la lista.
![[Pasted image 20241127144750.png]]

La mayoría del tiempo va destinada al sistema operativo.

- **El problema principal: Condiciones de carrera.**
    
    - Cada hilo ejecuta tareas paralelas que llaman a la función `Insert` para agregar claves a la tabla hash.
    - Sin embargo, no hay ninguna sincronización entre hilos cuando acceden a los buckets (listas enlazadas de la tabla hash).
    - Esto significa que varios hilos pueden:
        - Leer y escribir el mismo bucket al mismo tiempo.
        - Modificar nodos de la lista enlazada de manera no segura.
        - Actualizar erróneamente el nodo con la clave de mayor frecuencia.
    
    Esto provoca **errores de ejecución** (como nodos perdidos, contadores inconsistentes, y problemas de orden en las listas).
    
- **Colisiones entre hilos:**
    
    - En tu configuración (**N=40 millones, H=7, MAX=1000**), las claves generadas tienen un rango muy pequeño (**MAX=1000**) y solo hay **7 buckets**.
    - Esto genera muchísimas colisiones, lo que aumenta las probabilidades de que varios hilos accedan al mismo bucket simultáneamente.

Informe:
#### **Perfil con un solo hilo (T=1):**

- Con un solo hilo, el programa ejecuta `Insert` de manera secuencial, dominando el tiempo de CPU (~90%).
- No hay sobrecarga por sincronización ni administración de tareas paralelas, lo que permite que el tiempo esté directamente asociado con la operación principal (manejo de listas enlazadas en los buckets).

#### **Perfil con múltiples hilos (T=2, 6):**

- Al incrementar el número de hilos, se observa un cambio significativo:
    - El tiempo gastado en `Insert` disminuye porque los conflictos de acceso a memoria y la sincronización consumen recursos adicionales.
    - Un mayor porcentaje del tiempo se dedica a tareas del sistema operativo relacionadas con:
        - Manejo de condiciones de carrera.
        - Planificación y administración de tareas paralelas.
        - Sincronización implícita en el acceso a la tabla hash.
        - Retrocesos (rollbacks) por conflictos de memoria.

#### **Conclusión:**

- **Cuello de botella principal:** La falta de sincronización adecuada en la función `Insert` causa condiciones de carrera que incrementan la intervención del sistema operativo.
- **Impacto:** El uso de múltiples hilos no mejora el rendimiento debido a la sobrecarga de administración y conflictos en el acceso a memoria.
- **Recomendaciones:** Para evitar este problema:
    1. Implementar sincronización explícita (por ejemplo, usando bloqueos por bucket).
    2. Reducir la sobrecarga agrupando más operaciones por tarea.
    3. Optimizar la estructura de datos para reducir colisiones y falsa compartición.


# 3.2

```shell
[aa-29@aolin-login PR5]$ nano HT.cpp
[aa-29@aolin-login PR5]$ g++ -Ofast -fopenmp HT.cpp -o HT
[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=1
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions HT
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 1 threads
Hash[0]: key= 28256, count= 10
Hash[1]: key= 50384, count= 9
Hash[2]: key= 48694, count= 9
Hash[3]: key= 34817, count= 9
Hash[4]: key= 89619, count= 9
Hash[5]: key= 88802, count= 10
Hash[6]: key= 85644, count= 9
Ok

 Performance counter stats for 'HT':

    34.183.003.018      cpu_core/cycles:u/
     5.738.780.115      cpu_core/instructions:u/

       7,856781592 seconds time elapsed

       7,848297000 seconds user
       0,000997000 seconds sys


[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=2
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions HT
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 2 threads
Hash[0]: key= 28256, count= 10
Hash[1]: key= 50384, count= 9
Hash[2]: key= 48694, count= 9
Hash[3]: key= 34817, count= 9
Hash[4]: key= 89619, count= 9
Hash[5]: key= 88802, count= 10
Hash[6]: key= 85644, count= 9
Error. List Size is 199990

 Performance counter stats for 'HT':

    34.267.828.004      cpu_core/cycles:u/
     5.758.918.389      cpu_core/instructions:u/

       4,120836067 seconds time elapsed

       8,218100000 seconds user
       0,007655000 seconds sys


[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=6
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions HT
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 6 threads
Hash[0]: key= 28256, count= 10
Hash[1]: key= 50384, count= 9
Hash[2]: key= 48694, count= 9
Hash[3]: key= 34817, count= 9
Hash[4]: key= 89619, count= 9
Hash[5]: key= 88802, count= 10
Hash[6]: key= 85644, count= 9
Error. List Size is 199963

 Performance counter stats for 'HT':

    34.315.974.519      cpu_core/cycles:u/
     5.766.319.896      cpu_core/instructions:u/

       1,441389188 seconds time elapsed

       8,626625000 seconds user
       0,000998000 seconds sys


[aa-29@aolin-login PR5]$ export OMP_NUM_THREADS=12
[aa-29@aolin-login PR5]$ perf stat -e cycles,instructions HT
Insert 200000 (repeated) keys (MAX = 100000) in Hash table with 7 entries using 12 threads
Hash[0]: key= 28256, count= 10
Hash[1]: key= 50384, count= 9
Hash[2]: key= 48694, count= 9
Hash[3]: key= 34817, count= 9
Hash[4]: key= 89619, count= 9
Hash[5]: key= 88802, count= 9
Hash[6]: key= 85644, count= 9
Error. List Size is 199846

 Performance counter stats for 'HT':

    33.220.172.778      cpu_core/cycles:u/
     5.767.943.922      cpu_core/instructions:u/

       0,699772933 seconds time elapsed

       8,342681000 seconds user
       0,004949000 seconds sys


```

#### **Perfil de ejecución (N=200K, MAX=100K):**

1. **Reducción de colisiones:**
    
    - El mayor rango de valores de las claves (**MAX=100K**) reduce la probabilidad de que múltiples hilos accedan al mismo bucket.
    - Esto mejora la independencia del trabajo entre hilos.
2. **Menor tamaño de listas enlazadas:**
    
    - Con menos claves por bucket, las operaciones en las listas enlazadas (búsqueda, inserción, actualización) son más rápidas.
3. **Sobrecarga de tareas menos significativa:**
    
    - El tamaño reducido del problema (**N=200K**) minimiza la sobrecarga de creación y planificación de tareas en comparación con el primer caso (**N=40M**).
4. **Efecto positivo de paralelización:**
    
    - A medida que aumenta el número de hilos, más trabajo se realiza en paralelo, aprovechando mejor los recursos de la CPU.

#### **Conclusión:**

- **Razones de mejora:** La reducción de colisiones y el tamaño pequeño de las listas enlazadas permiten una mejor distribución del trabajo entre hilos, mitigando parcialmente los problemas de condiciones de carrera.
- **Escalabilidad:** En este caso, la paralelización mejora el tiempo de ejecución porque la cantidad de trabajo es adecuada para ser distribuida entre los hilos sin generar una sobrecarga excesiva.



# Ejercicio 4

Se puede usar el mecanismo de **bloqueo por bucket** para sincronizar los accesos concurrentes a las listas enlazadas.

#### **Bloquear el bucket antes de cualquier operación:**

- Antes de acceder o modificar la lista enlazada de un bucket, se debe adquirir el lock del nodo inicial del bucket:
    
    `omp_set_lock(&T[h].lock);  // Bloquear el bucket`
    
- Esto asegura que solo un hilo pueda realizar operaciones en ese bucket al mismo tiempo.

---

#### **2. Liberar el lock después de la operación:**

- Una vez que la operación de inserción o actualización haya terminado, se libera el lock para permitir que otros hilos accedan:
    
    
    `omp_unset_lock(&T[h].lock);  // Desbloquear el bucket`

De esta manera el número total de claves insertadas en la tabla hash es correcto, no aparece ningun mensaje de error.

![[Pasted image 20241202014431.png]]

Con 2 threads:
![[Pasted image 20241202014846.png]]


Con 6 threads:
![[Pasted image 20241202014819.png]]


Con 12 `threads`:
![[Pasted image 20241202014748.png]]

(FALTA PREGUNTAR MAS AL CHAT PQ PASA ESTO Q LO JUSTIFIQUE CUENTA FUTPLAY)

# Ejercicio 5

```
Tienes una versión actualizada del programa, HT2.cpp, que aborda el problema de las condiciones de carrera en la función Insert(). La solución no funciona, y deberías entender la idea y corregir los problemas. Tu primer objetivo es lograr una ejecución correcta en múltiples hilos. Pide ayuda a tus profesores. 

Luego, puedes centrarte en mejorar el rendimiento: compila, ejecuta y evalúa el rendimiento de las ejecuciones en un solo hilo y en múltiples hilos para el siguiente problema: N=200K, H=7 y MAX=100K, con T=1, 6 y 12 hilos.
```


Mover `omp_set_lock(&prev->lock)` antes de `ptr = prev->next` garantiza que el acceso a `prev->next` esté protegido por el bloqueo. Esto evitará condiciones de carrera.

Con esta modificación:

1. Se asegura que `prev->next` no pueda ser modificado por otro hilo mientras se inicializa `ptr`.
2. Esto protege la lectura inicial del puntero, eliminando posibles inconsistencias.

`perf report` antes de modificar nada:
[[eje5 shell]]
![[Pasted image 20241202123800.png]]

### 2HT2.cpp

```cpp

// Se distribuyen las inserciones de forma estática entre los thread
#pragma omp parallel firstprivate(Seed, N) shared(HashT) 
{ 
	unsigned thread_id = omp_get_thread_num(); 
	unsigned total_threads = omp_get_num_threads(); 
	
	 // Cada thread genera e inserta sus propias claves 
	for (int i = thread_id; i < N; i += total_threads) 
	{ 
		unsigned key = nrand48(Seed) % MAX; 
		 Insert(HashT, key); 
	} 
}

```

### **Problemas con la primera version**

1. **Overhead de tareas dinámicas:** La creación y programación de cada tarea (`#pragma omp task`) tiene un coste adicional. Si el trabajo dentro de las tareas es pequeño (como insertar una key), el overhead puede ser más grande que el beneficio.
    
    
2. Uso no óptimo de los threads:** Como los threads tomaban tareas de una cola dinámica, podían quedar inactivos si no había suficientes tareas en la cola, o podían terminar procesando tareas de manera desbalanceada.

En lugar de crear tareas dinámicas, podemos dividir el trabajo estáticamente entre los threads. Cada thread manejará una parte del trabajo desde el inicio, reduciendo el overhead de programación de tareas y mejorando la distribución del trabajo.

![[Pasted image 20241202141258.png]]

Se puede observar como el contador de cada llave está entre 24 y 30. Antes estaba etorno a 9 y 10. 
(Preguntar a chat)

# Ejercicio 6

```
Diseña e implementa una versión especulativa que utilice locks solo al insertar un nuevo nodo en la lista, sin usar locks para buscar el punto de inserción. Ten cuidado: el orden en el que se actualizan los punteros del nodo insertado y la lista es crucial para el correcto funcionamiento del programa. Mide el rendimiento y proporciona una explicación. Consulta el documento de referencia disponible en el Campus Virtual sobre el uso de locks con OpenMP, donde se explica el concepto de la técnica de especulación. También puedes inspirarte en el par de instrucciones de sincronización de bajo nivel llamadas load-linked y store-conditional.

Actividad 6. Explica la estrategia de especulación que pretendes utilizar, intenta codificarla en un nuevo programa, HT3.cpp, y, si lo haces, analiza su rendimiento con un tamaño de problema N = 200K, H = 7, MAX = 100K, con T=1, 6 y 12 hilos.
```

La especulación es una técnica para reducir el coste de la sincronización (u otros costos de computación) en algunas situaciones. La idea es ejecutar parte del código sin utilizar locks, y en algún momento adquirir el lock y verificar que el trabajo realizado sea correcto. Si es correcto, entonces la parte crítica del código está hecha y el lock se libera. Si es incorrecto, entonces se puede especular nuevamente. El algoritmo debe garantizar que después de un cierto número de intentos especulativos, el trabajo se realizará en una cantidad finita de tiempo, tal vez basándose en un algoritmo lento y seguro que utilice locking.

**En informe**
La especulación es una técnica utilizada para reducir el coste de la sincronización en ciertas situaciones. Consiste en ejecutar parte del código sin locks y luego adquirir el lock para verificar si el trabajo realizado es correcto. Si es correcto, se libera el lock; si no, se intenta nuevamente.

En el código anterior, **se bloqueaba cada nodo mientras se recorría la lista** para buscar el punto de inserción o actualizar un contador. Esto introducía un overhead significativo porque cada nodo necesitaba adquirir y liberar su lock.

En el nuevo código:

- Durante la búsqueda (`while`), no usamos locks.
- Solo cuando estamos listos para modificar (`omp_set_lock(&prev->lock)`), bloqueamos el nodo necesario.
- Si detectamos un cambio (lista inconsistente), liberamos el lock y repetimos la operación desde el principio.

![[Pasted image 20241202160239.png]]

es posible que otro thread modifique la lista mientras estamos buscando el punto de inserción. Por ello, antes de realizar cualquier cambio, **validamos el estado actual de la lista**.

Esto se logra verificando que `prev->next` sigue siendo el mismo puntero que observamos durante la búsqueda:

![[Pasted image 20241202160346.png]]


### (OPCIONAL)
### **¿Por qué estos cambios mejoran el rendimiento?**

1. **Menor contención por locks:**
    
    - Solo se bloquean nodos cuando es estrictamente necesario (inserción o actualización).
    - La búsqueda se realiza de manera no bloqueante.
2. **Menos conflictos entre threads:**
    
    - La división de trabajo asigna claves de forma exclusiva a cada thread, reduciendo la probabilidad de que múltiples threads accedan al mismo bucket simultáneamente.
3. **Validación especulativa:**
    
    - Si ocurre una colisión en la tabla hash, repetimos la operación sin interrumpir a otros threads. Esto asegura que el programa avance incluso bajo alta contención.
4. **Escalabilidad:**
    
    - El trabajo paralelo está mejor distribuido, lo que mejora el rendimiento en sistemas con múltiples núcleos.



![[Pasted image 20241202162152.png]]
![[Pasted image 20241202213355.png]]

[[eje6 shell]]

# Ejercicio 7

```
La siguiente instrucción en lenguaje ensamblador, obtenida del código anterior, es crucial para realizar la operación atómica: una instrucción de máquina `cmpxchg` (comparar e intercambiar) precedida por un prefijo de lock, señalando al procesador que la instrucción debe ejecutarse atómicamente: (1) leer de la memoria, (2) comparar el valor leído de la memoria con el valor proporcionado, y si la comparación es válida, (3) intercambiar los valores, y (4) actualizar la memoria.

Actividad 7. Completa la implementación del diseño sin locks y codifícalo en un nuevo programa, HT4.cpp. Explica cómo la versión final asegura que la funcionalidad siempre será correcta. Mide el rendimiento de la ejecución con un tamaño de problema N = 200K, H = 7, MAX = 100K, con T=1, 6 y 12 hilos.
```

La idea es usar variables atómicas para proteger las operaciones críticas como actualizaciones de los contadores o cambios en los punteros, permitiendo concurrencia segura sin locks.

[[eje7 shell]]

- Antes, los contadores y punteros se protegían con bloqueos.
- Ahora, usan variables atómicas para eliminar la necesidad de bloqueos explícitos y evitando condiciones de carrera.

```cpp
struct node {
    unsigned key;                     // Clave almacenada
    atomic<unsigned> count;           // Contador de ocurrencias, ahora atómico
    atomic<node *> next;              // Puntero al siguiente nodo, ahora atómico
    node() : key(0), count(0), next(nullptr) {} // Constructor sin locks
};

```
![[Pasted image 20241202212424.png]]
![[
]]
![[Pasted image 20241202212440.png]]

![[Pasted image 20241202212615.png]]