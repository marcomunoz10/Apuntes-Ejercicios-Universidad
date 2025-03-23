
Para actualizar tu sistema Debian, sigue estos comandos en terminal:

1. **Actualizar la lista de paquetes**:
   ```bash
   sudo apt update
   ```

2. **Actualizar todos los paquetes instalados**:
   ```bash
   sudo apt upgrade
   ```

3. **Actualizar todo, incluyendo actualizaciones que puedan añadir o eliminar paquetes**:
   ```bash
   sudo apt full-upgrade
   ```

4. **Eliminar paquetes obsoletos** (opcional):
   ```bash
   sudo apt autoremove
   ```


---

La carpeta **`/etc`** en un sistema Linux (o Unix) es un directorio muy importante que contiene archivos de configuración del sistema y de las aplicaciones instaladas.
Imp. utilizar ``root`

---

### systemctl


1. **Iniciar un servicio**:  
   ```bash
   sudo systemctl start <nombre_servicio>
   ```

2. **Detener un servicio**:  
   ```bash
   sudo systemctl stop <nombre_servicio>
   ```

3. **Reiniciar un servicio**:  
   ```bash
   sudo systemctl restart <nombre_servicio>
   ```

4. **Habilitar un servicio en el arranque**:  
   ```bash
   sudo systemctl enable <nombre_servicio>
   ```

5. **Deshabilitar un servicio del arranque**:  
   ```bash
   sudo systemctl disable <nombre_servicio>
   ```

6. **Ver el estado de un servicio**:  
   ```bash
   sudo systemctl status <nombre_servicio>
   ```

7. **Ver todos los servicios activos**:  
   ```bash
   sudo systemctl list-units --type=service
   ```

Estas cubren la mayoría de las operaciones básicas para gestionar servicios en Debian.
La diferencia entre **parar** y **deshabilitar** un servicio es:

- **Parar** (`systemctl stop <servicio>`): Detiene el servicio en el momento, pero puede volver a iniciarse en el próximo arranque o manualmente.

- **Deshabilitar** (`systemctl disable <servicio>`): Evita que el servicio se inicie automáticamente al arrancar el sistema, pero no lo detiene si ya está en ejecución.

Para detener y evitar que arranque automáticamente, necesitas usar ambos comandos: `stop` y luego `disable`.


---

### Journaltcl

journalctl es una herramienta integrada en systemd que muestra los [[logs]] de eventos y 
servicios.

1. **Ver Todos los Registros**:
   ```bash
   journalctl
   ```

2. **Ver Registros de un Servicio Específico**:
   ```bash
   journalctl -u nombre-del-servicio
   ```
   (Reemplaza `nombre-del-servicio` con el nombre del servicio que deseas consultar).

3. **Filtrar por Prioridad**:
   ```bash
   journalctl -p err
   ```
   (Esto mostrará solo los registros de nivel de error o superior).

4. **Monitorear en Tiempo Real**:
   ```bash
   journalctl -f
   ```
-f de follow.

5. **Limitar el Número de Líneas Mostradas**:
   ```bash
   journalctl -n 100
   ```
   (Esto mostrará solo las últimas 100 líneas de los registros).

6. Para mostrar los logs desde el último arranque
```bash
journalctl -b
```
-b de boot(arranque), por eso se puede hacer --boot
8. **Ver Registros de un Periodo Específico**:
   ```bash
   journalctl --since "2024-09-01" --until "2024-09-28"
   ```

---
### Componentes adicionales
Para comprobar los componentes y funcionalidades que mencionas sobre `systemd`, puedes utilizar una serie de comandos y técnicas. Aquí te explico cómo verificar cada uno:

1. **Systemd-journald**:
   - Verifica que `systemd-journald` esté activo:
     ```bash
     systemctl status systemd-journald
     ```
   - Para ver los logs gestionados por este daemon:
     ```bash
     journalctl
     ```

2. **Systemd-logind**:
   - Comprueba el estado de `systemd-logind`:
     ```bash
     systemctl status systemd-logind
     ```

3. **Systemd-networkd**:
   - Verifica el estado de `systemd-networkd`:
     ```bash
     systemctl status systemd-networkd
     ```
   - Para analizar los enlaces de red:
     ```bash
     networkctl
     ```


5. **Udev**:
   - Verifica el estado de `udev`:
     ```bash
     systemctl status systemd-udevd
     ```
   - Para listar los dispositivos gestionados:
     ```bash
     ls /dev
     ```


---

El directorio `/usr/lib` en sistemas Linux contiene bibliotecas y archivos de soporte para programas y aplicaciones.

---
En [[Systemd]] un `target` es una unidad especial que agrupa otros servicios para definir un estado o un nivel de operación de sistema.
Cuando se activa un target se activan los servicios y unidades que se hayan definido como dependencias de ese target. 

Aislar un target en `systemd` sirve para cambiar el estado del sistema a un entorno específico, controlando qué servicios y unidades se inician o se detienen. Aquí hay algunas razones y beneficios de aislar un target:

### Razones para Aislar un Target

1. **Cambiar el Entorno Operativo**:
    
    - Permite cambiar rápidamente entre diferentes modos de operación del sistema, como pasar de un entorno gráfico (`graphical.target`) a un entorno de solo texto (`multi-user.target`).


---
`udev info -a -n /dev/sda

Desglose del Comando
- **`udevadm`**: Herramienta para interactuar con `udev`, el gestor de dispositivos en Linux.
- **`info`**: Modo para mostrar información sobre un dispositivo.
- **`-a`**: Muestra todos los atributos de los dispositivos padre e hijo asociados.
- **`-n /dev/sda`**: Especifica el dispositivo (`/dev/sda`, normalmente el disco principal) para el que se quiere obtener la información.
---
**Estàtics**: Contenen arxius que no canvien sense la intervenció del 
root, i poden ser llegits per qualsevol altre usuari /bin, /sbin, /opt, /boot, /usr/bin
`/bin` de binario --> son comandos cortos (ls)
`/opt` de opcional
`/sbin` contiene **ejecutables y scripts de administración del sistema** esenciales
`/boot` de arranque
`/usr/bin` **No críticos para el arranque**: A diferencia de `/bin` o `/sbin`, los programas en `/usr/bin` no son necesarios para que el sistema arranque, ya que se montan después.

**Dinàmics**: Es creen en el moment de l'arrancada o en el moment q es genera quan es 
crea nova informació. Contenen arxius que canvien, i poden llegir-se i escriure's (pel seu respectiu usuari o root). Contenen configuracions, documents, etc. /var/mail, /var/spool, /var/run, /var/lock, /home...

**Compartits**: Contenen arxius que es troben en un ordinador i poden utilitzar-se en un altre, o fins i tot compartir-se entre usuaris.

Restringits: Contenen fitxers que no es poden compartir, i només poder ser modificats per l'administrador /etc, /boot ...

---

`tree -d -L 1`

muestra en estructura de árbol todos los directorios `-d` limitada a nivel 1 de profundidad

---

`sudo mount -t ext4 /dev/sdb1 /mnt/usb`

* `/dev/sdb1` el dispositivo o partición que se monta
* `/mnt/usb` donde se monta
* `-t ext4` especifica el tipo de archivo que se monta

Para ver todos los sistemas de archivos montados: `mount`

El archivo `/etc/fstab` es el archivo que define como y donde se ##montan los sistemas de archivos y dispositivos al iniciar el sistema como root.
El archivo `/etc/mtab` muestra los archivos que hay actualmente mostrados en el sistema.


`more` se utiliza para ver archivos largos 

`chown` root puede cambiar dueño de archivo
`chgrp` root puede cambiar grupo de archivo
`file _archivo_` te dice el tipo de fichero que es
`ls -la = ls -al` lista detallada de todos los archivos

---
### Administració local: ordres habituals

1. Instalar el servicio sysstal, habilitar el servicio y reiniciarlo con restart en systemctl.

`sar`: genera informes sobre la actividad del sistema.
![[Pasted image 20241027095752.png]]

`iostat`: información sobre utilización de CPU y estadísticas sobre discos.
![[Pasted image 20241027095842.png]]

`pidstat` muestra estadísticas de rendimiento y uso de recursos **por cada proceso** en ejecución.
![[Pasted image 20241027100205.png]]

`vmstat` estadísticas sobre el uso de la memoria.
![[Pasted image 20241027100333.png]]

los registros están en `/var/log`
**`gnome-logs`** es una interfaz gráfica que  accede automáticamente a los registros de `systemd` que están en `/var/log`

`logrotate` es una herramienta en Linux que se utiliza para gestionar el tamaño de los archivos de registro (logs).

- **`lscpu`**: Muestra información sobre la **CPU** (núcleos, hilos, frecuencia).![[Pasted image 20241027104804.png]]

- **`hwinfo`**: Proporciona detalles sobre **todo el hardware** del sistema.

- **`lsmod`**: Lista los **módulos del kernel** cargados actualmente.
![[Pasted image 20241027104829.png]]

- **`lspci`**: Muestra información sobre los **dispositivos PCI** (tarjetas de red, gráficos, etc.).
- **`lsusb`**: Lista los **dispositivos USB** conectados.
- **`ps`**: Muestra una lista de **procesos en ejecución**.
 ![[Pasted image 20241027104847.png]]
Muestra la lista en el momento en q ejecuto el comando, ps -e para verlos todos, ps aux para mostrar - **`a`**: Muestra procesos de todos los usuarios. **`u`**: Muestra la salida en un formato de usuario amigable. **`x`**: Incluye procesos que no están asociados a una terminal (como servicios en segundo plano).

- **`top`**: Muestra en tiempo real los **procesos y su uso de recursos** ordenada por utilización de cpu.
![[Pasted image 20241027105151.png]]
- **`free`**: Muestra el **uso de memoria RAM y swap**.
![[Pasted image 20241027105234.png]]
- **`du`**: Informa del **uso de espacio en disco** por archivo o directorio.
![[Pasted image 20241027105245.png]]
- **`df`**: Muestra el **espacio libre y ocupado** en los sistemas de archivos.
![[Pasted image 20241027105257.png]]
 ---
#### Network Manager

Daemon q intenta q la configuración sea lo más automático y transparente posible.

Importante. Si se gestiona desde network manager se tienen q desactivar todas las configuraciones de red desde `/etc/network/interface` para evitar conflictos.

Una vez que desactivado o eliminado las configuraciones en `/etc/network/interfaces`,gestionar todas tus conexiones de red a través de Network Manager, ya sea utilizando la GUI o la línea de comandos (`nmcli`).

  ```bash
  sudo systemctl restart NetworkManager
  ```


en `/etc/NetworkManager/NetworkManager.conf ` tener la opcion `managed=true`.

---
Las nuevas versiones de ubuntu tienen `netplan` utilizando archivos `yaml`
Permite escribir expresiones lógicas y legibles, es más simple que `NetworkManager` y ``
`snaps` son paquetes de sw q contienen todo lo necesario (dependencias) para ejecutar una aplicación.
`snapd` es el daemon q permite la instalación, actualización y eliminación de los `snaps` del sistema. No interacciona con el SO, compacta todo en un disco virtual. Gestiona las aplicaciones fuera del `apt`. Permite que las librerías evolucionen 

---
### DNS
para configurar servidores dns archivo `etc/resolv.conf`  y poner lineas del tipo:
`nameserver 10.10.10.10` es la dirección del servidor DNS que quiero utilizar. 
o en el archivo `/etc/network/interfaces` poner `dns-nameserves`


1. Configuración de DNS básica. En el archivo `etc/resolv.conf`se define el servidor DNS, por ejemplo:
```shell
nameserver 8.8.8.8
```

2. Para probar resolución de nombres externos probar con:
```shell
dig debian.com @localhost
```
Poner localhost indica que quieres ejecutar el DNS que se está ejecutando en tu propia máquina en vez de resolverlo con DNS externo.

3. Se puede añadir una máquina en `/etc/hosts` para resolver el DNS de manera interna.
```shell
<ip> <ruta dns> <alias>
8.8.8.8 google.com google
```

luego, en bash pondrás:
```shell
dig google @localhost
```
Para decirle que lo haga de manera interna

`ip a` (de ip addres) Muestra todas las interfaces de red y sus direcciones IP asociadas.
![[Pasted image 20241027135623.png]]

`ip r`  (de ip routing) Gestiona la tabla de rutas del sistema. 

Añadir una ruta:
```shell
ip route add <red>/<máscara> via <gateway> dev <interfaz>
```

Eliminar una ruta:
```shell
ip del <red>/<máscara>
```
![[Pasted image 20241027135908.png]]

---
`sysctl` permite configurar parámetros de kernel.
Se puede utilizar para eliminar ipv6

Claro, te explico cada parte del proceso para deshabilitar **IPv6** en un sistema Linux utilizando el comando `sysctl`:

```bash
echo "net.ipv6.conf.all.disable_ipv6 = 1" >> /etc/sysctl.conf
```
- **`echo`**: Este comando se utiliza para mostrar texto en la terminal.
- **`"net.ipv6.conf.all.disable_ipv6 = 1"`**: Es el texto que se va a añadir. Esta línea establece que se desactive IPv6 para todas las interfaces de red del sistema.
  - **`net.ipv6.conf.all.disable_ipv6`**: Este es el parámetro que controla la desactivación de IPv6.
  - **`= 1`**: Indica que se debe deshabilitar IPv6 (1 significa "deshabilitado").
- **`>> /etc/sysctl.conf`**: Esto redirige el texto que se está mostrando hacia el archivo **`/etc/sysctl.conf`**, añadiéndolo al final del archivo. Este archivo se utiliza para almacenar configuraciones de `sysctl`, y los cambios en él se aplicarán cada vez que se inicie el sistema.

```bash
sysctl -p
```
- **`sysctl -p`**: Este comando carga y aplica las configuraciones del archivo **`/etc/sysctl.conf`**. Al ejecutar este comando, el sistema lee el archivo de configuración y aplica cualquier parámetro definido en él. En este caso, deshabilitará IPv6 de acuerdo con la línea que añadiste.

---
WAP = *wireless acces point* Dispositivo que permite la conexión de dispositivos inalámbricos a una red cableada

`ssid` nombre que recibe la red.

---
Un **gateway** (o **puerta de enlace**) es un dispositivo o nodo que sirve como punto de conexión entre dos redes.

Con **ip_forwarding** el usuario recibe paquetes desde una interfaz y los reenvía a otra
Para habilitar el **ip_forwarding** se puede hacer de manera temporal:
`echo 1 > /proc/sys/net/ipv4/ip_forwarding`

Para habilitar IP forwarding **de manera permanente**, debes editar el archivo de configuración `/etc/sysctl.conf` y añadir o modificar la línea: `net.ipv4.ip_forward = 1`

Para recargar la configuración `sysctl -p`. Lo de `-p` es para cargar los `parámetros`.


---
#### Masquerading

Concepto: esconde todo el rango de.
`iptables` se utiliza para gestionar reglas de firewall y de NAT en linux. Para utilizarlo tengo que poner el path de /sbin/iptables. **hace falta ser root**

```shell
iptables -t nat -A POSTROUTING –o eth0 –s 172.16.0.0/16 -j MASQUERADE
```

`-t nat` = tipo NAT; indica que la regla se aplica en la tabla de nat
`-A` significa `append`, añadir algo al final.
`POSTROUTING` para manipular los paquetes de datos justo **después de que hayan sido enrutados**. Es decir, antes de que salgan por la interfaz de red. 
`-o` de **output**, para cambiar la interfaz de salida del router hacia internet.
`-s` de **source**, para indicar la subred privada para hacer la traducción.
`-j` de **jump**, los paquetes tendrán que saltar de interfaz
`MASQUERADE` remplaza la `ip` privada por la `ip` pública.

#### Prerouting
Regla para los paquetes que llegan desde internet al router. 

`SNAT` tipo de NAT que sirve para cambiar de dirección de los paquetes que llegan desde internet.

```shell

iptables -t nat -A PREROUTING -p tcp -i eth1 --dport 80 -j DNAT --to-destination 10.1.1.1:80
```

para listar las reglas de `iptables`: `sudo /sbin/iptables -L -n | less`
`-L` de listar.
`-n` para que no intente resolver dns. es decir, no intentará resolver direcciones de IP a nombres de host.
`less` para verlo todo de mejor manera, te muestra solamente la salida



---
### DHCP

Banda del **cliente**
Instalar `apt-get install isc-dhcp-client`
en la carpeta `/etc/network/interfaces` añadir un `dhcp` al final de la linea.
```shell
iface enp0s3 inet dhcp
```
donde `enp0s3` es el nombre de la interfaz.

Banda del **servidor**: 
Instalar `apt-get install isc-dhcp-server`
carpeta `etc/dhcp/dhcp.config` 
Ver la actividad del servidor
```shell
/usr/sbin/dhcp -d -f
```
* `usr/sbin/dchp` es el ejecutable del servidor
* `-d` pone servidor en modo `debugging` (depuración)
* `-f` de `foreground`, quiere decir q el servidor permanecerá en la terminal actual y el administrador podrá ver todo lo que ocurra.


**Cuando DCHP no está en tu subred**
`DHCP RELAY` --> el router recibe en formato broadcast y envía unicast al servidor dhcp.
La razón por la que se utiliza un unicast en este caso es que el servidor DHCP no puede recibir mensajes broadcast en subredes diferentes de la que se encuentra. Luego reelay reenvia como broadcast. 
Instalar `apt-get install isc-dhcp-relay
Configurar `etc/default/isc-dhcp-relay` con:
```shell
SERVERS="176.16.1.1"
```
que es la dirección donde se tienen q enviar peticiones.


---
#### SSH

**USUARIO**
`ssh-keygen   -t   rsa|dsa`
L'usuari podria copiar la clau pública (id_rsa.pub) en l'arxiu  $HOME/.ssh/authorized_keys de la 
màquina remota o fer servir l’ordre ssh-copy-id usuari@màquina.
Carpetas: `/etc/ssh/ssh_config` i 
para conectarse `ssh user@host`
Necesito entorno gráfico para hacer `-X`
Para ejecutar aplicaciones gráficas utilizar parámetro `-X`
`ssh -X host_remot`

**SERVIDOR**
`/etc/ssh/sshd_config.`
La `d` es de daemon, escucha por el puerto 22 en la máquina que va a aceptar conexiones entrante. 
`PermitRootLogin  yes|no` per  a  permetre  la  connexió  de  l'usuari  root  o  no

`IgnoreRhosts  yes ` per  a  evitar  llegir  els  arxius  $HOME/.rhosts i  $HOME/.shosts  dels  usuaris

`X11Forwarding  yes`  per  a  permetre  que  aplicacions  Xwindows  en  el  servidor  es  visualitzin  a  la  pantalla  del  client. 
No necesito entorno gráfico si quiero que se conecten a mí con -X, necesito la biblioteca X11

Sirve para túneles VPN sobre SSL

---

### NFS
Para poder compartir ficheros de forma remota. Utiliza RPC (*remote procedure call*)

**Servidor**
```shell
apt-get install nfs-kernel-server
sudo systemctl enable nfs-kernel-server
nano /etc/idmpap.conf # cambiar el dominio
```


**Cliente**
Siendo **root**:
```shell
apt-get install nfs-common
#crear un directori local
sudo mount -t nfs ip_servidor:ruta_dir_remota directori_local
```
Para hacer el montaje automático, en `/etc/fstab` añadir una linea tipo:
```shell
ip_servidor:ruta_dir_remota directorio_local nfs defaults 0 0
```

**Servidor**
Definir las carpetas que se pueden compartir con clientes. Actúa como una ACL (*address control list*) en la carpeta `etc/exports`:

![[Pasted image 20241028173851.png]]


---
### Gluster FS
```shell
gluster peer probe node2
```
Permite que `node 2` sea un nodo activo en el gluster
Cada nodo es considerado como un igual, `peer`
`probe` detectar un nuevo nodo.

A continuación se explica cada uno de los comandos y parámetros que mencionas en el contexto de GlusterFS, así como sus funciones y propósitos.

### 1. `gluster peer status`
- **Propósito**: Este comando se utiliza para mostrar el estado actual de los nodos que forman parte del cluster de GlusterFS.
- **Salida**: Proporciona información sobre cada nodo en el cluster, incluyendo si están conectados (peers), su estado y si están en modo de partición (partitioned).
- **Uso**: Permite a los administradores verificar la conectividad y el estado de los peers en el cluster.

##### 2. `gluster volume create vol_distributed transport tcp \ node01:/var/lib/glusterfs/distributed \ node02:/var/lib/glusterfs/distributed`
- **Propósito**: Este comando crea un nuevo volumen de GlusterFS llamado `vol_distributed`, que utiliza la configuración de distribución entre dos nodos (node01 y node02).
- **Parámetros**:
  - `vol_distributed`: Nombre del volumen que se está creando.
  - `transport tcp`: Especifica que la comunicación entre los nodos se realizará a través del protocolo TCP.
  - `node01:/var/lib/glusterfs/distributed`: Define la ruta del directorio donde se almacenará el volumen en node01.
  - `node02:/var/lib/glusterfs/distributed`: Define la ruta del directorio donde se almacenará el volumen en node02.
- **Uso**: Permite configurar un volumen distribuido que almacena datos en ambos nodos, ofreciendo redundancia y disponibilidad.

### 3. `gluster volume start vol_distributed`
- **Propósito**: Inicia el volumen GlusterFS llamado `vol_distributed` después de haberlo creado.
- **Uso**: Es necesario ejecutar este comando para que el volumen esté disponible para su uso después de ser creado.

### 4. `gluster volume info`
- **Propósito**: Muestra información detallada sobre los volúmenes de GlusterFS configurados.
- **Salida**: Proporciona detalles como el nombre del volumen, el tipo, el estado, la cantidad de bricks (unidades de almacenamiento), y la configuración actual del volumen.
- **Uso**: Ayuda a los administradores a obtener un resumen del estado y la configuración de los volúmenes existentes en el cluster.

### 5. `gluster volume set vol_distributed nfs.disable off`
- **Propósito**: Habilita el acceso NFS para el volumen `vol_distributed`.
- **Parámetro**:
  - `nfs.disable off`: Al configurar este parámetro a `off`, se permite que el volumen sea accesible a través de NFS.
- **Uso**: Facilita el acceso al volumen mediante el protocolo NFS, permitiendo que los clientes que usan NFS puedan montar el volumen.

### 6. `apt -y install glusterfs-client`
- **Propósito**: Instala el cliente GlusterFS en la máquina cliente que necesita acceder al volumen.
- **Parámetro**:
  - `-y`: Automatiza la instalación al responder "sí" a todas las preguntas de confirmación.
- **Uso**: Es necesario tener instalado el cliente GlusterFS en los nodos que desean montar y acceder a los volúmenes de GlusterFS.

### 7. `mount -t glusterfs node01.nteum.org:/vol_distributed /mnt`
- **Propósito**: Monta el volumen `vol_distributed` en el directorio local `/mnt` en el cliente.
- **Parámetros**:
  - `-t glusterfs`: Especifica que el tipo de sistema de archivos a montar es GlusterFS.
  - `node01.nteum.org:/vol_distributed`: Indica la ubicación del volumen que se desea montar, especificando el nodo donde está ubicado el volumen.
  - `/mnt`: Especifica el punto de montaje local donde se accederán los datos.
- **Uso**: Permite a los usuarios acceder a los datos almacenados en el volumen GlusterFS montado en su sistema.

### 8. `df -hT`
- **Propósito**: Muestra información sobre el uso del sistema de archivos en el sistema.
- **Parámetros**:
  - `-h`: Muestra el uso del disco en un formato legible (por ejemplo, GB).
  - `-T`: Incluye el tipo de sistema de archivos en la salida.
- **Uso**: Proporciona información sobre el espacio utilizado y disponible en los sistemas de archivos montados, incluidos los volúmenes de GlusterFS.

### 9. `systemctl enable rpcbind`
- **Propósito**: Habilita el servicio `rpcbind` para que se inicie automáticamente al arrancar el sistema.
- **Uso**: Es necesario para que los clientes NFS puedan realizar correctamente las llamadas RPC (Remote Procedure Call) para acceder a los volúmenes NFS.

### 10. `mount -t nfs -o mountvers=3 node01.nteum.org:/vol_distributed /mnt`
- **Propósito**: Monta el volumen NFS de GlusterFS en el directorio local `/mnt`.
- **Parámetros**:
  - `-t nfs`: Especifica que se trata de un sistema de archivos NFS.
  - `-o mountvers=3`: Establece que se debe utilizar la versión 3 del protocolo NFS.
- **Uso**: Permite a los clientes acceder al volumen utilizando el protocolo NFS, en lugar de GlusterFS directamente.


---
### Cloud Storage
```shell
docker run -d -p 8080:8080 owncloud/server
```

- **`docker run`**: Ejecuta un nuevo contenedor en Docker.
    
- **`-d` (detached mode)**: Ejecuta el contenedor en segundo plano, permitiendo que la terminal quede libre para otros comandos.
    
- **`-p 8080:8080`**: Mapea el puerto 8080 del host (primer 8080) al puerto 8080 del contenedor (segundo 8080). Esto hace que la aplicación de ownCloud sea accesible a través del puerto 8080 en la máquina host.
    
- **`owncloud/server`**: Especifica la imagen que se va a usar para el contenedor, en este caso, la imagen oficial de `ownCloud` alojada en Docker Hub.


---
### CDN (*content delivery netrwork*)

Red distribuida geográficamente para aumentar el rendimiento y disponibilidad.
POP = punto de presencia.
`ICAP` internet content adaptation protocol. Estandar abierto para la conexión de servidores de aplicaciones.

---
### Chroot
`chroot` es un comando y técnica en Linux que permite cambiar el directorio raíz aparente de un proceso o conjunto de procesos, de modo que esos procesos creen que el directorio especificado es la raíz del sistema (`/`). Esto significa que esos procesos no pueden acceder a archivos o directorios fuera del entorno raíz especificado, proporcionando una forma de aislamiento o "jaula" de sistema de archivos.

---
reverse proxy distribuye la carga mientras que forward proxy es un gateway.

----
### Cgroups
`systemd-cgls `ens mostrarà un arbre dels grups i les seves dependències.
`systemd-cgtop` para monitorizar en tiempo real.

---
#### swarm
- **Manager**: El nodo que ejecuta el comando `docker swarm init` se convierte en un **manager**. Este nodo es responsable de gestionar el estado del swarm, programar contenedores en los nodos disponibles y manejar la configuración de servicios. Los managers tienen la capacidad de recibir y procesar comandos, así como de tomar decisiones sobre la distribución de tareas entre nodos.
    
- **Worker**: Los nodos adicionales que se unen al swarm (usando el comando `docker swarm join`) se convierten en **workers**. Estos nodos ejecutan los contenedores pero no toman decisiones sobre la orquestación. Solo reciben instrucciones del nodo manager.

**Nodo** Recibe y ejecuta tareas. 
Dos tipos de nodos: `manager`: envía tareas a los nodos de trabajo `worker`. (para ello configurar en modo solo gestor, ya que tambien podria tratar las tareas pero pa q si puedes tener trabajadores)

`worker` nodo que recibe y ejecuta tareas. Envía notif al nodo gestor del estado actual de las tareas.

**Servicio** conjunto de tareas a ejecutar. Cuando se crea un serv espec la imagen de contenedor q se utilizará.

Aquí tienes una explicación de cada uno de los comandos que mencionaste:

### 1. **Iniciar el Swarm**
```bash
docker swarm init --advertise-addr 20.20.20.20
```
Este comando inicializa un nuevo swarm de Docker, convirtiendo el nodo actual en un manager. El parámetro `--advertise-addr` especifica la dirección IP que otros nodos usarán para unirse al swarm.

### 2. **Mensaje de inicialización**
```
Swarm initialized: current node (dxn1zf6l61qsb1josjja83ngz) is now a manager.
```
Este es un mensaje de confirmación que indica que el nodo actual se ha convertido en un manager del swarm.

### 3. **Unir un nodo trabajador**
```bash
docker swarm join --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c 20.20.20.20:2377
```
Este comando permite que un nodo adicional se una al swarm como trabajador. Necesita el token generado en la inicialización del swarm y la dirección del nodo manager (en este caso, `20.20.20.20` en el puerto `2377`).

### 4. **Mostrar información del swarm**
```bash
docker info
```
Muestra información general sobre la instalación de Docker, incluidos los detalles del swarm, como el número de nodos y servicios.

### 5. **Listar nodos en el swarm**
```bash
docker node ls
```
Lista todos los nodos que forman parte del swarm, mostrando su estado, rol (manager o worker) y disponibilidad.

### 6. **Obtener el token de unión para trabajadores**
```bash
docker swarm join-token worker
```
Muestra el comando necesario para que un nuevo nodo se una al swarm como worker, incluyendo el token necesario.

### 7. **Crear un servicio**
```bash
docker service create --replicas 1 --name helloworld alpine ping docker.com
```
Crea un nuevo servicio llamado `helloworld` utilizando la imagen `alpine`. Inicia una réplica del contenedor que ejecuta el comando `ping docker.com`.

### 8. **Listar servicios**
```bash
docker service ls
```
Muestra todos los servicios que se están ejecutando actualmente en el swarm, junto con detalles como el número de réplicas.

### 9. **Inspeccionar un servicio**
```bash
docker service inspect --pretty helloworld
```
Proporciona detalles más específicos sobre el servicio `helloworld`, mostrando información de forma legible.

### 10. **Listar tareas de un servicio**
```bash
docker service ps helloworld
```
Muestra las tareas asociadas con el servicio `helloworld`, incluyendo su estado y en qué nodo están ejecutándose.

### 11. **Escalar un servicio**
```bash
docker service scale helloworld=5
```
Aumenta el número de réplicas del servicio `helloworld` a 5. Esto ejecutará 5 instancias del contenedor.

### 12. **Eliminar un servicio**
```bash
docker service rm helloworld
```
Elimina el servicio `helloworld` del swarm, deteniendo y eliminando todas las réplicas asociadas.

### 13. **Actualizar disponibilidad de un nodo**
```bash
docker node update --availability drain worker1
```
Cambia el estado de disponibilidad del nodo `worker1` a `drain`, lo que significa que no aceptará nuevas tareas y se eliminarán las tareas existentes.

### 14. **Reactivar un nodo**
```bash
docker node update --availability active worker1
```
Cambia la disponibilidad del nodo `worker1` a `active`, permitiendo que vuelva a recibir tareas.

### 15. **Crear un servicio web**
```bash
docker service create --name my-web --publish published=8080,target=80 --replicas 2 nginx
```
Crea un servicio llamado `my-web` usando la imagen `nginx`, publicando el puerto `8080` del host al puerto `80` del contenedor, y ejecutando 2 réplicas.

### 16. **Inspeccionar puertos de un servicio**
```bash
docker service inspect --format="{{json .Endpoint.Spec.Ports}}" my-web
```
Muestra en formato JSON los detalles sobre los puertos del servicio `my-web`, específicamente qué puertos están expuestos.

Si necesitas más detalles sobre algún comando en particular o algún otro tema, ¡déjamelo saber!