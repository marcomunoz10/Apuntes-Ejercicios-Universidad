1. [[Desinstalar y eliminar un paquet]]
2. Instal·lar el servidor DNS a A: apt-get install dnsmasq 
3. Configurar el servidor DNS a A, editant l’arxiu /etc/dnsmasq.conf i afegint les línies següents: 

```shell
interface=<A_midle_interfacace>
listen-address= <A_midle_IP>
```

midle_interface --> eth1
midle_IP --> ip d'eth1
4. Reiniciar el servei: systemctl restart dnsmasq.service 
5. Eliminar la informació de /etc/hosts (si hi hagués) de las demás MV per poder verificar el funcionament del servei DNS què serà configurat. 
6. Afegir la IP d'A al principi del llistat de servidors DNS de les altres MV, dins de l’arxiu /etc/resolv.conf