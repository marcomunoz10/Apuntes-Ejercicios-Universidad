
Es el proceso de evaluar la seguridad de un sistema o aplicación mediante la identificación de vulnerabilidades y amenazas potenciales. Incluye enfoques como **Threat Modeling** (modelado de amenazas) para prever ataques y diseñar defensas adecuadas.

### **Security Certification (formal) 

Proceso en el que un sistema es evaluado contra estándares de seguridad reconocidos (como **ISO 27001, NIST, SOC 2**). Una certificación formal implica auditorías y pruebas rigurosas para garantizar el cumplimiento de requisitos específicos de seguridad.

### **Security Analysis (find/prevent security issues) 

Este proceso busca detectar vulnerabilidades antes de que sean explotadas. Se divide en varias técnicas:

#### **1. Static analysis of code

- Se examina el código fuente sin ejecutarlo para buscar errores de programación, vulnerabilidades de seguridad y malas prácticas. Durante la fase de implementación. No encuentra todas las vulnerabilidades.

#### **2. Fuzzing or dynamic testing 

- Se prueba un sistema enviándole entradas aleatorias o maliciosas para detectar fallos.
- Detecta vulnerabilidades como desbordamientos de búfer o problemas de validación de entrada.
**Fuzzers de Mutación**
- Toman una entrada válida y la modifican aleatoriamente para generar nuevas pruebas.

**Fuzzers Generativos**
- Crean entradas que siguen las reglas del sistema bajo prueba, pero con modificaciones diseñadas para encontrar errores.
#### **3. Pentesting (Pruebas de penetración)**

- Simulación de ataques reales para encontrar vulnerabilidades explotables.
- Se evalúa la seguridad de redes, aplicaciones y sistemas.
- Puede ser **white-box** (con acceso al código) o **black-box** (sin acceso previo).
- Herramientas: **Metasploit, Burp Suite, Nmap, Kali Linux**.

#### **4. Threat modeling (Modelado de amenazas)**

- Identifica amenazas potenciales desde el diseño del sistema.
- Usa metodologías como **STRIDE** (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege).
- Ayuda a priorizar medidas de seguridad antes del desarrollo o despliegue.

En conjunto, estas técnicas fortalecen la seguridad de un sistema al detectar y mitigar vulnerabilidades antes de que puedan ser explotadas.