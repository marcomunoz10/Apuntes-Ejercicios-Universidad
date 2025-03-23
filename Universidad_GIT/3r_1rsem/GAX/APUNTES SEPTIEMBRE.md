A continuació tens el text amb les comandes de terminal en **itàlica**:

---

A la MV usuari: **adminp** passwd: **pnimda** usuari: **root** accedir amb **_sudo -i_**

gax-gei-10:CxH7CiewTgJH http://nebulacaos.uab.cat:8443

bleachbit no clickar en memory ni en free disc space

etc sudoers

**_sudo -i_**
**_visudo_** para editar el archivo.  
adminp puede hacer todo sin password.

copypaste.me  
en la máquina hay q poner **_/connect_**

12:11  
**_sudo -1_**  
**_systemctl_** (te da toda la información de todos los procesos)  
units: no són aplicacions, cada unitat pot tenir més d'una aplicació  
**_no--daemon_** no funciona siempre, solamente se pone en marcha cuando tocas la configuración de la red.  
**_snap_**: gestor de paquets que a diferència de l'apt fa càpsules de les aplicacions on cadascuna és una ISO que té el que necessita. Puc tenir diferents versions de sistema operatiu, pot tenir duplicitats.

---

12/09  
Básico haber leído [[Systemd]].


**_systemctl status_** (solament preguntem target i service) si sale degraded poner **_--failed_**  
**_systemctl status ssh_**  
per posar-ho en marxa **_sudo systemctl start ssh_** para activar el ssh  
**_ssh localhost_** para acceder a tu ordenador  
**_sudo -i systemctl stop_** para pararlo

**_systemctl list_**  
el network Manager gestiona tots els dispositius.

**_journalctl_** ens dona tots els missatges d'error  
**_ls /var/log_**  
**_sudo -i_**  
**_dmesg | more_** missatges d'arrancada del sistema

per exemple error acpi que és l'estàndard que controla la freq del processador i el control d'energia de la màquina.

**_cd /var/log_**  
**_journalctl -x -S 2024-09-12_** (en groc warning, en verd informació, en vermell són errors)  
**_gnome-logs_** per mirar-ho en forma gràfica.

**_hostnamectl_** per canviar la informació de la màquina  
**_hostnamectl --help_** per veure opcions  
**_hostnamectl set-hostname_**

**_ls /dev_**  
**_tti_** terminals virtuals

**_/dev/nvme0_** informació  
**_lspci_** per veure informació del dispositiu  
**_hwinfo_**  

**_more /proc/cpuinfo | grep vmx_** (para saber si esta máquina puede virtualizar o no)

**_free -h_**

**_ss -updta | grep LISTEN_**

---

19/09  
SYST D --> NetD  
Udev --> NetD  
NetD --> int  
NetD --> np (netplan)  
NetD --> nm

**_/etc/network_**

**_ip a_** permet veure la màscara  
**_ip r_**  
**_cloud-init_** |  
**_one-context_** |  
**_/etc/one-context/_**

**_more /etc/netplan/50-one-context.yaml_**  
```
routes: 
  0.0.0.0/0 --> qualsevol gateway
```

Es pot posar més d'un gateway?  
Si hi ha dues portes de sortida agafes la que sigui més fàcil. Poden estar en diferents xarxes, el que defineixo és la métrica, com més baixa sigui, més prioritat té. Si tinc una métrica a 100 i una altra a 200, anirà la majoria per la 100, i quan aquesta estigui saturada, s'anirà per l'altra. Al tenir gateway, la màquina no està desconnectada perquè tens una altra línia.

*cd /etc/one-context.d/*  
**_ls_**  
hi ha nº en diverses seqüències i configuracions.

*more loc-10-network*

Es pot utilitzar _vi_ en comtpes de _nano_

Open (ctrl + O), insert(ctrl+I), append (ctrl + A).

en open nebula: configuració -> power --> screen blanck never, automatic suspend off

despues de las fotos hace:
* ip r
* ping 10.10.10.47 pero no llega

_netplan apply_

creo q ha abierto lo del 50-yaml y ha puesto lo mismo q en la otra pero poniendo 20.20.20.6/22

more /proc/sys/netf/IP

POSTROUTING vull paquets fora

_iptables -t nat -A POSTROUTING -j MASQUERADE_


---
### Clase 26/09/2024

Si no funciona ping a google.com ni a 8.8.8.8 ping a gateway.
*ip R* (possa el gateway al default)
Fem un ping al **gateway**
Si no tinc gateway --> *ip r add default 0.0.0.0/0* (qualsevol cosa cap a fora)

altra màquina sysctl -p
more /proc/sys/net/ipv4
iptables -t nat -L (la tabla nat se refresca cada vez q inicias la máquina)
iptables -t nat -A POSTROUTING -j MASQUERADE
apt install iptables-persistent 
cd /etc/iptables
ls
more rules.v4

spuffing: fer veure q ets una màquina quan ets altre
catnix és el punt neutre

paquets de l'altre màquina per l'eth0

```bash
tcpdump -i eth1

tcpdump -i eth0 host (ip altra màquina)
iptables -t nat -F
iptables -t nat -L

ss -upta | grep LISTEN // són els punts q tinc un daemon escoltant allà, això vol dir que tinc una porta oberta
```
en eth0 no arriba pq el kernel l'agafa i no arriba a tcpdump q es capa 2, es queda en capa3


*tcpdump -i eth1* 
paquets de l'altre màquina per l'eth0
*tcpdump -i eth0 host (ip altra màquina)*
*iptables -t nat -F*
*iptables -t nat -L*
en eth0 no arriba pq el kernel l'agafa i no arriba a tcpdump q es capa 2, es queda en capa3
*ss -upta | grep LISTEN* són els punts q tinc un daemon escoltant allà, això vol dir que tinc una porta oberta

Paquet DNS: DNSmasq. Utilitza el /etc/hosts per resoldre els noms.
DHCP per poder assignar els paràmetres de xarxa (IP)
Systemd resolved. Volem un DNS intern per a q totes les màquines es coneguin.


```console
systemctl status systemd-resolved
appt install dnsmasq // peta pq el port 53 està ocupat (el port dns)
systemctl stop systemd-resolved
systemctl disable systemd-resolved
systemctl restart dnsmasq
systemctl status systemd-resolved
//trec el daemon q ocupat pel dns i posso el meu
systemctl status dnsmasq* possa q encara no està ven configurat, ho farem a la pràctica
```

```console
more /etc/resolv.com //nameservers de l'autònoma
vi /etc/resolv.conf //canvio el primer per 127.0.0.1 i dóna error
cd ..
vi dnsmasq.conf
//busquem el tag interface
systemctl restart dnsmasq //pq he canviat la configuració del DNS
host mvb.gax.org //em dona la IP nostra
```

Per poder sortir de vi sense guardar: esc :q!

DCHP
Un broadcast no pot saltar de xarxa, es pot possar un `dchp-relay`  per a q surti a una altra màquina
`/etc7default/isc-dhcp-relay`


---
