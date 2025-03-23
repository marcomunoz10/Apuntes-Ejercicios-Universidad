**PKI** (Infraestructura de Clave Pública, por sus siglas en inglés: **Public Key Infrastructure**) es un sistema de gestión de claves criptográficas utilizado para facilitar la seguridad en la comunicación digital. Su objetivo principal es proporcionar **autenticación, integridad, confidencialidad y no repudio** a través del uso de **criptografía de clave pública**.

### Componentes principales de PKI:

1. **Clave pública y clave privada**:
    - **Clave pública**: Es utilizada para **cifrar** datos o verificar una firma digital.
    - **Clave privada**: Es utilizada para **descifrar** datos o firmar digitalmente información.

2. **Autoridad de Certificación (CA)**: Es la entidad que emite los **certificados digitales**, asegurando que la clave pública pertenece a su propietario legítimo.

3. **Certificados digitales**: Son documentos que **vinculan** una **clave pública** con la identidad de un individuo o **entidad**, garantizando que la clave pública pertenece realmente a quien dice ser.

4. **Repositorio de certificados**: Es un sistema donde se almacenan y gestionan los certificados digitales y las claves públicas.

5. **Política de gestión de claves**: Define las **reglas** y procedimientos para la gestión de las claves y certificados dentro del sistema PKI.

PKI se utiliza en aplicaciones como el correo electrónico seguro, firmas digitales, VPNs, y autenticación de sitios web (como el HTTPS).

---
**Organizaciones que desarrollan los estándares** utilizados en sistemas PKI para garantizar su interoperabilidad y seguridad:

1. **IETF (Internet Engineering Task Force)**: Desarrolla estándares para Internet, como los **RFC**(Request For Comments), que incluyen especificaciones para la seguridad en la web (ej. RFC 5280, que define los certificados X.509 utilizados en PKI).

2. **ISO (International Standards Organisation)**: Define estándares globales de seguridad, como la **ISO/IEC 27001**, que cubre la gestión de la seguridad de la información, incluyendo PKI.

3. **ETSI (European Telecommunications Standards Institute)**: Establece normas de telecomunicaciones y seguridad en Europa, como el **ETSI EN 319 411-1**, que regula la firma digital y certificados.

4. **RSA Laboratories**: Publica los **PKCS** (Public Key Cryptography Standards), como el **PKCS#12**, que especifica cómo almacenar claves privadas y certificados de forma segura.

Estos estándares aseguran la interoperabilidad entre aplicaciones que usan PKI para la seguridad.