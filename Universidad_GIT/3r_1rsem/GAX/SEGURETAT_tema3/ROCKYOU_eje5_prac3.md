
Per a dur a terme l'exercici i provar un atac per força bruta contra el servidor **B** des de **A** utilitzant **Medusa**, seguirem aquests passos detallats. Aquest exercici es centra a simular un atac per força bruta per avaluar la seguretat de les contrasenyes.

---

### **Configuració de l'entorn**
#### 1. **Instal·lar Medusa a A**
Primer, instal·la **Medusa** al sistema A des dels repositoris de Debian:

```bash
sudo apt update
sudo apt install medusa
```

#### 2. **Crear un usuari a B amb una contrasenya habitual/comuna**
Al servidor **B**, crea un usuari amb una contrasenya coneguda o feble. Pots fer-ho amb els següents passos:

1. Crear l'usuari:
   ```bash
   sudo adduser usuari_test
   ```

2. Assignar una contrasenya feble:
   Quan se't demani una contrasenya, utilitza una de les contrasenyes més habituals (per exemple, **123456**.

---

### **Preparació de la llista de contrasenyes**
En `A` Baixa una llista de contrasenyes habituals des d'un repositori públic, com ara **SkullSecurity**. Per exemple:

En firefox buscar `downloads.skullsecurity.org/passwords`
Descarregar `rockyou-10.txt.bz2`

Ho canviem a la carpeta `practica3/Passwords`
Descomprimeix el fitxer si està comprimit:

```bash
bunzip2 rockyou-10.txt.bz2
```

---

### **Execució de l'atac amb Medusa**
L'atac es farà des de **A** cap a **B**. Medusa és una eina de força bruta multi-threaded, molt eficient per comprovar credencials contra serveis com SSH.

#### Exemple d'atac SSH:
Suposem que:
- **IP del servidor B**: `20.20.20.195`
- **Usuari a atacar**: ``
- **Fitxer de contrasenyes**: `rockyou-10.txt`

Executa Medusa amb la següent comanda:

S'ha intentat canviar el nom d'usuari de b però no deixa.

```bash
medusa -h 20.20.20.195 -u prac3_ex5 -P rockyou.txt -M ssh
```

**Explicació dels paràmetres**:
- `-h 192.168.1.2`: Adreça IP del servidor **B**.
- `-u usuari_test`: Nom de l'usuari a provar.
- `-P rockyou.txt`: Fitxer que conté la llista de contrasenyes.
- `-M ssh`: Mòdul a utilitzar, en aquest cas **SSH**.

---

### **Verificació del resultat**
1. **Si la contrasenya es troba**, Medusa mostrarà el següent missatge:
   ```
   ACCOUNT FOUND: [ssh] Host: 192.168.1.2 User: usuari_test Password: 123456
   ```
   Això indica que s'ha pogut trobar la contrasenya correcta.

2. **Si no es troba cap contrasenya vàlida**, és possible que:
   - La contrasenya no estigui a la llista.
   - Hi hagi un sistema de bloqueig d'IPs actiu després de múltiples intents fallits.

---

### **Mesures de mitigació**
Després de realitzar l'atac, és important implementar mesures de seguretat per protegir el servidor **B**. Algunes recomanacions inclouen:

0. Possar com a root el nostre usuari `prac3_ex5`.
Des d'adminp possar:
```shell
sudo usermod -aG sudo prac3_ex5
```

Desbanear ip:
```shell
sudo fail2ban-client set JAIL unbanip IP_ADDRESS
```


---

### **Lliurament**
- **Captures de pantalla** que mostrin:
  - L'execució del programa Medusa i la troballa de la contrasenya (si escau).
  - La configuració de les contrasenyes i usuaris al servidor **B**.
- **Gràfica o resum** dels intents i els temps necessaris per superar l'autenticació.
- **Mesures de seguretat** implementades després de l'exercici per protegir el sistema.

Amb aquest exercici pots avaluar tant la força de les contrasenyes com les vulnerabilitats del servidor **B** enfront d'atacs de força bruta.