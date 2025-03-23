# Seguridad a nivel de usuarios
## Ejercicio 1

1. Crear usuari troll amb contrasenya troll amb la shell de rbash per tenir accès restringit
```shell
sudo adduser troll
#preguntan contraseña
troll
```

`rbash`, o **bash restringit**, és una versió limitada del shell `bash` que restringeix moltes de les accions que un usuari pot fer, especialment útil per a usuaris de baixa confiança o amb privilegis reduïts. L'objectiu principal de `rbash` és impedir que un usuari canviï configuracions o executi accions que puguin comprometre la seguretat del sistema.

2. Canviem la shell de l'usuari a `rbash`
```shell
sudo usermod -s /bin/rbash troll
```

Si soy usuario `adminp` y quiero cambiar a `troll` hago:

```shell
su - troll
```

b)

# Seguridad a nivel de RED
## Ejercicio 2

#### Atac DOS
### 1. Idea de la Solució
Per prevenir atacs de denegació de servei (**DoS**), Apache es pot configurar com a **proxy server** amb funcions de **cache**. Aquesta configuració permet a Apache limitar i gestionar millor les peticions entrants, bloquejar dominis sospitosos, i servir continguts emmagatzemats en cache, reduint la càrrega en el servidor principal.

### 2. Procés de Configuració
   - **Activació de Mòduls Necessaris**: Cal activar els mòduls de proxy i cache amb les comandes:
     ```bash
     a2enmod proxy proxy_http headers rewrite
     ```
     Això permetrà que Apache funcioni com a servidor proxy.
   
   - **Configuració com a Proxy Forward o Reverse**: 
     Es crea un host virtual amb configuracions bàsiques de proxy per definir el comportament com a **proxy forward** o **reverse**. Exemple:
     ```apache
     ProxyRequests On
     <Proxy "*">
         Order deny,allow
         Deny from all
         Allow from 192.168.0.0/24
     </Proxy>
     ```
     Això permetrà l'accés al proxy només a clients dins del rang d'IP definit.
   
   - **Cache de Continguts**: Per habilitar la cache i reduir el processament de peticions repetitives, es pot configurar el cache en disc:
     ```apache
     <IfModule mod_disk_cache.c>
         CacheRoot "/usr/local/apache/proxy"
         CacheSize 500
         CacheDirLevels 5
         CacheDirLength 3
     </IfModule>
     ```

   - **Blocatge de Dominis Maliciosos**: Amb `ProxyBlock`, es poden bloquejar dominis coneguts com a maliciosos:
     ```apache
     ProxyBlock .dominiomaliciós.com
     ```

### 3. Verificació del Funcionament
   - **Proves de Connexió**: Configura un client per accedir a internet a través del proxy d’Apache i comprova que les peticions s'envien correctament i els dominis bloquejats no són accessibles.
   - **Consulta de Registres**: Revisa els logs d'Apache per verificar que les peticions es gestionen correctament, el cache funciona i els dominis bloquejats es rebutgen.
   - **Simulació de Peticions Massives**: Per garantir la resistència al DoS, es pot simular un alt volum de peticions i comprovar si Apache gestiona la càrrega de manera efectiva.

Aquests passos configuren Apache per protegir-se d'atacs DoS, mantenint l'estabilitat i disponibilitat del servei.

---

#### Desactivar qualsevol connexió des d'una IP concreta
Es pot utilitzar [[iptables]] per crear una regla que bloquegi qualsevol intent de connexió des d'aquesta IP.

```bash
sudo iptables -A INPUT -p tcp --dport 22 -s A.B.C.D -j DROP
```
- **`-A INPUT`**: Afegim la regla a la cadena `INPUT`, que gestiona el tràfic entrant.
- **`-p tcp`**: Especifica que la regla s'aplica al protocol TCP.
- **`--dport 22`**: Limita la regla al port 22, el port per defecte de SSH.
- **`-s A.B.C.D`**: Aplica la regla només a l’adreça IP especificada.
- **`-j DROP`**: Indica que el tràfic coincident ha de ser bloquejat (descartat).

## Ejercicio 3: Desplegar una DMZ


### 1. Configuracions de Xarxa

#### Màquina A
- **Ruta de xarxa per a la DMZ**: Afegeix una regla de ruteig per arribar a la xarxa DMZ. 
```shell
sudo ip route add <IP_de_la_DMZ>/24 via <IP_de_B_a_la_DMZ>

```

#### Màquina E
- **Gateway per defecte**: Configura la IP de B a la xarxa DMZ com a `default gateway` per a E. Aquesta acció farà que tot el tràfic de sortida de la màquina E passi a través de B.
  ```bash
  sudo ip route add default via <IP_de_B_a_la_DMZ>
  ```

#### Màquina B
- **Configuració de la nova interfície a DMZ**:
  - Primer, connecta una nova interfície de xarxa (tal com especifica l'exercici) a la xarxa DMZ i activa-la:
    ```bash
    sudo ip link set <nova_interfície> up
    ```
  - `<nova_interficie>` pot ser per exemple  `eth2` 
  - Edita el fitxer de configuració de netplan per afegir la IP assignada a la nova interfície.
  - Aplica els canvis amb `netplan apply`.

### 2. Configuració del Firewall (Màquina B)

Es necessita un script `iptables` per configurar les regles del firewall. Aquest script ha de permetre i bloquejar els accessos segons les especificacions de l'exercici. Pots començar amb un fitxer `script_dmz.sh` i afegir comentaris per cada regla.

A continuació, tens una plantilla bàsica d'script amb exemples de regles:

```bash
#!/bin/bash

# Reseteja les regles existents
iptables -F
iptables -X

# Bloqueja tot el tràfic per defecte
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT DROP

# Permetre tràfic localhost
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT

# Permetre connexions establertes
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -A OUTPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Des de C i D es podrà connectar a E per a serveis Apache i ssh
iptables -A FORWARD -s <IP_C> -d <IP_E> -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -s <IP_C> -d <IP_E> -p tcp --dport 22 -j ACCEPT
iptables -A FORWARD -s <IP_D> -d <IP_E> -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -s <IP_D> -d <IP_E> -p tcp --dport 22 -j ACCEPT

# Des de C i D es poden connectar a https://duckduckgo.com/ (port 443)
iptables -A FORWARD -s <IP_C> -d 52.142.124.201 -p tcp --dport 443 -j ACCEPT
iptables -A FORWARD -s <IP_D> -d 52.142.124.201 -p tcp --dport 443 -j ACCEPT

# Des de E NO es pot connectar a cap servei de B, C, D
iptables -A FORWARD -s <IP_E> -d <IP_B> -j REJECT
iptables -A FORWARD -s <IP_E> -d <IP_C> -j REJECT
iptables -A FORWARD -s <IP_E> -d <IP_D> -j REJECT

# Des de E pot connectar-se a A (per serveis HTTP i SSH) i a duckduckgo.com
iptables -A FORWARD -s <IP_E> -d <IP_A> -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -s <IP_E> -d <IP_A> -p tcp --dport 22 -j ACCEPT
iptables -A FORWARD -s <IP_E> -d 52.142.124.201 -p tcp --dport 443 -j ACCEPT

# Des de A es pot connectar a E per HTML (80) i SSH (22)
iptables -A FORWARD -s <IP_A> -d <IP_E> -p tcp --dport 80 -j ACCEPT
iptables -A FORWARD -s <IP_A> -d <IP_E> -p tcp --dport 22 -j ACCEPT

# Finalitza amb ACCEPT per al tràfic que compleix les regles anteriors
iptables -A INPUT -j ACCEPT
iptables -A OUTPUT -j ACCEPT
```

### 3. Validació de la Configuració
Per comprovar que tot funciona segons el que s'ha especificat, fes captures de pantalla després d'executar l'script i verifica que cada regla s'està complint:
- Utilitza eines com `curl`, `ssh` o `telnet` per provar l'accés.
- Pots utilitzar `iptables -L -v` per revisar les regles actives i la seva efectivitat en cada connexió. 

Aquesta configuració cobreix els requeriments generals, però recorda adaptar els IP específics al teu entorn.
# Ataque DOS
## Ejercicio 4

# Ataque SSH login


## Ejercicio 5

