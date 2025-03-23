PDF: 1A_D

## 10 de ocubre
Aquest conjunt d'instruccions és per configurar i executar una aplicació **Flask** dins d'un contenidor Docker, probablement per a una aplicació web que funcioni en un port específic (5000) a la teva màquina local.

```shell
cd app-flask-trans-p3
ls
docker build -t myapp10 . #todos los archivos q están en el directorio actual
docker -d -p 5000:5000 myapp10.la
docker ps 
```

em diu q ha executat python app.py al comença,emt

```shell
docker inspect 62
```
Part important NetworkSettings ens diu la IP de la màq, ha creat un gateway i a cada xarxa li dona una IP i funciona.

```shell
docker network ls
```
xarxa en mode bridge la qual ell ha creat i m'ha enganxat la IP a aquesta màquina 
```shell
ip a
```
(entiendo q está hablando de un docker y le está dando ip)

Quan fem localhost:5000 ens surt una web amb missatge, concretament que no es pot conectar a Redis. S'ha de possar en marxa i dir-li quin port ha d'utilitzar --> utilitzar el docker compose

*file: docker-compose.yml*

```shell
docker compose up
```

Surt missatge d'error dient q el port 5000 ja està utilitzat.
Recomanable possar Redis per la base de dades.

```shell
docker stop 62
docker rm 62
docker compose up
```

Para els dos contenidors. No es pot conectar pq ha aturat els servidors.
Si borro contenedor perdo consistència.

```shell
#altre terminal
docker ps
docker exec -it a1 /bin/bash
```

Per a q la sessió sigui interactiva i per capturar el terminal

Ara ja estem dins del contenidor.
```shell
apt update
touch pirulo.txt
date > pirulo.date
ls
more porulo.txt
more pirulo.date #té la data actual
```

Ara miro a l'altra termial i em surt tots els arxius q he creat dins del contenidor pq li vaig dir q volia un volum compartit entre el directori contenidor i la màquina local. Tots els canvis de la màquina apareixen a l'app i tots els canvis de l'app apareix dins la màquina. 

#### Em puc crear una nova imatge a partir del contenidor
```shell
docker commit #not found
docker commit --help #veu les configuracions de docker commit
docker ps
socker commit a1 myappv2
docker image
docker run -d -p 5001:5000 myappv2
```

Ara vaig al navegador i posso localhost:5001. Funciona el contenidor. Ara veurem si dins del directori app tin el pirulo.txt

```shell
docker exec -it 6b /bin/bash
las
```

No tinc pirulo.txt. Haurien d'aparèixer. Ens hem equivocat en algo. Dos opcions: pq el volumen no s'ha fet persistent o ¿?¿?¿?

Sobre 6b
```shell
comanda q ens diu la IP ¿?
```

Contenidor a1
El comandament `docker commit` es fa servir per **crear una nova imatge Docker a partir d'un contenidor que està en funcionament**.

Contenidor x
```shell
docker commit 6b myappV3
docker run -d -p 5002:5000 myappv3
docker exec -it .5e /bin/bash
ip a
```

Errada: no haver declarat com persistent.

Borrem els contenidors
```shell
docker ps -a
docker stop 5e 6b a1 f1 
docker rm 5e 6b a1 f1
```

fa 
```shell
docker images
```

per veure tots els contenidors que té.
per borrar agafa la `IMATGE_ID` i possa els dos primers digits de cada docker
```shell
docker rmi 89 40 65 01
```

Si hi ha dos que començen igual possar tres primers digits

```shell
docker network ls
docker network
docker network prune #borra xarxes no utilitzadas
#no borrar xarxes per defecte --> bridge, host, null
```


Per k8 es necessiten 3 màquines en producció. API, gestiona contenidors(scheduler) i executa contenidors i després fas un desplegament. S'ha treballat en el miniKube per treballar només des d'una màquina. També es pot utilitzar swarm.

Hi ha directoris com host, localhost, var q no es poden canviar les configuracions per defecte. (mirar)


#### SWARM
Les dues màquines han de tenir dockers instalats. Docker.io si és debian ¿?
Va a una màquina interna amb dos IP(10.10.10.47) --> deb 1 li diem "1"
Màquina --> deb 2(20.20.20.126) li diem "2"

1
```shell
sudo docker swarm init #diu q hi ha dues targetes de red --advertise-addr
sudo docker swarm init --advertise-addr 20.20.20.6
ssh 20.20.20.126 
```

jagafo linia de docker swarm joint nsk y pongo:
```shell
sudo "copiaas lo de docker swarm"
sudo docker node ls
```


(fa algo a deb2 q no he vist)

1
tinc dos nodes (debian 1 y debian 2)

```shell
docker service ps
sudo docker service ls #per veure serveis en marxa
sudo -i
cd /home/adminp/
ls
cd apache2
¿? no sé qe hace aquí
```
El dns marca sempre ip d'entrada de swarm i no els nodes per això important possar noms.
té un docker file

```shell
docker build -t apache2 .
docker image
```

Tinc la imatge de apache2

2
```shell
ls
nano Dockerfile
```

possa q en comptes de DEB possi DEB2

Es podria utilitzar mateixa imatge pero no es veuria diferencia. Les imatges han de tenir mateix nom.
```shell
sudo docker build  -t apache2 .
```
No tinc accés a internet.

```shell
ip r add default via 20.20.20.6
ping google.com
```
Ara sí.

1
```shell
docker node ls #tinc els dos nodes
docker srvice create --name=apa2 --replicas=1 -p 8080:80 apache2
docker service ps apa2
```
Em diu q el servei s'està executant, q és el apa2 i q s'executa en localhost.

```shell
docker service ls
```
Em dona la informació del servei q he possat en marxa.

Ara s'en va a firefox, possa localhost:8080 i possa executant: node DEB

```shell
docker service scale apa2=4
```
Detecta q té una i crea 3 répliques més.

```shell
docker service ls
docker service ps apa2
```
Em diu on està funcionant cada réplica.

```shell
docker ps
apt install apache2-utils
ab -n 1000 -c 100 http://127.0.0.1:8080/
docker service scale apa2=8
```

