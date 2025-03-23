	Iptables és un firewall.

La sintaxi d'una ordre d'iptables segueix una estructura general que inclou una sèrie de components, cadascun amb una funció específica:

### Estructura bàsica d'una regla d'iptables

```bash
sudo iptables -<ACCIO> <CADENA> [OPCIONS DE COINCIDÈNCIA] -j <OBJECTIU>
```

- **`-<ACCIO>`**: Determina l'acció a realitzar sobre la regla. L'opció més habitual és:
  - **`-A`** (append): Afegeix la regla al final d’una cadena.
  
  Altres accions inclouen:
  - **`-I`** (insert): Insereix la regla en una posició específica dins la cadena.
  - **`-D`** (delete): Elimina una regla de la cadena.
  - **`-R`** (replace): Reemplaça una regla en una posició específica.

- **`<CADENA>`**: La cadena o taula en què s’aplicarà la regla. Les més comunes són:
  - **`INPUT`**: Per al tràfic entrant al sistema.
  - **`OUTPUT`**: Per al tràfic sortint des del sistema.
  - **`FORWARD`**: Per al tràfic que passa pel sistema però no hi està destinat.

- **Opcions de coincidència**: Definen els criteris per a la regla:
  - **`-p`**: Protocol (com `tcp`, `udp`, o `icmp`).
  - **`--dport`**: Port de destinació.
  - **`--sport`**: Port d'origen.
  - **`-s`**: Adreça IP d’origen.
  - **`-d`**: Adreça IP de destinació.
  
- **`-j <OBJECTIU>`**: Defineix l’acció a realitzar quan es compleixen les condicions de la regla. Els objectius més comuns són:
  - **`ACCEPT`**: Permet el tràfic.
  - **`DROP`**: Descarta el tràfic.
  - **`REJECT`**: Rebutja el tràfic amb un missatge d’error al remitent.
  - **`LOG`**: Registra la connexió en els registres del sistema.

### Exemple d'ús d'altres accions

- **Inserir** (`-I`): Insereix la regla al principi de la cadena `INPUT`, donant-li una prioritat més alta que les regles que ja hi són.
  ```bash
  sudo iptables -I INPUT 1 -p tcp --dport 22 -s A.B.C.D -j DROP
  ```

- **Eliminar** (`-D`): Elimina una regla específica de la cadena `INPUT` (s'especifica exactament la mateixa regla o la posició dins la cadena).
  ```bash
  sudo iptables -D INPUT -p tcp --dport 22 -s A.B.C.D -j DROP
  ```

En resum, no sempre es fa servir `-A`; l’ús de `-A`, `-I`, `-D` o altres depèn de com es vulguin gestionar les regles dins les cadenes.