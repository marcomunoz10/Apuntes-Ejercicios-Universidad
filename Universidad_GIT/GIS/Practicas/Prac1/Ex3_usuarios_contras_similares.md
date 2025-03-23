
### Separar el usuario del mail con el "@"

```python
if "@" in mail:
```


![[Pasted image 20250312162903.png]]

![[Pasted image 20250312162917.png]]

# Código

Si la contraseña es más del 80% igual quiere decir que es parecida. Si es más del 95 quiere decir que es igual.
```python
#!/usr/bin/env python3
import os
import re
from tqdm import tqdm
from collections import defaultdict

# Directorio con los archivos
directory_path = "/home/kali/Desktop/GIS/PRAC1/n"

# Diccionario: {usuario: {conjunto de contraseñas}}
user_passwords = defaultdict(set)

# Separadores posibles entre email y contraseña
separators = r"[:;| ]"

# Función para normalizar las contraseñas (convertir a minúsculas)
def normalize_password(password):
    return password.lower()

# Función para calcular la similitud entre dos cadenas (porcentaje de coincidencia)
def calculate_similarity(password1, password2):
    # Normalizar las contraseñas para ignorar diferencias de mayúsculas/minúsculas
    password1 = normalize_password(password1)
    password2 = normalize_password(password2)
    
    # Encontrar el número de caracteres coincidentes en la misma posición
    matches = sum(1 for a, b in zip(password1, password2) if a == b)
    
    # Calcular la similitud basada en la longitud de la contraseña más corta
    max_length = max(len(password1), len(password2))
    
    if max_length == 0:
        return 1.0  # Si las dos contraseñas están vacías (cosa rara, pero posible)
    
    similarity = matches / max_length
    
    return similarity

# Obtener todos los archivos en el directorio
all_entries = os.listdir(directory_path)
all_files = [entry for entry in all_entries if os.path.isfile(os.path.join(directory_path, entry))]

# Procesar cada archivo
for filename in tqdm(all_files, desc="Processing files", unit="file"):
    file_path = os.path.join(directory_path, filename)
    
    with open(file_path, "r", encoding="utf-8", errors="ignore") as file:
        for line in file:
            if not line.strip():
                continue  # Omitir las líneas vacías
            
            parts = re.split(separators, line, maxsplit=1)
            if len(parts) == 2:
                email = parts[0].strip()
                password = parts[1].strip()

                # Filtrar contraseñas inválidas
                if ".txt" in password or not password or not password.isprintable():
                    continue  

                # Extraer el usuario (antes del "@")
                if "@" in email:
                    user = email.split("@")[0]

                    # Agregar la contraseña al conjunto de contraseñas del usuario
                    user_passwords[user].add(password)

# Filtrar usuarios con más de una contraseña
users_with_multiple_passwords = {user: list(passwords) for user, passwords in user_passwords.items() if len(passwords) > 1}

# Calcular la similitud entre las contraseñas de cada usuario
for user, passwords in users_with_multiple_passwords.items():
    print(f"Usuario: {user}")
    
    # Imprimir todas las contraseñas del usuario
    print("  Contraseñas del usuario:")
    for pwd in passwords:
        print(f"    {pwd}")

    similar_password_pairs = []  # Almacenar los pares de contraseñas similares
    
    # Comparar todas las contraseñas entre ellas
    for i in range(len(passwords)):
        for j in range(i + 1, len(passwords)):
            similarity = calculate_similarity(passwords[i], passwords[j])
            if similarity >= 0.8:  # Si la similitud es superior o igual al 80%
                similar_password_pairs.append((passwords[i], passwords[j], similarity))

    # Si el usuario tiene contraseñas similares, mostrarlas
    if similar_password_pairs:
        print("  Contraseñas similares:")
        for pwd1, pwd2, sim in similar_password_pairs:
            print(f"    {pwd1} - {pwd2} (Similitud: {sim*100:.2f}%)")
    
    print("\n")

```

