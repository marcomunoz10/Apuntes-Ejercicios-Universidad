A continuació, et guiaré pas a pas per realitzar els exercicis de la pràctica en un sistema Debian 12. T'ho explicaré com si no en sabessis res. **Assegura't que tens accés d'administrador i que pots instal·lar programari.**

---

### **Preparació inicial**

Abans de començar, cal establir un entorn virtual amb màquines virtuals (MV). Això implica tenir almenys les següents màquines:

1. **MV-A**: Router i servidor VPN.
2. **MV-B**: Client per fer escaneigs de ports i connectar-se a la VPN.
3. **MV-D**: Client extra per comprovar connectivitat.

Si estàs utilitzant una eina com VirtualBox o VMware, assegura't de configurar les xarxes correctament:

- Una interfície en mode NAT o pont (per accés a Internet).
- Una altra interfície en mode "Only-Host" o xarxa interna (per comunicació entre MVs).

---

## **Exercici 1: Instal·lar nmapsi4 i realitzar escaneigs**

### 1. Instal·lació de **nmapsi4**:

1. Obre un terminal a **MV-A**.
2. Actualitza el sistema:
    
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```
    
3. Instal·la les dependències necessàries:
    
    ```bash
    sudo apt install nmap no(python3-pyqt5 -y)
    ```
    
4. Descarrega i instal·la **nmapsi4**:
    
    ```bash
    sudo apt install nmapsi4 -y
    ```
    
    Comprova que es troba instal·lat amb:
    
    ```bash
    nmapsi4 --version
    ```
    

---

### 2. Escaneig amb Nmap:

1. Obre **nmapsi4** des del menú d'aplicacions o executa:
    
    ```bash
    nmapsi4
    ```
    
2. Selecciona l'adreça IP o el nom de xarxa de **MV-B** (assegura't que les màquines estan en la mateixa xarxa interna).
3. Realitza els següents escaneigs:
    - **Quick scan**: Escaneig ràpid dels ports més comuns.
    - **Aggressive scan**: Escaneig més detallat amb detecció de serveis i versions.
    - **Intense scan all TCP ports**: Escaneig exhaustiu de tots els ports TCP.

### 3. Analitzar paràmetres:

Cada perfil d'escaneig de Nmap utilitza un conjunt de paràmetres. Aquí tens una explicació dels més comuns:

- **-sS**: Escaneig TCP "stealth" (escaneig per defecte).
- **-A**: Activa detecció d'OS, serveis i scripts.
- **-T4**: Nivell d'agressivitat (de 0 a 5).
- **-p-**: Escaneja tots els ports (per defecte només els primers 1000).
- **-Pn**: Desactiva la comprovació de ping (es pot utilitzar per màquines que no responen a ICMP).

---

### 4. Bloquejar escaneigs amb iptables:

A **MV-B**, pots bloquejar escaneigs específics modificant regles del tallafocs (iptables):

1. Bloquejar escaneigs de TCP (SYN scan):
    
    ```bash
    sudo iptables -A INPUT -p tcp --tcp-flags SYN,ACK,FIN,RST RST -j DROP
    ```
    
2. Bloquejar escaneigs ICMP (ping):
    
    ```bash
    sudo iptables -A INPUT -p icmp -j DROP
    ```
    
3. Guarda les regles:
    
    ```bash
    sudo iptables-save > /etc/iptables/rules.v4
    ```
    
4. Comprova si el bloqueig funciona repetint els escaneigs des de **MV-A**.

---

## **Exercici 2: Configuració de VPN amb WireGuard**

### 1. Instal·lació de WireGuard:

A **MV-A** i **MV-B**, instal·la WireGuard:

```bash
sudo apt install wireguard -y
```

### 2. Configurar **MV-A** com a servidor VPN:

1. Genera les claus:
    
    ```bash
    wg genkey | tee privatekey | wg pubkey > publickey
    ```
    
2. Edita la configuració del servidor:
    
    ```bash
    sudo nano /etc/wireguard/wg0.conf
    ```
    
    Exemple de configuració:
    
    [Interface]
    Address = 10.0.0.1/24
    PrivateKey = <clau_privada_MV-A>
    ListenPort = 51820
    
    [Peer]
    PublicKey = <clau_pública_MV-B>
    AllowedIPs = 10.0.0.2/32
    ```
    
3. Activa i inicia el servei:
    
    ```bash
    sudo systemctl enable wg-quick@wg0
    sudo systemctl start wg-quick@wg0
    ```
    

---

### 3. Configurar **MV-B** com a client VPN:

1. Genera claus a **MV-B**:
    
    ```bash
    wg genkey | tee privatekey | wg pubkey > publickey
    ```
    
2. Configura el client:
    
    ```bash
    sudo nano /etc/wireguard/wg0.conf
    ```
    
    Exemple:
    
    [Interface]
    Address = 10.0.0.2/24
    PrivateKey = <clau_privada_MV-B>
    
    [Peer]
    PublicKey = <clau_pública_MV-A>
    Endpoint = <IP_MV-A>:51820
    AllowedIPs = 0.0.0.0/0
    ```
    
3. Inicia el client:
    
    ```bash
    sudo systemctl enable wg-quick@wg0
    sudo systemctl start wg-quick@wg0
    ```
    

---

### 4. Comprovació de funcionalitat:

1. Fes un ping des de **MV-B**:
    
    ```bash
    ping google.com
    ```
    
2. Captura trànsit a **MV-A** amb **tcpdump**:
    - Per la interfície física:
        
        ```bash
        sudo tcpdump -i eth1
        ```
        
    - Per la interfície WireGuard:
        
        ```bash
        sudo tcpdump -i wg0
        ```
        
3. Repetir la prova des de **MV-D** i analitzar les diferències:
    - Configura **MV-D** per utilitzar **MV-A** com a passarel·la.
    - Fes el mateix procés de ping i captura.

---
