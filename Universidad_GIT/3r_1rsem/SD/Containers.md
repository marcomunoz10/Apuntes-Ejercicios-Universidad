Aunque comparten el kernel, los contenedores pueden tener diferentes **entornos de usuario** y bibliotecas, lo que permite que "parezcan" tener diferentes sistemas operativos.

Por ejemplo, en un host Linux, puedes tener contenedores con:

- Una distribución basada en Ubuntu.
- Una distribución basada en Alpine.
- Una distribución basada en CentOS.

### ¿Qué es la virtualización?

La **virtualización** es la técnica que permite ejecutar múltiples sistemas operativos o entornos aislados sobre un único hardware físico. Esto se logra mediante la creación de "máquinas virtuales" (VM), que tienen su propio sistema operativo y funcionan como si fueran computadoras independientes.

- Una máquina virtual tiene su propio **kernel** (núcleo del sistema operativo).
- Esto requiere un **hipervisor** (como VMware, VirtualBox, o Hyper-V), que actúa como intermediario entre el hardware físico y las máquinas virtuales.

### ¿Qué es un contenedor?

Un **contenedor** es un entorno aislado que empaqueta una aplicación y todas sus dependencias (bibliotecas, configuraciones, etc.) para que pueda ejecutarse de manera consistente en cualquier lugar.

- **Diferencia clave:** Los contenedores **no incluyen un kernel propio**. En lugar de eso, comparten el **kernel del sistema operativo del host**.

# Docker
Docker es una plataforma que permite crear contenedores.

Si el sistema operativo del host no es compatible con el contenedor (por ejemplo, ejecutando un contenedor Linux en Windows), Docker necesita usar una máquina virtual para proporcionar un kernel Linux, porque el kernel de Windows no puede ejecutar directamente procesos de Linux.

Si instalas Docker Desktop en Windows, se utiliza una máquina virtual para ejecutar contenedores Linux. Estos contenedores ofrecen aislamiento, portabilidad y compatibilidad total con Linux.

### Objetos de Docker
1. Imágener (snapshot de un contenedor)
2. Contenedores
3. Redes
4. Volúmenes
5. Dockerfile

El **Docker Host** es el sistema físico o virtual donde se ejecuta el motor de Docker (**Docker Engine**) y, por ende, donde se crean, ejecutan y gestionan los contenedores.

Los datos no persisten cuando se detiene ese contenedor.

- Es un desafío extraer los datos del contenedor si otro proceso los necesita.
- El rendimiento se ve afectado porque la capa de escritura requiere un controlador del kernel para gestionar el sistema de archivos.