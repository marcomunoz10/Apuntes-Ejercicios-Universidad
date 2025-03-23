```shell
-P INPUT ACCEPT
-P OUTPUT "
-P FORWARD "

-P PREROUTING "
-P POSTROUTING "
```


1R TOT EL Q ARRIBI AL FIREWALL DES DE PORT 80 

```shell
-A POSTROUTING -j MASQUERADE
-i ens8(firewall derecha) -o ens3(firewall izquierda)
-A PREROUTING -i ens3 -p tcp --dport 80 -j DNAT --to-destination<ip_dmz:80>
-A INPUT -i ens5 -m state --state new IP_DMZ:80
-A INPUT -i ens5 (la del firewall con dmz) -j DROP

```

connexió de LAN a DMZ:
En vez de poner "new" poner established o related.
De `DMZ` a `LAN`
```shell
-A Forward -i ens5 -d  20.20.0.0/16 -m state --state new -j DROP
```

---
# Proxy Socks
[] my host -1-> ()()INTERNET()() -2-> secure host

* Pueden poner man in the middle en internet.
Idea:

[] my host ----PIPE_k----> ()()INTERNET()() -----PYPE_j-----> myhost
* Poner un tubo para saltarnos cualquier elemento que haya en internet
Del `pipe_k` irá al `proxy_socks` y de ahí al `pype_j`.


`deb = 1(servidor) 10.10.10.47, 20.20.20.6` 
`deb2 = 2(cliente) 20.20.20.126`

1
```shell
more ssd_config #PASSWORD AUTH = YES

```

2
```shell
ssh -ND 9999 20.20.20.6 #N no quiero interconexión interactiva
# D crea el túnel con el servidor del otro lado
#VOY A FIREFOX, SETTINGS, AL FINAL HAY SETTINGS... 
#MANUAL PROXY CONF --> SOCKS_V5
#SOCKS_HOST 127.0.0.1:9999
#PROXY DNS USING SOCKET 5

#upc.edu
#como la comunicación va por la otra maq va lento
#para ver: en deb2 romper el tunel 
```

Puedo tener conexión ssl hacia el puerto 80 y voy dnd quiera

---
# Apache as proxy

Proxy http: 8080
Proxy https: 4430

Si el cliente se quiere librar con no_proxy hago un filtro:
Como se lo envía por capa 7, igualmente pasará por capa 3 y ahí puedes configurar.
Cuando le llega al proxy hace una regla y se lo vuelve a enviar a él mismo(el proxy)
```shell
-A PREROUTING -i ens 8 -p tcp --dport 80 -j DNAT --to-destination localhost:8080
```