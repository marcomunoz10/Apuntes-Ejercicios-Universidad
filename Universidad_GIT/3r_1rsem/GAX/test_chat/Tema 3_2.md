Aquí tienes un **test de preguntas tipo test y abiertas** sobre los temas relacionados con seguridad, criptografía y PKI presentados en tu documento:

---

### **Preguntas tipo test (selección múltiple)**:

1. **¿Qué garantiza la confidencialidad en la seguridad de la información?**
    
    - a) Algoritmos de hash como MD5.
    - b) El uso de encriptación para que solo los usuarios autorizados puedan acceder.
    - c) Checksums para verificar cambios en los datos.
    - d) Certificados autofirmados.
    
    **Respuesta esperada**: **b)**.
    

---

2. **¿Qué función tienen las firmas digitales?**
    
    - a) Proporcionar integridad y confidencialidad.
    - b) Garantizar autenticación, integridad y no repudio.
    - c) Generar claves simétricas para sesiones seguras.
    - d) Proteger las contraseñas mediante hash.
    
    **Respuesta esperada**: **b)**.
    

---

3. **En PKI, un certificado digital contiene:**
    
    - a) Solo la clave privada del usuario.
    - b) La clave pública y atributos relativos a la identidad.
    - c) Un algoritmo de encriptación y una clave simétrica.
    - d) Solo el nombre y la dirección del usuario.
    
    **Respuesta esperada**: **b)**.
    

---

4. **¿Qué método se utiliza en Kerberos para autenticar a un usuario?**
    
    - a) Contraseñas en texto claro.
    - b) Criptografía de clave asimétrica.
    - c) Claves simétricas y un tercero de confianza.
    - d) Checksums para las solicitudes.
    
    **Respuesta esperada**: **c)**.
    

---

5. **¿Cuál es el principal objetivo de un protocolo de handshake en SSL/TLS?**
    
    - a) Generar una clave privada para cifrar datos.
    - b) Autenticar el servidor, negociar un cifrado y establecer una clave de sesión compartida.
    - c) Comprobar la integridad de los datos transmitidos.
    - d) Proveer no repudio mediante firmas digitales.
    
    **Respuesta esperada**: **b)**.
    

---

### **Preguntas abiertas**:

6. **Explica qué es una infraestructura PKI y menciona sus componentes principales.** **Respuesta esperada**:
    - Una PKI (Infraestructura de Clave Pública) es un sistema para gestionar y usar certificados digitales basados en claves públicas. Incluye:
        - **Autoridades de Certificación (CA)**.
        - **Autoridades de Registro (RA)**.
        - **Sistemas de distribución de certificados**.
        - **Protocolos de validación (como OCSP)**.
        - **Certificados digitales (X.509)**.

---

7. **Describe cómo funciona Kerberos para autenticar a un usuario y establecer una comunicación segura.** **Respuesta esperada**:
    - **Kerberos** utiliza criptografía simétrica y un tercero de confianza llamado KDC (Key Distribution Center). Proceso:
        - El cliente solicita un servicio al AS (Authentication Server) con su nombre de usuario.
        - El AS envía un ticket cifrado con la clave del usuario y del TGS.
        - El cliente usa el ticket para autenticarse con el TGS, que genera otro ticket para acceder al servidor final.

---

8. **Diferencia entre claves simétricas y asimétricas, mencionando ejemplos de algoritmos.** **Respuesta esperada**:
    - **Claves simétricas**: Usan la misma clave para cifrar y descifrar datos. Ejemplo: AES, DES.
    - **Claves asimétricas**: Usan un par de claves (pública y privada). La clave pública cifra, y la privada descifra. Ejemplo: RSA, ECC.

---

9. **Explica cómo se asegura la integridad y no repudio en un sistema de firmas digitales.** **Respuesta esperada**:
    - Se calcula un **hash** del mensaje, que se cifra con la clave privada del remitente (firma digital).
    - El receptor descifra la firma con la clave pública del remitente y compara el hash recibido con el hash recalculado del mensaje.
    - Esto asegura que:
        - **Integridad**: El mensaje no fue alterado.
        - **No repudio**: El remitente no puede negar haber enviado el mensaje.

---

10. **Menciona las diferencias entre SSL y TLS.** **Respuesta esperada**:
    - **SSL**: Protocolo original (Netscape) con versiones obsoletas (2.0 y 3.0).
    - **TLS**: Evolución de SSL, más seguro y actualizado (última versión: TLS 1.3, 2018).
    - Ambos garantizan autenticación, integridad y confidencialidad.

---

Aquí tienes una lista ampliada con **muchas más preguntas** sobre los temas relacionados con **seguridad de la información, criptografía, PKI, Kerberos y Squid**, organizadas en tipo test y preguntas abiertas.

---

### **Preguntas tipo test**:

1. **¿Qué objetivo de seguridad garantiza que los datos no puedan ser alterados durante su transmisión?**
    
    - a) Confidencialidad.
    - b) Autorización.
    - c) Integridad.
    - d) Autenticación.
    
    **Respuesta esperada**: **c)**.
    
2. **¿Cuál de las siguientes técnicas se utiliza para garantizar la confidencialidad de los datos?**
    
    - a) Checksums.
    - b) Algoritmos de hash.
    - c) Encriptación.
    - d) Firmas digitales.
    
    **Respuesta esperada**: **c)**.
    
3. **¿Qué herramienta proporciona no repudio en comunicaciones electrónicas?**
    
    - a) Encriptación simétrica.
    - b) Firmas digitales.
    - c) Checksums.
    - d) Certificados autofirmados.
    
    **Respuesta esperada**: **b)**.
    
4. **¿Qué algoritmo de encriptación utiliza claves públicas y privadas?**
    
    - a) AES.
    - b) RSA.
    - c) DES.
    - d) HMAC.
    
    **Respuesta esperada**: **b)**.
    
5. **¿Qué protocolo permite la autenticación segura basada en un servidor de claves compartidas?**
    
    - a) SSL/TLS.
    - b) Kerberos.
    - c) IPsec.
    - d) DNSSEC.
    
    **Respuesta esperada**: **b)**.
    
6. **¿Cuál es la diferencia clave entre encriptación simétrica y asimétrica?**
    
    - a) La simétrica utiliza dos claves, la asimétrica solo una.
    - b) La asimétrica utiliza un par de claves (pública y privada), la simétrica una sola clave.
    - c) La simétrica es más segura que la asimétrica.
    - d) La asimétrica no requiere clave privada.
    
    **Respuesta esperada**: **b)**.
    
7. **¿Qué estándar de certificados digitales se utiliza en una PKI?**
    
    - a) X.509.
    - b) SSL.
    - c) SHA-256.
    - d) PGP.
    
    **Respuesta esperada**: **a)**.
    
8. **¿Qué puerto se utiliza comúnmente en un proxy Squid transparente?**
    
    - a) 3128.
    - b) 443.
    - c) 8080.
    - d) 80.
    
    **Respuesta esperada**: **c)**.
    
9. **En Squid, una ACL se utiliza para:**
    
    - a) Crear reglas de autorización basadas en IP o URL.
    - b) Configurar el almacenamiento en caché.
    - c) Establecer protocolos de encriptación.
    - d) Realizar autenticación mediante claves públicas.
    
    **Respuesta esperada**: **a)**.
    
10. **¿Qué puerto se utiliza típicamente para certificados digitales SSL?**
    
    - a) 80.
    - b) 443.
    - c) 8080.
    - d) 21.
    
    **Respuesta esperada**: **b)**.
    
11. **¿Cuál es la función principal de una autoridad de certificación (CA)?**
    
    - a) Proveer claves de cifrado para usuarios.
    - b) Firmar y emitir certificados digitales confiables.
    - c) Cifrar datos para proteger su confidencialidad.
    - d) Administrar firewalls en la red.
    
    **Respuesta esperada**: **b)**.
    
12. **¿Qué es un hash criptográfico?**
    
    - a) Un algoritmo que cifra datos para garantizar su confidencialidad.
    - b) Una función unidireccional que convierte datos en un valor único de longitud fija.
    - c) Un método para distribuir claves públicas.
    - d) Un algoritmo que realiza cifrado bidireccional.
    
    **Respuesta esperada**: **b)**.
    
13. **¿Qué protocolo en Squid permite la comunicación entre proxies?**
    
    - a) ICP.
    - b) Kerberos.
    - c) SSL/TLS.
    - d) DNSSEC.
    
    **Respuesta esperada**: **a)**.
    
14. **¿Qué elemento de la PKI permite comprobar si un certificado ha sido revocado?**
    
    - a) OCSP (Online Certificate Status Protocol).
    - b) X.509.
    - c) SHA-256.
    - d) SSL.
    
    **Respuesta esperada**: **a)**.
    

---

### **Preguntas abiertas**:

15. **Explica la diferencia entre autenticación y autorización, con un ejemplo práctico.**
    
16. **Describe los pasos básicos del funcionamiento de Kerberos en la autenticación de usuarios.**
    
17. **¿Cómo asegura la integridad un algoritmo de hash como SHA-256? Da un ejemplo.**
    
18. **Explica la diferencia entre un proxy directo (forward proxy) y un reverse proxy, y menciona un caso de uso para cada uno.**
    
19. **Describe cómo funciona un certificado digital en una infraestructura PKI y qué información contiene.**
    
20. **¿Qué pasos seguirías para configurar Squid como proxy transparente en Debian?**
    
21. **¿Por qué es importante garantizar el no repudio en las comunicaciones digitales y qué herramienta criptográfica lo permite?**
    
22. **Menciona tres diferencias entre un algoritmo de encriptación simétrico como AES y uno asimétrico como RSA.**
    
23. **¿Cómo se utiliza un hash combinado con una clave secreta (HMAC) para proporcionar autenticación en una comunicación?**
    
24. **¿Qué problemas resuelve el uso de OCSP en comparación con las CRLs (Certificate Revocation Lists)?**
    
25. **¿Qué es el handshake en TLS/SSL y qué pasos clave incluye para establecer una conexión segura?**
    

---

Estas preguntas cubren una amplia gama de temas y están diseñadas para evaluar tanto conocimiento teórico como práctico. Si necesitas que amplíe alguna de ellas o genere más, ¡solo dímelo! 😊