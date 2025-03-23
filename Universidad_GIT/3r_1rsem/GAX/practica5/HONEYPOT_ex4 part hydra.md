
### 2. **Instal·la Hydra a MV-C**

Instal·la **Hydra** o **Hydra-gtk** a **MV-C** per generar intents d'accés il·legítim:

```bash
sudo apt install hydra hydra-gtk -y
```

(Assegurar-se que `sudo systemctl stop systemd-resolved`)
### 3. **Executa Hydra per fer atacs de força bruta**

S'instal·la el rockyou-10, desde **[downloads.skullsecurity.org/passwords](http://downloads.skullsecurity.org/passwords

se crea la carpeta /worldlists en /usr/share.

Se instala bunzip2. `sudo apt install bunzip2`.

Se descomprime, se copia en `/usr/share/worldlists/rockyou-10`

Genera intents de connexió SSH cap a MV-D (que està executant el honeypot).

- Exemple d'atac de força bruta a SSH:
    
    ```bash
    hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://30.30.30.80
    ```
    
    - **`-l root`**: Usuari que Hydra intentarà autenticar (en aquest cas, `root`).
    - **`-P`**: Indica el fitxer de contrasenyes que utilitzarà. El fitxer `rockyou.txt` és un diccionari comú, disponible al paquet `wordlists`.
- Per un atac HTTP (si el honeypot té aquest port activat):
    
    ```bash
    hydra -l admin -P /usr/share/wordlists/rockyou.txt http-get://30.30.30.80
    ```
    

---

## **Anàlisi del Comportament a MV-D**

### 1. **Monitorització del honeypot**

El honeypot registrarà automàticament tots els intents d’accés i generarà logs. Pots veure aquests logs a la consola o redirigir-los a un fitxer per analitzar-los més tard.

- **Exemple de sortida del honeypot** (intents d’accés SSH):
    
    ```
    [INFO] Connection attempt from 30.30.30.81:52234
    [ALERT] Brute force attempt detected: SSH user=root password=123456
    ```
    
- Si necessites guardar els logs:
    
    ```bash
    honeypots --port 22 > honeypot.log
    ```
    

### 2. **Comprovar els logs del sistema**

Els intents també poden quedar registrats al sistema, especialment si el honeypot redirigeix missatges als logs:

```bash
sudo journalctl -u honeypots
```

---

## **Lliurament**

Per completar l'exercici, realitza captures de pantalla de les següents accions:

1. **Sortida de `ping`, `nmap` i altres intents des de MV-C cap a MV-D.**
2. **Execució d’atacs amb Hydra i els resultats a la consola de MV-C.**
3. **Registres del honeypot a MV-D que mostrin les connexions i intents d’atac.**

---

Amb aquests passos podràs completar l'exercici i analitzar el comportament del honeypot. Si tens més dubtes, estaré encantat d'ajudar! 😊