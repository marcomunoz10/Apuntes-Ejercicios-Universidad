eje 6
Para realizar este ejercicio de configuración de un *reverse proxy* en Apache en la máquina B, redirigiendo las solicitudes a la máquina C, sigue estos pasos detallados:

### 1. Habilitar los módulos de Proxy en la máquina B

En primer lugar, debes habilitar los módulos de Apache necesarios para configurar el *reverse proxy*. En la máquina B, ejecuta los siguientes comandos:

```bash
sudo a2enmod proxy
sudo a2enmod proxy_http
```

Estos comandos habilitarán los módulos `proxy` y `proxy_http` necesarios para que Apache pueda actuar como un proxy HTTP.

### 2. Deshabilitar las demás páginas web en B

En la máquina B, deshabilita cualquier otra configuración de sitios web que pueda estar activa para evitar que Apache sirva páginas no deseadas. Por ejemplo:

```bash
sudo a2dissite 000-default.conf
```

Esto deshabilitará la configuración del sitio por defecto de Apache. Si tienes otras configuraciones de sitios en `/etc/apache2/sites-enabled/`, deshabilítalas también utilizando `a2dissite` con el nombre adecuado del archivo de configuración.

### 3. Crear una nueva configuración para el reverse proxy

Ahora, debes crear una configuración de Apache en la máquina B para que actúe como un *reverse proxy*, redirigiendo las solicitudes a la máquina C.

1. Crea un archivo de configuración en `/etc/apache2/sites-available/reverse-proxy.conf` con el siguiente contenido:

```apache
<VirtualHost *:80>
    ServerName B-Middle.gax.org

    # Habilitar la configuración del proxy
    ProxyRequests Off
    ProxyPreserveHost On

    # Redirigir las solicitudes al servidor C
    ProxyPass / http://C-Deep.gax.org/
    ProxyPassReverse / http://C-Deep.gax.org/

    # Configuración adicional si quieres apuntar a un archivo específico como index.php
    DirectoryIndex index.html index.php
</VirtualHost>
```

Este archivo de configuración realiza lo siguiente:
- `ProxyRequests Off`: Desactiva las solicitudes directas del cliente hacia el servidor de destino.
- `ProxyPreserveHost On`: Mantiene el nombre de host original al hacer la redirección.
- `ProxyPass`: Configura el proxy hacia la URL de la máquina C (`http://C-Deep.gax.org/`).
- `ProxyPassReverse`: Hace que las respuestas del servidor C sean correctamente interpretadas por Apache.
- `DirectoryIndex index.html index.php`: Define que si hay varios archivos de índice, `index.html` se seleccionará por defecto.

### 4. Habilitar el nuevo sitio

Una vez que hayas creado el archivo de configuración, debes habilitarlo con el siguiente comando:

```bash
sudo a2ensite reverse-proxy.conf
```

Luego, reinicia Apache para aplicar la nueva configuración:

```bash
sudo systemctl restart apache2
```

### 5. Verificar la configuración

Para verificar que la configuración del *reverse proxy* está funcionando correctamente, intenta acceder a la dirección IP de B desde la máquina A:

```bash
curl http://B-Middle.gax.org
```

La respuesta debe ser la página servida por la máquina C (es decir, la página PHP o el archivo `index.php` de C).

### 6. Realizar la prueba de captura de pantalla

Accede desde la máquina A (usando el navegador o `curl`) a la dirección de B (`http://B-Middle.gax.org`) y verifica que la respuesta sea servida desde C. La página que se muestre debe ser la página PHP servida por C.

Si todo está configurado correctamente, en la captura de pantalla podrás mostrar que la página PHP de C se ha servido a través de B, lo que confirma que el reverse proxy está funcionando.

### 7. Revertir la configuración

Después de finalizar el ejercicio, no olvides revertir la configuración para que Apache vuelva a su estado original.

1. Deshabilita el sitio de reverse proxy:

```bash
sudo a2dissite reverse-proxy.conf
```

2. Vuelve a habilitar el sitio por defecto:

```bash
sudo a2ensite 000-default.conf
```

3. Reinicia Apache para aplicar los cambios:

```bash
sudo systemctl restart apache2
```

Con estos pasos, habrás configurado y probado un reverse proxy en la máquina B, redirigiendo las solicitudes a la máquina C, y luego habrás revertido la configuración como se pidió en el ejercicio.

`index.html` está en /var/www

### Errores
al hacer sudo systemctl reload apache2.
Solución mirar `/var/log/apache2/error.log`
`sudo apache2ctl configtest



# eje 7

Sí, necesitas tener el archivo **`web_texto.html`** disponible en tu máquina virtual para poder realizar las pruebas de rendimiento con ApacheBench. Este archivo debe estar alojado en el servidor web (en tu caso, en la máquina que ejecuta Apache) y ser accesible a través de una URL.

### Pasos para asegurarte de que el archivo esté disponible:

1. **Subir el archivo a tu servidor**: Si aún no lo has hecho, descarga el archivo **`web_texto.html`** desde el Campus Virtual o de donde te haya indicado tu instructor y cópialo en el directorio adecuado de tu servidor web. El directorio común para archivos web en Apache es `/var/www/html/`.

   Si tienes el archivo en tu máquina local, puedes usar `scp` (si tienes acceso SSH a la máquina virtual) para copiarlo:

   ```bash
   scp web_texto.html user@ip_maquina:/var/www/html/
   ```

2. **Verificar la configuración de Apache**: Asegúrate de que Apache está configurado para servir archivos desde el directorio `/var/www/html/`. Puedes verificarlo comprobando el archivo de configuración de Apache, que generalmente se encuentra en `/etc/apache2/sites-available/000-default.conf`.

   Debe haber una línea similar a esta:

   ```bash
   DocumentRoot /var/www/html
   ```

3. **Acceder al archivo desde un navegador**: Una vez que hayas subido el archivo, deberías ser capaz de acceder a él a través de la URL correspondiente, por ejemplo:
http://B-Middle.gax.org/web_texto.html

# Eje 8
Aquí tens una guia pas a pas per desplegar i configurar **OwnCloud** fent servir **Docker** i **docker-compose**, seguint les especificacions que demanes.

### Pas 1: Preparar el fitxer `docker-compose.yml`
Per començar, crea un directori per al projecte, i dins d'aquest directori, crea un fitxer anomenat `docker-compose.yml` amb el següent contingut bàsic:

```yaml
version: '3.1'

services:
  owncloud:
    image: owncloud/server:latest
    container_name: owncloud
    restart: always
    ports:
      - 8080:8080
    environment:
      - OWNCLOUD_DOMAIN=localhost  # reemplaça "localhost" amb la IP de la màquina E a l'arxiu .env
      - OWNCLOUD_TRUSTED_DOMAINS=localhost  # reemplaça "localhost" amb la IP de la màquina E
      - OWNCLOUD_ADMIN_USERNAME=admin
      - OWNCLOUD_ADMIN_PASSWORD=admin
      - OWNCLOUD_MYSQL_HOST=db
      - OWNCLOUD_MYSQL_USERNAME=owncloud
      - OWNCLOUD_MYSQL_PASSWORD=owncloud
      - OWNCLOUD_MYSQL_DATABASE=owncloud
    volumes:
      - owncloud_data:/mnt/data
    depends_on:
      - db

  db:
    image: mariadb:latest
    container_name: owncloud_db
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=root_password
      - MYSQL_DATABASE=owncloud
      - MYSQL_USER=owncloud
      - MYSQL_PASSWORD=owncloud
    volumes:
      - db_data:/var/lib/mysql

volumes:
  owncloud_data:
  db_data:
```

### Pas 2: Crear el fitxer `.env`
Crea un fitxer `.env` al mateix directori, on defineixis les variables d'entorn que s'han d'utilitzar al fitxer `docker-compose.yml`. 

> **NOTA**: Canvia "localhost" per la IP de la màquina `E` en les línies `OWNCLOUD_DOMAIN` i `OWNCLOUD_TRUSTED_DOMAIN`.

Exemple de `.env`:

```env
OWNCLOUD_DOMAIN=192.168.1.100
OWNCLOUD_TRUSTED_DOMAIN=192.168.1.100
```

### Pas 3: Desplegar OwnCloud
Des de la mateixa carpeta on has creat els fitxers `docker-compose.yml` i `.env`, executa el següent comandament per desplegar el servei:

```bash
docker-compose up -d
```

Això posarà en marxa els contenidors d'OwnCloud i MariaDB en segon pla.

### Pas 4: Accedir a OwnCloud des de la màquina `E`
1. Obre el navegador de la màquina `E`.
2. Accedeix a `http://192.168.1.100:8080` (reemplaça `192.168.1.100` amb la IP que has definit a l'arxiu `.env`).
3. Hauries de veure la pantalla d'inici de sessió d'OwnCloud.

### Pas 5: Verificar l'accés des d'altres màquines
Intenta accedir a `http://192.168.1.100:8080` des d'una altra màquina de la xarxa local. Si tot està configurat correctament, també hauries de poder veure la pantalla de OwnCloud.

### Pas 6: Pujar arxius per verificar el funcionament
1. Inicia sessió a OwnCloud amb les credencials (`admin`/`admin`).
2. Utilitza l'opció "Arrossega i deixa anar o puja fitxers" per pujar alguns arxius de diferent tipus (PDF, imatges, documents de text, etc.) per verificar que el servei funciona correctament.

### Pas 7: Instal·lar connectors (plugins) a OwnCloud
Per afegir funcionalitats extres, pots instal·lar plugins com ara:
- **Documents**: per a la col·laboració en documents.
- **Media Viewer**: per visualitzar arxius multimèdia.

#### Instal·lació de plugins:
1. A OwnCloud, ves a **Mòduls** (Apps).
2. Busca els connectors que vols instal·lar, com ara "Documents" i "Media Viewer".
3. Instal·la els connectors i verifica que es poden utilitzar.

### Pas 8: Captures de pantalla
Per lliurar la tasca, assegura’t de fer captures de pantalla:
- Des de la màquina `E` i una altra màquina de la xarxa accedint a OwnCloud.
- Mostra els connectors instal·lats i funcionals.
- Mostra alguns arxius pujats a OwnCloud.

Això et permetrà verificar que la configuració és correcta i que OwnCloud funciona tant en accés local com en accés remot dins de la mateixa xarxa.


Para reiniciar `.env`:

```shell
sudo docker-compose down
sudo docker-compose up -d
```

# Eje 9

En B:
```shell
sudo iptables -t nat -A PREROUTING -p tcp --dport 8080 -d B-DEEP -j DNAT --to-destination 30.30.30.84:8080

sudo iptables -t nat -A PREROUTING -p tcp --dport 8080 -d B-MIDDLE -j DNAT --to-destination 30.30.30.84:8080
```

Para hacer las reglas persistentes:
```shell
sudo apt install iptables-persistent
```

Desde A se hace `20.20.20.195:8080` y aparece owncloud de E