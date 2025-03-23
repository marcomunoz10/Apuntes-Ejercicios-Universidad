o
Per resoldre aquest exercici a Kali Linux, seguirem aquests passos:

El hash h91 correspon a una de les 100 contrasenyes més utilitzades pels usuaris d’Adobe, concatenada amb un any i un símbol. 
Hash:
`adacf9fd76805303d9126dbbfeb9262809b8306e5010027b6337a8278efa0e67 `
### **1. Determinar la funció hash**
Com que el hash proporcionat és de 64 caràcters, és probable que sigui **SHA-256**. Ho podem confirmar amb eines com `hash-identifier`:

```bash
echo -n "adacf9fd76805303d9126dbbfeb9262809b8306e5010027b6337a8278efa0e67" | hash-identifier
```

Si es confirma que és SHA-256, procedirem amb el següent pas.

---

### **2. Obtenir un diccionari base**
Sabem que la contrasenya prové d’una de les **100 contrasenyes més comunes d’Adobe**. Podem descarregar aquestes contrasenyes de la llista filtrada de l'incident d’Adobe:

```bash
curl -O https://raw.githubusercontent.com/danielmiessler/SecLists/master/Passwords/Leaked-Databases/adobe100.txt
```

---

### **3. Generar un diccionari amb alteracions**
Hem de modificar la llista de contrasenyes afegint-hi **un any i un símbol**. Això ho podem fer amb `Crunch` o `awk`:

```bash
awk '{for(y=1899; y<=2025; y++) {for(i=1;i<=length("!@#$%_*-&+=<>^");i++) print $0 y substr("!@#$%_*-&+=<>^",i,1)}}' adobe100.txt > dict_expanded_final.txt

```

Aquest script afegeix diferents anys i un símbol  a cada contrasenya.

---

### **4. Atacar el hash amb Hashcat**
Ara utilitzem **Hashcat** per atacar el hash amb el diccionari generat:

```bash
hashcat -m 1400 -a 0 adacf9fd76805303d9126dbbfeb9262809b8306e5010027b6337a8278efa0e67 dict_expanded_final.txt --force
```

Explicació:
- `-m 1400`: Mode SHA-256.
- `-a 0`: Atac de diccionari.
- `dict_variants.txt`: Diccionari amb variacions de les contrasenyes.
- `--force`: Força l’execució si Hashcat detecta problemes amb drivers.

---
