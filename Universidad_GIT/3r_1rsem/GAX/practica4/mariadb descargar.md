Perfecte! Els plugins `check_mariadb_slaves` i `check_dockers` són molt útils per monitoritzar **MariaDB** (replicació) i **Docker**, respectivament. Ara pots configurar-los al teu sistema **Nagios**. Segueix aquests passos per fer-ho:

---

### **1. Configura el plugin `check_mariadb_slaves`**

#### **1.1. Descarrega i instal·la el plugin**

Si encara no tens el plugin, descarrega'l des de [Nagios Exchange](https://exchange.nagios.org/). Copia'l al directori de plugins de Nagios:

```bash
sudo cp check_mariadb_slaves /usr/local/nagios/libexec/
sudo chmod +x /usr/local/nagios/libexec/check_mariadb_slaves
```

#### **1.2. Prova el plugin manualment**

Executa el plugin des de la línia de comandes per assegurar-te que funciona. Per exemple:

```bash
/usr/local/nagios/libexec/check_mariadb_slaves -H <IP_MARIADB_HOST> -u <usuari> -p <contrasenya>
```

- Substitueix `<IP_MARIADB_HOST>`, `<usuari>` i `<contrasenya>` per les credencials del servidor MariaDB.
- Si no tens credencials, pots crear un usuari amb permisos de lectura sobre l'estat de replicació:

```sql
GRANT REPLICATION CLIENT ON *.* TO 'nagios'@'%' IDENTIFIED BY 'contrasenya';
```

#### **1.3. Afegeix el servei a Nagios**

Edita l'arxiu de configuració (per exemple, `localhost.cfg` o `hosts.cfg`) i afegeix una entrada per monitoritzar MariaDB:

```bash
define service {
    use                     local-service
    host_name               A
    service_description     MariaDB Replication
    check_command           check_mariadb_slaves!<usuari>!<contrasenya>
}
```

- Assegura't que `A` és el nom del host configurat.
- Substitueix `<usuari>` i `<contrasenya>` pels valors adequats.

---

### **2. Configura el plugin `check_dockers`**

#### **2.1. Descarrega i instal·la el plugin**

Si encara no l'has descarregat, fes-ho des de [Nagios Exchange](https://exchange.nagios.org/). Copia'l al directori de plugins de Nagios:

```bash
sudo cp check_dockers /usr/local/nagios/libexec/
sudo chmod +x /usr/local/nagios/libexec/check_dockers
```

#### **2.2. Configura l'accés a Docker**

- Assegura't que el servei Docker està en execució:
    
    ```bash
    sudo systemctl start docker
    ```
    
- El plugin `check_dockers` normalment accedeix al socket Docker (`/var/run/docker.sock`). Assegura't que l'usuari Nagios té accés:
    
    ```bash
    sudo usermod -aG docker nagios
    ```
    

#### **2.3. Prova el plugin manualment**

Executa el plugin des de la línia de comandes per comprovar que funciona:

```bash
/usr/local/nagios/libexec/check_dockers -c running
```

Això comprova que els contenidors estan en execució. Pots especificar altres opcions segons el que vulguis monitoritzar.

#### **2.4. Afegeix el servei a Nagios**

Edita l'arxiu de configuració i afegeix una entrada per monitoritzar Docker:

```bash
define service {
    use                     local-service
    host_name               A
    service_description     Docker Status
    check_command           check_dockers!-c running
}
```

---

### **3. Verifica la configuració**

1. Comprova que la configuració de Nagios és vàlida:
    
    ```bash
    sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
    ```
    
2. Si no hi ha errors, reinicia Nagios:
    
    ```bash
    sudo systemctl restart nagios
    ```
    

---

### **4. Comprova a la interfície web**

1. Ves a la interfície web de Nagios.
2. A la secció **Services**, comprova que els serveis `MariaDB Replication` i `Docker Status` apareixen correctament i que els seus estats són "OK".

---

### **5. Resol errors**

Si alguna cosa no funciona:

- Revisa els logs de Nagios:
    
    ```bash
    sudo tail -f /usr/local/nagios/var/nagios.log
    ```
    
- Assegura't que els plugins tenen els permisos adequats i que les comandes funcionen des de la línia de comandes.

Si necessites més ajuda, no dubtis a dir-ho! 😊