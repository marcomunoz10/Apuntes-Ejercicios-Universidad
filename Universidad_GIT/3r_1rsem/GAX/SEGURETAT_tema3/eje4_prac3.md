

### **4. Verificar l'impacte de l'atac**
Mentrestant, has de monitorar el servidor **B** des d'**A** i des de **B** mateix per comprovar l'efecte de l'atac.

#### **4.1. Monitorització amb `tcpdump`**
Executa aquesta comanda a **B** per veure el trànsit entrant:
```bash
sudo tcpdump -i <interface> tcp port 80
```
- Substitueix `<interface>` per l'interfície de xarxa en ús (com `eth0` o `ens33`).

#### **4.2. Comprova connexions amb `ss`**
Executa aquesta comanda a **B** per veure connexions actives al port HTTP:
```bash
ss -ant | grep :80
```

#### **4.3. Verifica la caiguda**
- Intenta accedir al servei web de **B** des d'un navegador o amb `curl`:
  ```bash
  curl http://<IP_B>
  ```
- Si el servei no respon o és molt lent, l'atac ha tingut èxit.

---

### **5. Mitigar l'atac a B**
Per mitigar l'atac, pots aplicar una de les solucions següents al servidor **B**:

#### **5.1. Configuració del tallafocs (`iptables`)**
Limita el nombre de connexions simultànies per IP:
```bash
sudo iptables -A INPUT -p tcp --syn --dport 80 -m connlimit --connlimit-above 20 -j REJECT
```




### **5. Verificación de la caída**

En la Máquina B, verifica si el servicio HTTP está respondiendo:
1. Usa `curl` o `wget`:
   ```bash
   curl http://localhost
   ```
2. Observa los recursos del sistema:
   ```bash
   top
   ss -tuln
   ```
3. Captura tráfico de red con `tcpdump`:
   ```bash
   sudo tcpdump -i eth0 port 80
   ```

Si el servicio está no responde o los recursos están saturados, significa que el ataque fue exitoso.

---


# Fail2ban

Sí, **Fail2Ban** és una eina excel·lent per mitigar atacs com els realitzats amb **slowhttptest**, especialment si vols automatitzar la resposta a patrons d'atac detectats. Aquí tens els passos per configurar **Fail2Ban** per mitigar un atac com el **slow HTTP attack**:

---

### **1. Instal·lar Fail2Ban**
1. Assegura't que tens **Fail2Ban** instal·lat al servidor (B):
   ```bash
   sudo apt update
   sudo apt install fail2ban -y
   ```

2. Comprova que el servei estigui funcionant:
   ```bash
   sudo systemctl status fail2ban
   ```

---

### **2. Crear una configuració específica per HTTP**
1. **Edita el fitxer de configuració local de Fail2Ban**:
   ```bash
   sudo nano /etc/fail2ban/jail.local
   ```

2. Afegeix una configuració per protegir el servidor web de l'atac. Aquí tens un exemple bàsic per protegir Apache o Nginx:

   ```ini
   [http-get-dos]
   enabled  = true
   port     = http,https
   filter   = http-get-dos
   logpath  = /var/log/apache2/access.log  # Modifica segons el teu servidor web
   maxretry = 100                         # Nombre de peticions abans de bloquejar
   findtime = 10                          # Temps (segons) per comptar peticions
   bantime  = 3600                        # Temps de bloqueig (segons)
   action   = iptables[name=HTTP, port=http, protocol=tcp]
   ```

---

### **3. Crear el filtre de detecció d'atacs (http-get-dos)**
1. Crea el fitxer de filtre:
   ```bash
   sudo nano /etc/fail2ban/filter.d/http-get-dos.conf
   ```

2. Afegeix les regles següents per detectar un volum anormal de peticions GET:
   ```ini
   [Definition]
   failregex = ^<HOST> -.*"(GET|POST).*HTTP.*$
   ignoreregex =
   ```

   Això detecta múltiples peticions **GET** o **POST** des d'una mateixa adreça IP en poc temps.

---

### **4. Reiniciar Fail2Ban**
1. Guarda els canvis i reinicia **Fail2Ban** perquè apliqui la nova configuració:
   ```bash
   sudo systemctl restart fail2ban
   ```

2. Comprova que la configuració s'ha carregat correctament:
   ```bash
   sudo fail2ban-client status
   ```

---

### **5. Prova el sistema**
1. Executa l'atac des de **A** amb **slowhttptest**:
   ```bash
   slowhttptest -c 10000 -H -u http://<IP_B>/ -l 1000
   ```

2. Monitoritza l'activitat de Fail2Ban per veure si detecta i bloqueja l'IP atacant:
   ```bash
   sudo fail2ban-client status http-get-dos
   ```

3. Si tot funciona correctament, Fail2Ban bloquejarà l'IP de **A** automàticament amb regles **iptables**.

---

### **6. Ajusta els paràmetres segons el teu entorn**
- **`maxretry`**: Configura segons el volum normal d'ús del servidor. Un valor massa baix podria bloquejar usuaris legítims.
- **`findtime`**: Ajusta el període en què es compten les peticions.
- **`bantime`**: Defineix quant de temps l'IP romandrà bloquejada.

---

Amb aquesta configuració, el servidor **B** quedarà protegit contra atacs tipus **slowloris** i similars. Si necessites ajuda addicional o vols altres mètodes de mitigació, avisa! 😊
