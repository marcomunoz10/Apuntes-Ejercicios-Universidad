pagina interesante:
https://www.securityartwork.es/2012/05/22/defensas-contra-nmap/



### **Part 1: Realitzar escaneigs amb `nmapsi4` sobre MV-B**

#### **1. Preparar la xarxa**

- Assegura't que les màquines virtuals (MV) estan en funcionament i que es poden comunicar entre elles segons la seva configuració de xarxa.
- Verifica que MV-A pot accedir a MV-B mitjançant `ping 20.20.20.195` des de MV-A.

#### **2. Obrir `nmapsi4` a MV-A**

- Inicia `nmapsi4` (executa `nmapsi4` des de la terminal o busca l'aplicació al menú del sistema).

#### **3. Configurar i executar els escaneigs**

A `nmapsi4`, selecciona les següents opcions per fer escaneigs exhaustius sobre MV-B (`20.20.20.195`):

##### **Quick Scan**

1. Tria el "Profile: Quick scan".
2. Especifica l'objectiu: `20.20.20.195`.
3. Executa l'escaneig.
4. Observa els resultats:
    - Ports oberts.
    - Tipus de servei disponible (si es detecta).
    - Duració de l'escaneig.

##### **Aggressive Scan**

1. Tria el "Profile: Aggressive scan".
2. Executa l'escaneig sobre MV-B.
3. Analitza els resultats:
    - Aquest mode utilitza `-A`, que intenta descobrir més informació (versions de serveis, sistema operatiu).
    - Nota el temps d'execució i la quantitat d'informació recollida.

##### **Intense Scan - All TCP Ports**

1. Tria el "Profile: Intense scan all TCP ports".
2. Configura l'objectiu: `20.20.20.195`.
3. Observa com aquest escaneig és més complet, analitzant tots els ports TCP disponibles.
    - Nota com es comporta MV-B durant aquest escaneig (pot augmentar l'ús de CPU o xarxa).

---

### **Part 2: Analitzar paràmetres i opcions utilitzades per Nmap**

#### **Opcions principals utilitzades pels perfils d'escaneig**

- **Quick scan**: Utilitza poques opcions, només comprova els ports principals (1000 més comuns). Equivalent a: `nmap -F 20.20.20.195`.
- **Aggressive scan**: Utilitza `-A`, que inclou detecció de sistema operatiu, serveis, i scripts de Nmap. Equivalent a: `nmap -A 20.20.20.195`.
- **Intense scan all TCP ports**: Escaneja tots els 65535 ports TCP. Equivalent a: `nmap -p- 20.20.20.195`.

---

### **Part 3: Bloquejar escaneigs amb `iptables`**

#### **1. Identificar els patrons dels escaneigs**

Cada tipus d'escaneig deixa traces en MV-B que pots analitzar:

- Revisa els logs del sistema (`/var/log/syslog` o `/var/log/messages`) a MV-B per identificar intents de connexió als ports.

#### **2. Configurar regles `iptables` per bloquejar escaneigs**

Pots afegir regles específiques a `iptables` a MV-B per bloquejar escaneigs de port.

##### **Regla per bloquejar escaneigs d'intents de connexió ràpids**

Executa les següents comandes a MV-B:

```bash
# Bloquejar escaneigs de connexió ràpida
iptables -A INPUT -p tcp --dport 1:65535 -m connlimit --connlimit-above 5 -j DROP
```


##### **Bloquejar escaneigs amb SYN (stealth scan)**

```bash
iptables -A INPUT -p tcp --tcp-flags SYN,ACK,FIN,RST SYN -j DROP
```

##### **Bloquejar tot el tràfic Nmap detectat**

Utilitza un mòdul específic per Nmap:

```bash
iptables -A INPUT -m string --algo bm --string "Nmap" -j DROP
```

#### **3. Verificar les regles**

Després d'afegir regles, comprova-les:

```bash
iptables -L -v
```

#### **4. Provar de nou els escaneigs**

Executa els mateixos escaneigs des de MV-A i comprova si:

- Els escaneigs són més lents.
- Alguns ports ja no apareixen als resultats.

---

### **Nota Final**

Desa les regles `iptables` per garantir que persisteixin després d'un reinici:

```bash
iptables-save > /etc/iptables/rules.v4
```

Si necessites més ajuda amb algun dels passos, fes-m'ho saber! 😊




# **Part 3 script DROP per defecte**

### **Script adaptat per a Màquina B**

```bash
#!/bin/sh

echo -n Aplicant Regles de Firewall per a Màquina B...

## Netejar regles existents
iptables -F
iptables -X
iptables -Z
iptables -t nat -F

## Polítiques per defecte a DROP
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP

## Permetre tràfic local (loopback)
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT

## Permetre tràfic dins de la xarxa "Middle" (20.20.20.0/24)
iptables -A INPUT -s 20.20.20.0/24 -j ACCEPT
iptables -A OUTPUT -d 20.20.20.0/24 -j ACCEPT

## Permetre tràfic dins de la xarxa "Deep" (30.30.30.0/24)
iptables -A INPUT -s 30.30.30.0/24 -j ACCEPT
iptables -A OUTPUT -d 30.30.30.0/24 -j ACCEPT

## Permetre tràfic dins de la xarxa "DMZ" (30.30.34.0/24)
iptables -A INPUT -s 30.30.34.0/24 -j ACCEPT
iptables -A OUTPUT -d 30.30.34.0/24 -j ACCEPT

## Permetre tràfic de serveis web (HTTP i HTTPS)
iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT       # Entrada HTTP
iptables -A OUTPUT -p tcp -m tcp --sport 80 -j ACCEPT      # Respostes HTTP
iptables -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT      # Entrada HTTPS
iptables -A OUTPUT -p tcp -m tcp --sport 443 -j ACCEPT     # Respostes HTTPS

## Permetre tràfic DNS
iptables -A INPUT -p udp -m udp --sport 53 -j ACCEPT       # Respostes DNS
iptables -A OUTPUT -p udp -m udp --dport 53 -j ACCEPT      # Consultes DNS


## Bloquejar escaneigs de ports i tràfic no autoritzat
iptables -A INPUT -p tcp --dport 1:1024 -j DROP            # Bloquejar ports reservats
iptables -A INPUT -p udp --dport 1:1024 -j DROP
iptables -A INPUT -p tcp -m tcp --dport 1723 -j DROP       # Bloquejar PPTP (si no s'utilitza)
iptables -A INPUT -p tcp -m tcp --dport 3306 -j DROP       # Bloquejar MySQL
iptables -A INPUT -p tcp -m tcp --dport 5432 -j DROP       # Bloquejar PostgreSQL

echo " OK. Verifiqueu les regles aplicades amb: iptables -L -n"
```

---

### **Adaptacions fetes**

1. **Neteja i configuració inicial**: Mantingut tal com al teu script original.
2. **Polítiques `DROP` per defecte**:
    - Totes les connexions no explícitament permeses seran rebutjades.
3. **Permetre tràfic segons la configuració de la xarxa**:
    - Xarxes **Middle** (20.20.20.0/24), **Deep** (30.30.30.0/24) i **DMZ** (30.30.34.0/24).
4. **Permetre serveis essencials**:
    - **HTTP/HTTPS**: Per serveis web.
    - **DNS**: Per resolució de noms.
    - **NTP**: Per sincronització horària.
5. **Protecció contra escaneigs**:
    - Bloqueig de ports comuns (`1:1024`, `3306`, `5432`, etc.).

---

### **Com utilitzar l'script**

1. Desa l'script en un fitxer (per exemple, `firewall-b.sh`):
    
    ```bash
    nano firewall-b.sh
    ```
    
    Pega el contingut i desa'l (`Ctrl+O`, `Enter`, `Ctrl+X`).
    
2. Fes-lo executable:
    
    ```bash
    chmod +x firewall-b.sh
    ```
    
3. Executa'l com a root:
    
    ```bash
    sudo ./firewall-b.sh
    ```
    
4. Verifica les regles aplicades:
    
    ```bash
    iptables -L -n
    ```
    

---

Si necessites ajustar alguna configuració addicional, fes-m'ho saber! 😊