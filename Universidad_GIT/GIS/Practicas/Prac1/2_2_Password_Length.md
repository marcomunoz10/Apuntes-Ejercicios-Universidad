![[Pasted image 20250312193305.png]]

### **2. The lengths of the passwords (i.e., how many passwords of each length are there in D). What is the shortest password? And the longest? What are the average and median lengths?**

```python
#!/usr/bin/env python3
import os
import re
from tqdm import tqdm
import matplotlib.pyplot as plt
from collections import Counter
import numpy as np

# Ruta del directori on es troben els fitxers
directory_path = "/home/kali/Desktop/GIS/PRAC1/n"

separators = r"[:;| ]"

contrasenyes = []

# Llistem els fitxers del directori
entrades = os.listdir(directory_path)
fitxers = [entry for entry in entrades if os.path.isfile(os.path.join(directory_path, entry))]

# Processament dels fitxers
for filename in tqdm(fitxers, desc="Processant fitxers", unit="fitxer"):
    file_path = os.path.join(directory_path, filename)
    with open(file_path, "r", encoding="utf-8", errors="ignore") as file:
        for line in file:
            if not line.strip():
                continue
            parts = re.split(separators, line, maxsplit=1)
            if len(parts) == 2:
                _, contrasenya = parts
                contrasenya = contrasenya.strip()

                if ".txt" in contrasenya or not contrasenya.isprintable() or len(contrasenya) < 1:
                    continue

                contrasenyes.append(contrasenya)

# Obtenir la longitud de cada contrasenya
longituds_contrasenyes = sorted([(len(p), p) for p in contrasenyes])

# Seleccionar les 5 contrasenyes més curtes (diferents)
contrasenyes_mes_curtes = []
contrasenes_vistes = set()

for length, pwd in longituds_contrasenyes:
    if length not in contrasenes_vistes:
        contrasenes_vistes.add(length)
        contrasenyes_mes_curtes.append((length, pwd))
    if len(contrasenyes_mes_curtes) == 5:
        break

# Seleccionar les 5 contrasenyes més llargues
contrasenyes_mes_llargues = longituds_contrasenyes[-5:]

# Calcular la mitjana i la mediana
mitjana_longitud = np.mean([length for length, _ in longituds_contrasenyes])
mediana_longitud = np.median([length for length, _ in longituds_contrasenyes])

# Preparar les dades per al gràfic
etiquetes = [f"Curta {i+1}" for i in range(5)] + [f"Llarg {i+1}" for i in range(5)]
longituds = [length for length, _ in contrasenyes_mes_curtes] + [length for length, _ in contrasenyes_mes_llargues]

# Crear el gràfic
plt.figure(figsize=(10, 6))
plt.bar(etiquetes, longituds, color='skyblue', edgecolor='black')

# Afegir línies per indicar la mitjana i la mediana
plt.axhline(y=mitjana_longitud, color='red', linestyle='dashed', linewidth=2, label=f'Mitjana: {mitjana_longitud:.2f}')
plt.axhline(y=mediana_longitud, color='green', linestyle='dashed', linewidth=2, label=f'Mediana: {mediana_longitud:.2f}')

# Etiquetes i títol
plt.title("Longituds de les contrasenyes més curtes i més llargues")
plt.ylabel("Longitud de la contrasenya")
plt.legend()
plt.tight_layout()
plt.show()

# Mostrar a la consola les contrasenyes seleccionades
print("\n Contrasenyes més curtes:")
for i, (length, pwd) in enumerate(contrasenyes_mes_curtes, 1):
    print(f"{i}. Longitud {length}: {pwd}")

print("\n Contrasenyes més llargues:")
for i, (length, pwd) in enumerate(contrasenyes_mes_llargues, 1):
    print(f"{i}. Longitud {length}: {pwd}")

print(f"\n Longitud mitjana: {mitjana_longitud:.2f}")
print(f" Longitud mediana: {mediana_longitud:.2f}")


```