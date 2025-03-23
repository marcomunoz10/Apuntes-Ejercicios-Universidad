
```python
import hashlib

# Contraseña que quieres hashear
password = "Montblanc"

# Crear un objeto hash utilizando SHA-256
hashed_password = hashlib.sha256(password.encode()).hexdigest()

# Mostrar el hash resultante
print("SHA-256 Hash de 'Montblanc':", hashed_password)
```