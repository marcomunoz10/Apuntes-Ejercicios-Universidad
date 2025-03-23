# Algoritmo crypt()

Utiliza una clave de 56 bits.Coge la contraseña, lo cifra con un bloque de 64 bits de cero. El resultado lo vuelve a cifrar de la misma manera 25 veces. Los últimos 64 bits son descomprimidos en una cadena de 11 caracteres en el archivo /etc/passwd o etc/shadow.

Ataque de diccionario con contraseñas a fuerza bruta comprimiendo y comparando el valor almacenado en /etc/passwd, es por ello que se utiliza etc/shadow.

Problemas: potencia de cómputo (GPU) y descifrado de contraseñas. **No se utiliza para entrar en los sistemas**
# Algoritmo salt

Para complicar ataque de fuerza bruta con el descifrado. Salt de DES (_data encryption standard_), numero aleatorio entre 0 y 4096 (de 64 bits) que cambia ligeramente el resultado de la función DES, lo que hace que dos contraseñas iguales sean cifradas diferente.

Funcionamiento:

El programa /bin/passwd selecciona un salt **basado en la hora del dia**. Salt se convierte en cadena de dos caracteres almacenada en **etc/shadow** junto con passwd de cifrado. 

# Algoritmo utilizado hoy en dia

Formato `$id$salt$encripted`, donde `$id` determina método de cifrado (MD5, SHA-256...).

**ID  Method**
1   MD5
2a  Blowfish (solo en algunas distros) 
5   SHA-256 (desde glibc 2.7)
6   SHA-512 (desde glibc 2.7)

Por ejemplo:
`$5$salt$encrypted` es sha-256.

Aquí el **salt** es de 16 carácteres y el tamaño del passwd dependerá del método.