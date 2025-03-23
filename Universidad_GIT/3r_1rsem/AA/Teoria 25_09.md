(página 12 PDF)
### 1. Multiprocesador:

Definición: Es un sistema en el que varios procesadores comparten una única memoria y recursos periféricos. Se caracteriza por tener una arquitectura tightly coupled (acoplada estrechamente).
Memoria: Los procesadores comparten una memoria común, lo que permite una mayor coherencia y sincronización entre ellos.
Comunicación: Los procesadores se comunican a través de la memoria compartida. La comunicación es rápida ya que se encuentran en un mismo sistema.
Escalabilidad: Tiene limitaciones en cuanto a la escalabilidad, ya que añadir más procesadores aumenta la complejidad de gestionar la memoria compartida y los recursos.
Ejemplo: Ordenadores con múltiples núcleos o servidores con varios procesadores en una misma máquina.

**(1) Modelo de programación paralela = Memoria compartida**
Threads que están incluidos en un proceso

### 2. Multicomputador:

Definición: Es un sistema formado por varios ordenadores independientes que se conectan entre sí mediante una red, cada uno con su propia memoria y recursos. Se caracteriza por una arquitectura loosely coupled (acoplada de forma laxa).
Memoria: Cada computadora tiene su propia memoria local. No hay una memoria compartida entre los procesadores.
Comunicación: Los sistemas se comunican a través de una red de interconexión (como Ethernet), lo que puede ser más lento en comparación con un multiprocesador.
Escalabilidad: Es altamente escalable, ya que se pueden añadir más computadoras sin afectar demasiado el rendimiento del sistema.
Ejemplo: Clústeres de ordenadores, como los sistemas de computación distribuida utilizados en grandes centros de datos.

**(2) Modelo de programación paralela: paso de mensajes**
--> Send/Receive Process ID
Broadcast
--> RPC
Cada proceso se ejecuta en un procesador y entre ellos se comunican.

Resumen:
Multiprocesador: Varios procesadores en una sola máquina con memoria compartida y comunicación rápida.
Multicomputador: Varios sistemas independientes conectados por una red, cada uno con su propia memoria y recursos, lo que permite mayor escalabilidad.


pag 16
### **Stack (Pila)**:

- **Qué es**: Es una región de la memoria que almacena variables locales, direcciones de retorno y el estado de las funciones en ejecución (como el Program Counter y el Stack Pointer). Se organiza en forma de una pila (LIFO: Last In, First Out), donde las variables se apilan y desapilan a medida que las funciones se llaman y se completan.
- **Características**:
    - Cada **hilo (thread)** tiene su propia **pila (stack)**, lo que significa que las variables locales de un hilo son privadas para ese hilo.
    - La memoria en el stack se gestiona de forma automática: cuando una función termina, sus variables locales se eliminan automáticamente.
    - **Rápida asignación** y liberación de memoria, ya que la estructura es muy simple.
    - **Tamaño limitado**: Generalmente, el tamaño de la pila es fijo y mucho menor que el heap.
- **Uso**:
    - Variables locales (privadas para cada hilo).
    - Parámetros de función y direcciones de retorno.
    - Variables que sólo existen durante la ejecución de una función o bloque de código.

### **Heap (Montículo)**:

- **Qué es**: Es una región de memoria de tamaño mucho mayor, donde se almacenan objetos y datos que requieren una vida útil más prolongada y cuya asignación es más flexible. Se gestiona dinámicamente, lo que significa que el programador puede solicitar y liberar memoria manualmente (en lenguajes como C o C++) o mediante un recolector de basura (en lenguajes como Java o Python).
- **Características**:
    - La memoria en el heap es compartida entre todos los hilos, lo que significa que cualquier hilo puede acceder a ella (si tiene la dirección), por lo que es más susceptible a errores de concurrencia como **condiciones de carrera**.
    - Se utiliza para variables dinámicas o globales, y es más flexible, pero su administración es más costosa en términos de rendimiento.
    - **Tamaño grande**: Es mucho más grande que el stack y puede crecer según las necesidades del programa.
- **Uso**:
    - Almacena datos dinámicos (variables que se crean con `malloc`, `new`, etc.).
    - Almacenamiento de objetos grandes o estructuras de datos complejas que deben persistir más allá del tiempo de vida de una función o bloque de código.
    - Puede ser compartido entre hilos si es necesario.

### Relación con **Variables Compartidas** y **Privadas** en Hilos:

- **Variables compartidas**:
    
    - Como mencionas, cuando una variable es **compartida** entre hilos (como una variable estática o global), su dirección es la misma para todos los hilos y se almacenará en el área de datos o en el **heap**.
    - Los hilos pueden acceder a las mismas direcciones de memoria, lo que facilita la comunicación entre ellos, pero requiere mecanismos de sincronización (como mutex o semáforos) para evitar problemas de concurrencia.
- **Variables privadas**:
    
    - Una **variable privada** en cada hilo tendrá una **dirección diferente**, ya sea porque es una variable local en el **stack** de cada hilo, o porque es una variable asignada dinámicamente en el **heap** y la dirección es gestionada por el hilo de forma independiente.
    - Cada hilo tiene su propio conjunto de variables privadas que otros hilos no pueden acceder directamente, lo que garantiza la independencia de su ejecución.

### Resumen:

- El **stack** es privado para cada hilo y almacena variables locales, direcciones de retorno y contexto de ejecución. Es rápido pero tiene un tamaño limitado.
- El **heap** es compartido entre hilos y almacena datos dinámicos o globales que pueden tener una vida más larga, pero su manejo es más complejo y requiere cuidado con la sincronización en sistemas multihilo.