## 01/10/2024

tienes dos máquinas, una servidor y otra cliente.

  

deb2 = 2 (servidor)
deb = 1 (cliente)

1 ping google.com
(tinc sortida)
```bash
sudo -i iptables -t nat -L
sysctl -p
```


2
```bash
ip r add default via (ip deb1)
ip r
ping google.com
more /etc/resolv.com
```

(em de fer servir el de l'autonoma pq el 8.8.8.8 està desinstalat)

1.
```bash
more /etc/host
host mvb.gax.org
```

notfound pq no estic resolvent el domini intern

2. apt install openssh-server
```bash
cd /etc/ssh/sshd_config.d/
cd ..
vi sshd_conf 
systemctl restart ssh
```


1
ssh (nom de la maquina  deb 2)
(per no fer man in de middle agafa un identificador hash shanon 126 per incrementar la seguretat)
```bash

```


2
des de la màquina interna no puc entrar mitjançant ssd a la màquina de la uab, per això utilitzem un túnel per la interfaç gràfica.

```bash
hostnamectl set-hostname deb2

```

1
```bash
ssh adnimp@(ip deb 2)
xeyes 

```
Cant open display.

1
```bash
ssh -X adnimp@(ip deb2)
```

1
```bash
xeyes
xclock
xterm
```

2
tanco terminal

1
```bash
gnome-terminal
```
no deixa 

1
```bash
xterm
```
obre la terminal 2 al terminal 1

2
```bash
ps -edaf | grep xclock
```
posa adminp 3293

2
```bash
kill 3293
```
elimina el procés de mostrar el terminal al terminal1

1
```bash
ssh -X adminp@(ip2)
gnome-terminal
```
ho crea a l'altra màquina

```bash
ssh-keygen --help
```
Si no lo dic res utiliza rsh i el tamany de ¿?
No és recomanable el `dsa` pq es pot trobar la clau pública

```bash
ssh-keygen
```
la clau la possa a `.ssh`

```bash
cd .ssh
more id_rsa
```
em mostra la contrassenya privada

```bash
more id_rsa.pub
```
em mostra la contrassenya pública q és la q hauré de compartir

```bash
ssh-copy-id adminp@(ip2)
```
em podré conectar a la màquina remota, em demana el psswd pq encara no tinc la clau  pública.

```bash
cd .ssh
ssh-keygen
ssh-copy-id adminp@(ip2)
```
la clau la genero en el client i envio la publica al servidor

```bash
ssh -X adminp@(ip2)
```

ara tanco la conexió i torno al localhost

2
mira authorized keys i té password
```bash
cp /dev/null authorized keys
```

## 8 De octubre
### Storage
Discos de 2 o més series (probabilitat de fallada molt baixa) --> [[RAID]]

Chmod 777 para dar permiso a todo a todos los usuarios.

CDN: red distribuida de servidores geográficamente sincronizados para dar contenido. Prevee el contenido q q va a necesitar en tal sitio y lo pone cerca de ese sitio.

ISP: proveedor de internet.

---

# 22/10

```shell
snmpwalk -v 2c -c public (ipmaq)
snmpwalk -v 2c -c mycom (ipmaq)
```


deb2
```shell
sudo -i
more /etc/snmp/snmpd/

```

deb1
```shell
/etc/snmp
snmpwalk -v3 -l authpriv -u pirulo -a SHA -A pirulo2024 -x DES -X pirulo2024 (ipdeb2)
```

`-l` per tenir el protocol sigui encriptat i amb ussuari password

#### Instalar nepdata

```shell
apt install netdata
```

Per instalar-la en local, solament monitoritzo aquesta màquina. Molt flexible. Port 19999

per probar wireshark
```shell
snmpwalk -v 2c -c public (ipmaq)
sudo apt remove --purge netdata
```

copiem la ordre de netdata per monitoritzar la màquina. la maquina administrada inicia la configuració. Podem tenir tantes dashboard com vulguem. També es pot monitoritzar docker i cubernetes. El triger per monitoritzar el fa el sistema gestionat. Instala l'agent amb la clau per visualitzar.

Eina Nagios per monitoritzar.


---

# 24/10/2024
```shell
apt install nagios
```

Ja està tot en marxa. Obrim el navegador i possem localhost/nagios4.

El daemon que dóna vida a això es...

```shell
systemctl nagios4
```

Visualitzem q està actiu.

```shell
systemctl stop nagios
```

Tornem a l'interfície del navegador. No funciona res per l'interacció de php i nagios.