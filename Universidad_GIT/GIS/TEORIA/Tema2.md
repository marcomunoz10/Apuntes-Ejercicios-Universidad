# User authentication

# Password
Contraseña: Un valor secreto que presentado por un usuario a un sistema para autenticar su identidad.
Se guarda utilizando funciones hash y el salt (para que no todos los hashes den el mismo resutlado)

## Ataques de contraseñas
- Adivinación de contraseñas en línea. (ataques de fuerza bruta o diccionario)
- Adivinación de contraseñas offline. (ataques realizados sobre hashes de contraseñas previamente obtenidos)
- Captura de contraseñas (observación mientras la ingresa, sw o hw que captura las teclas...)
- Evasión de la interfaz de autenticación (explotación de vulnerabilidades para acceder sin autentificación)
- Derrotar mecanismos de autenticación.

### Adivinación de contraseñas en línea

- Posible ante cualquier sistema
- Adivinación automática utilizando un username
Defensa:
- Límite de intentos por usuario
- Aumentar el delay
La limitación de accesos puede hacer que el atacante bloquee el acceso de la víctima (solución: captcha para diferenciar humanos y bots)


# One time passwords
Hay contraseñas válidas de un solo uso, normalmente basadas en la autenticación por tokens.
Ventajas:
- El servidor le envía la contraseña al cliente
- Cliente y servidor generan la contraseña desde una semilla secreta.
Tipos:
- Contador
- Tiempo
- Cadenas de hashes. 

## Inicialización simple

### Registro
Servidor y cliente comparten una contraseña secreta sobre un canal seguro. (se inicializa el registro mediante una semilla secreta usando un QR o un input numérico.)

### Generación
Servidor y cliente pueden generar la misma contraseña desde ela misma semilla al mismo momento.


## HMAC-based OTP (one time passwords)

El estándar es el RFC-4226
```
K: Comparten una semilla secreta
C: Contador (sincronizado)
S: Parámetro de resincronización (el servidor intentará verificiar si hay s valores consecutivos)
```

$$ OTP = Truncate(HMAC(K,C)$$

- Trunca la salida de HMAC a 6-10 dígitos.
- El contador se incrementa después de cada intento de registro exitoso. (puede soportar resincronización)