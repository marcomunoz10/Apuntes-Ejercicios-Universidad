### Previo
Paso `5`.
En la máquina A, en  `/usr/local/nagios/etc/objects/localhost.cfg` definir los hosts de A,B,C i añadir servicios para B y C.

```shell
# Servei PING per a A
define service {
    use                     local-service
    host_name               A
    service_description     PING
    check_command           check_ping!100.0,20%!500.0,60%
}

# Servei SSH per a A
define service {
    use                     local-service
    host_name               A
    service_description     SSH
    check_command           check_ssh
}

# Servei HTTP per a A
define service {
    use                     local-service
    host_name               A
    service_description     HTTP
    check_command           check_http
}

# SERVEIS PER B I C
define service {
    use                     local-service
    host_name               B
    service_description     PING
    check_command           check_ping!100.0,20%!500.0,60%
}

define service {
    use                     local-service
    host_name               B
    service_description     SSH
    check_command           check_ssh
}

define service {
    use                     local-service
    host_name               B
    service_description     HTTP
    check_command           check_http
}

define service {
    use                     local-service
    host_name               C
    service_description     PING
    check_command           check_ping!100.0,20%!500.0,60%
}

define service {
    use                     local-service
    host_name               C
    service_description     SSH
    check_command           check_ssh
}

define service {
    use                     local-service
    host_name               C
    service_description     HTTP
    check_command           check_http
}

```


Per verificar:
```shell
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```
![[Pasted image 20241206204558.png]]

Possa error que hi ha serveis amb localhost però no hi ha cap servei que es digui així. Es soluciona esborrant els serveis q possi localhost.

![[Pasted image 20241206210916.png]]

Ara ja funciona.

```shell
sudo systemctl restart nagios
```

Vas a `Firefox`:
``` shell
http://A-Middle/nagios
#user nagiosadmin
#password hola1234**
```

![[Pasted image 20241206211443.png]]


Ara falta instal·lar-se un servei addicional.

En `Firefox` me voy a:
```shell
https://exchange.nagios.org/directory/Plugins/Databases/Others
descargar MariaDB
```

[[mariadb descargar]]

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcutVGPUhIJydZVLXF1Qo5s_0OLY0Eizdb3pTWOJzncBaVeasqODUxVvQgQhfivL1kDAyaecuRl4ii16bYuV8kq_087J_B8KdGMuwZBXn-1ACtWvVYBGJoRiaEzrzCBhGL47efJ?key=2KnHNVaUKW_s1NADHj2ctSp-)**


![[Pasted image 20241207130711.png]]



# Exercici 2

En `B` i en `C`:
![[Pasted image 20241207132302.png]]

![[Pasted image 20241207132354.png]]


Es segueix l'apèndix de la pràctica. Com poder veure em instal·lat el plugin.
![[Pasted image 20241207144625.png]]

Des de B:


Des de C:
![[Pasted image 20241207145517.png]]


# Exercici 3
a)

![[Pasted image 20241209231121.png]]
Para desinstalar por completo:

```
sudo apt remove --purge netdata
sudo apt autoremove
```

Para verificar que se ha desinstalado (poner que como problema utilizar `dkpg`, ya que no funcionaba):
```shell
ps aux | grep netdata
```

Se elimina cualquier fichero de netdata:
```shell
sudo rm -rf /etc/netdata /var/lib/netdata /var/log/netdata /usr/lib/netdata /opt/netdata
```

Wget:
```shell
wget -O /tmp/netdata-kickstart.sh https://get.netdata.cloud/kickstart.sh && sh /tmp/netdata-kickstart.sh --nightly-channel --claim-token FlqTcfE1MthjOCM5QWXyDZq7lOE3aWPobGfybeeGI-eXk4IekUKBznVLI2maHRpnIo0ei4PaXZuIAilxYQpUsxTC-l2k6GvByJpNlgzW4HD5hi5HAMBcvnKvb2RMeVQ5Z9b3fV8 --claim-rooms b772c3c1-79cb-45fc-93ed-fc91a6594a97 --claim-url https://app.netdata.cloud
```

Màquina A
![[Pasted image 20241211160916.png]]

![[Pasted image 20241211160944.png]]

![[Pasted image 20241211161005.png]]![[Pasted image 20241211161019.png]]
# Exercici 5

M'he instal·lat docker:

```shell
sudo apt-get update
sudo apt-get install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

al fer `http://a-middle:8080` em possa **unable to connect**


### Chat 2:

Aquí tens una explicació pas a pas, assumint que no tens coneixements previs de Linux.

---

## **Configuració d'una aplicació Flask i Redis amb Docker i Docker Compose**

### **Què farem?**

Crearem una aplicació web bàsica amb Flask (un framework per Python) que compta les vegades que s'accedeix a la pàgina. Aquesta aplicació emmagatzemarà el comptador en Redis (una base de dades en memòria). Tot això s'executarà en contenidors Docker, que són com caixes aïllades on l'aplicació pot funcionar independentment del sistema operatiu de l'ordinador.

---

### **1. Pre-requisits**

#### **a. Instal·la Docker i Docker Compose**

Docker i Docker Compose són eines que ens permeten crear i gestionar els contenidors.

1. **Actualitza el sistema** per assegurar-te que tens els últims paquets disponibles:
    
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```
    
2. **Instal·la Docker**:
    
    ```bash
    sudo apt install docker.io -y
    ```
    
3. **Activa Docker perquè s'iniciï automàticament**:
    
    ```bash
    sudo systemctl start docker
    sudo systemctl enable docker
    ```
    
4. **Comprova que Docker funciona correctament**:
    
    ```bash
    docker --version
    ```
    
    Apareixerà una línia amb la versió de Docker instal·lada, per exemple: `Docker version 20.10.25`.
    
5. **Instal·la Docker Compose**:
    
    ```bash
    sudo apt install docker-compose -y
    ```
    
6. **Comprova que Docker Compose funciona**:
    
    ```bash
    docker-compose --version
    ```
    
    Apareixerà una línia amb la versió de Docker Compose instal·lada.
    

---

### **2. Crear els fitxers necessaris**

Anem a crear els fitxers que defineixen la nostra aplicació i els seus contenidors.

#### **a. Crea un directori per al projecte**

1. Obre un terminal.
2. Crea un directori anomenat `flask_redis_app`:
    
    ```bash
    mkdir flask_redis_app
    ```
    
3. Entra dins del directori:
    
    ```bash
    cd flask_redis_app
    ```
    

#### **b. Crea el fitxer `Dockerfile`**

1. Escriu aquesta comanda per obrir un editor de text (p. ex., `nano`):
    
    ```bash
    nano Dockerfile
    ```
    
2. Copia aquest contingut al fitxer:
    
    ```dockerfile
    FROM python:2.7-slim
    WORKDIR /app
    ADD . /app
    RUN pip install --trusted-host pypi.python.org -r requirements.txt
    EXPOSE 5000
    CMD ["python", "app.py"]
    ```
    
3. Guarda i tanca el fitxer (`Ctrl + O` per guardar, després `Ctrl + X` per sortir).

---

#### **c. Crea el fitxer `requirements.txt`**

Aquest fitxer defineix les llibreries que necessita la nostra aplicació.

1. Escriu aquesta comanda:
    
    ```bash
    nano requirements.txt
    ```
    
2. Copia aquest contingut:
    
    ```
    flask
    redis
    ```
    
3. Guarda i tanca el fitxer.

---

#### **d. Crea el fitxer `app.py`**

Aquest és el codi de la nostra aplicació Flask.

1. Escriu aquesta comanda:
    
    ```bash
    nano app.py
    ```
    
2. Copia aquest contingut:
    
    ```python
    from flask import Flask
    from redis import Redis
    
    app = Flask(__name__)
    redis = Redis(host='redis', port=6379)
    
    @app.route('/')
    def hello():
        try:
            visits = redis.incr("counter")
        except Exception:
            visits = "<i>cannot connect to Redis, counter disabled</i>"
        html = "<h3>Hello World!</h3> <b>Visits:</b> {visits}"
        return html.format(visits=visits)
    
    if __name__ == "__main__":
        app.run(host="0.0.0.0", debug=True)
    ```
    
3. Guarda i tanca el fitxer.

---

#### **e. Crea el fitxer `docker-compose.yml`**

Aquest fitxer defineix com els contenidors treballen junts.

1. Escriu aquesta comanda:
    
    ```bash
    nano docker-compose.yml
    ```
    
2. Copia aquest contingut:
    
    ```yaml
    version: '2'
    services:
      web:
        build: .
        ports:
          - "5000:5000"
        volumes:
          - .:/app
        depends_on:
          - redis
      redis:
        image: redis
    ```
    
3. Guarda i tanca el fitxer.

---

### **3. Construir i executar els contenidors**

#### **a. Construeix els contenidors**

Executa aquesta comanda per crear la imatge del contenidor:

```bash
docker-compose build
```

#### **b. Executa els contenidors**

Inicia els contenidors amb aquesta comanda:

```bash
docker-compose up
```

Apareixerà molta informació al terminal, indicant que Flask i Redis estan funcionant.

---

### **4. Verifica que l'aplicació funciona**

1. Obre un navegador web.
2. Ves a `http://localhost:5000`.
3. Hauries de veure un missatge com aquest:
    
    ```
    Hello World!
    Visits: 1
    ```
    

Cada cop que recarreguis la pàgina, el nombre de visites incrementarà.

---

### **5. Aturar l'aplicació**

Si vols aturar els contenidors, prem `Ctrl + C` al terminal on s'executa `docker-compose up` i després escriu:

```bash
docker-compose down
```



![[Pasted image 20241210115756.png]]


![[Pasted image 20241210115902.png]]

https://www.kubecost.com/kubernetes-devops-tools/cadvisor/


![[Pasted image 20241211004345.png]]

fent http://b-middle:8080:
![[Pasted image 20241211004512.png]]

![[Pasted image 20241211004648.png]]
Con la maquina de A:
![[Pasted image 20241211005948.png]]


Entesos! Aquí tens el tutorial complet amb totes les modificacions que has indicat. He ajustat els passos perquè no fem servir noms predefinits per les imatges (com `apache_finexo`), i sempre llistarem les imatges amb `sudo docker images` per identificar el **IMAGE ID** corresponent.

---

### **b. Contenidor Apache amb el site "Finexo"**

#### **1. Descarregar el site Finexo**

1. Ves al [site Finexo](https://www.free-css.com/free-css-templates/page296/finexo).
2. Descarrega l'arxiu `.zip` i desa’l al teu ordinador.
3. Descomprimeix-lo i situa els fitxers en una carpeta (per exemple, `finexo_site`).

#### **2. Crear el `Dockerfile`**

1. Crea una carpeta per al projecte Apache Finexo:
    
    ```bash
    mkdir apache_finexo
    cd apache_finexo
    ```
    
2. Copia els fitxers descomprimits del site Finexo dins d’aquesta carpeta.
3. Crea un fitxer `Dockerfile`:
    
    ```bash
    nano Dockerfile
    ```
    
4. Copia aquest contingut al `Dockerfile`:
    
    ```dockerfile
    # Usa una imatge oficial d'Apache com a base
    FROM httpd:2.4
    
    # Estableix el directori de treball a Apache
    WORKDIR /usr/local/apache2/htdocs/
    
    # Copia tots els fitxers del directori actual al directori de treball
    ADD . /usr/local/apache2/htdocs/
    
    # Defineix la comanda per iniciar Apache
    CMD ["httpd-foreground"]
    ```
    
5. Desa i tanca el fitxer (`Ctrl + O` per guardar i `Ctrl + X` per sortir).

#### **3. Construir la imatge Docker**

1. Construeix la imatge:
    
    ```bash
    sudo docker build .
    ```
    

#### **4. Identificar el nom o ID de la imatge**

1. Llista totes les imatges disponibles:
    
    ```bash
    sudo docker images
    ```
    
    Veureu una sortida semblant a aquesta:
    
    ```
    REPOSITORY       TAG       IMAGE ID       CREATED         SIZE
    <none>           <none>    9be7252ef87a   2 minutes ago   150MB
    ```
    
2. Apunta’t l’**IMAGE ID** de la imatge que acabes de crear (per exemple, `9be7252ef87a`).

#### **5. Executar el contenidor**

1. Executa el contenidor utilitzant l’**IMAGE ID**:
    
    ```bash
    sudo docker run -d --name apache_finexo -p 8081:80 <IMAGE_ID>
    ```
    
    Substitueix `<IMAGE_ID>` pel valor corresponent (p. ex., `9be7252ef87a`).

#### **6. Verificar**

1. Obre el navegador i accedeix a `http://<IP_de_la_MV>:8081`.
2. Hauries de veure el site Finexo carregat.

---

### **c. Contenidor amb l'app Flask del `docker-curriculum`**

#### **1. Clonar el repositori**

1. Clona el repositori del `docker-curriculum`:
    
    ```bash
    git clone https://github.com/prakhar1989/docker-curriculum.git
    ```
    
2. Navega al directori de l'app Flask:
    
    ```bash
    cd docker-curriculum/flask-app
    ```
    

#### **2. Construir la imatge**

1. Construeix la imatge:
    
    ```bash
    sudo docker build .
    ```
    

#### **3. Identificar el nom o ID de la imatge**

1. Llista totes les imatges disponibles:
    
    ```bash
    sudo docker images
    ```
    
    Apunta’t l’**IMAGE ID** de la imatge creada.

#### **4. Executar el contenidor**

1. Executa el contenidor utilitzant l’**IMAGE ID**:
    
    ```bash
    sudo docker run -d -p 5001:5000 <IMAGE_ID>
    ```
    
    Substitueix `<IMAGE_ID>` pel valor correcte.

#### **5. Verificar**

1. Obre el navegador i accedeix a `http://<IP_de_la_MV>:5001`.
2. Hauries de veure l'aplicació Flask funcionant.

---

### **Monitoritzar els contenidors amb cAdvisor**

1. Assegura’t que cAdvisor està funcionant. Si encara no l’has configurat:
    
    ```bash
    sudo docker run \
      --volume=/:/rootfs:ro \
      --volume=/var/run:/var/run:ro \
      --volume=/sys:/sys:ro \
      --volume=/var/lib/docker/:/var/lib/docker:ro \
      --publish=8080:8080 \
      --detach=true \
      --name=cadvisor \
      google/cadvisor:latest
    ```
    
2. Accedeix a `http://<IP_de_la_MV>:8080` al navegador.
3. Verifica que pots veure els tres contenidors:
    - Flask + Redis (port 5000).
    - Apache Finexo (port 8081).
    - Flask del repositori (port 5001).

---

### **Lliurament**

1. Realitza captures de pantalla que mostrin:
    - **Estat dels contenidors amb `docker ps`:** prova que els tres contenidors estan en funcionament.
    - **Accés a les aplicacions:**
        - Site Finexo (`http://<IP_de_la_MV>:8081`).
        - Flask + Redis (`http://<IP_de_la_MV>:5000`).
        - App Flask del `docker-curriculum` (`http://<IP_de_la_MV>:5001`).
    - **Monitorització amb cAdvisor:** mostra les estadístiques dels contenidors.


---
Per eliminar una imatge Docker, segueix aquests passos:

---

### **1. Llista les imatges existents**

Executa aquesta comanda per veure totes les imatges al sistema:

```bash
sudo docker images
```

Apareixerà una sortida semblant a aquesta:

```
REPOSITORY          TAG          IMAGE ID       CREATED          SIZE
<none>              <none>       d5f52de61a11   2 minutes ago    150MB
httpd               latest       494b2b45fd74   3 days ago       138MB
```

- **IMAGE ID** és l'identificador únic de la imatge que necessites per eliminar-la.

---

### **2. Elimina una imatge específica**

Executa aquesta comanda per eliminar la imatge, utilitzant el seu **IMAGE ID**:

```bash
sudo docker rmi <IMAGE_ID>
```

Per exemple:

```bash
sudo docker rmi d5f52de61a11
```

---

### **3. Si la imatge està en ús**

Si la imatge està associada a un contenidor actiu o aturat, hauràs de fer el següent:

#### **a. Identifica el contenidor que usa la imatge**

Llista tots els contenidors, inclosos els aturats:

```bash
sudo docker ps -a
```

#### **b. Atura el contenidor si està actiu**

Si el contenidor està en funcionament, atura’l:

```bash
sudo docker stop <CONTAINER_ID>
```

#### **c. Elimina el contenidor**

Un cop aturat, elimina el contenidor:

```bash
sudo docker rm <CONTAINER_ID>
```

#### **d. Torna a eliminar la imatge**

Ara pots eliminar la imatge:

```bash
sudo docker rmi <IMAGE_ID>
```

---

### **Nota: Forçar l'eliminació**

Si necessites forçar l'eliminació d'una imatge (per exemple, si encara està en ús):

```bash
sudo docker rmi -f <IMAGE_ID>
```

### **Activar un contenidor**

```shell
sudo docker start nom_docker
```

---

Amb això podràs gestionar i eliminar les imatges que necessitis. Si tens algun problema, comparteix el missatge d'error i t'ajudaré! 😊

![[Pasted image 20241211012225.png]]

![[Pasted image 20241211012335.png]]

![[Pasted image 20241211012729.png]]

CAdvisor de finexo:
![[Pasted image 20241211014007.png]]

![[Pasted image 20241211014030.png]]

el error de docker curriculum es la version de flask, hay que quitarla de `requirements.txt`
![[Pasted image 20241211144903.png]]
![[Pasted image 20241211151004.png]]

![[Pasted image 20241211151329.png]]

docker-curriculum
![[Pasted image 20241211151420.png]]


![[Pasted image 20241211151532.png]]




apache-finexo
![[Pasted image 20241211151652.png]]

![[Pasted image 20241211151746.png]]


flask-redis

![[Pasted image 20241211151823.png]]

![[Pasted image 20241211151856.png]]