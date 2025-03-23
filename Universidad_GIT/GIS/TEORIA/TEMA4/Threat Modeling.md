[+] Key Questions
1. What are you building? (model)  
2. What can go wrong? 
3. What should you do about those things that can go wrong?
4. Did you do a decent job of analysis?

#### 1.What are you building? (model)

Crear un modelo simple para definir quién controla qué.

**Trust Boundary**
Es el punto donde se cruzan diferentes niveles de confianza.

### **Ejemplos de Trust Boundaries**

1. **Cuentas de usuario en sistemas UNIX (UIDs)**
    
    - En sistemas operativos como UNIX, cada usuario tiene un identificador único (UID).
        
    - El límite de confianza está en la separación de privilegios entre diferentes UID, evitando que un usuario normal acceda a archivos o procesos de otro usuario o del sistema.
        
2. **Interfaces de red**
    
    - La comunicación entre diferentes redes (por ejemplo, una red interna y la red pública de Internet) marca un límite de confianza.
        
    - Los firewalls y sistemas de autenticación controlan qué datos pueden cruzar esta barrera.
        
3. **Diferentes computadoras físicas**
    
    - Cuando dos computadoras interactúan, cada una tiene su propio entorno de seguridad.
        
    - Compartir datos o ejecutar código remotamente introduce riesgos y requiere mecanismos como autenticación o cifrado.
        
4. **Máquinas virtuales (VMs)**
    
    - Un host que ejecuta múltiples máquinas virtuales tiene límites de confianza entre cada VM.
        
    - Una vulnerabilidad en la capa de virtualización podría permitir que una VM maliciosa acceda a otra.
        
5. **Límites organizacionales**
    
    - Diferentes empresas o departamentos dentro de una organización tienen distintos niveles de confianza.
        
    - Un usuario de Recursos Humanos no debería tener acceso directo a los sistemas financieros de la empresa.


### 2. What can go wrong?

Se trata de pensar como un hacker. 
Métodos
1. Mnemonics (STRIDE),
2. trees,
3. libraries of threats.


### **1. STRIDE**

STRIDE es un modelo desarrollado por Microsoft que ayuda a clasificar amenazas en seis categorías principales. No es una clasificación completa, puede haber vulnerabilidades que no se identifiquen.

| **Amenaza**                                             | **Descripción**                                           | **Entidades afectadas**                                |
| ------------------------------------------------------- | --------------------------------------------------------- | ------------------------------------------------------ |
| **S**poofing (Suplantación)                             | Un atacante se hace pasar por otro usuario o sistema.     | Procesos, entidades externas (máquinas, red), personas |
| **T**ampering (Manipulación)                            | Modificar datos de forma no autorizada.                   | Procesos, Flujo de datos, Almacenamiento de datos      |
| **R**epudiation (Repudio)                               | Niega haber realizado una acción sin prueba en su contra. | Procesos, Entidad externa                              |
| **I**nformation Disclosure (Divulgación de información) | Exposición de datos sensibles.                            | Procesos, Flujo de datos, Almacenamiento de datos      |
| **D**enial of Service (Denegación de servicio)          | Sobrecargar un sistema para que deje de funcionar.        | Procesos, Flujo de datos, Almacenamiento de datos      |
| **E**levation of Privilege (Elevación de privilegios)   | Obtener acceso no autorizado a funciones restringidas.    | Procesos                                               |

STRIDE es útil para analizar qué amenazas pueden afectar a cada componente de un sistema.

Puede ser visto como el opuesto a algunas propiedades de seguridad.

Spoofing --> Autentificación
Tampering --> Integridad
Repudio --> No repudio
Information Disclusore --> Confidencialidad
Denial of Service --> Disponibilidad
Elevation of Privilege --> Autorización

---

### **2. Árboles de Amenazas (Threat Trees)**

Los **árboles de amenazas** son diagramas utilizados para visualizar cómo un atacante puede explotar una vulnerabilidad.

- Se comienza con un objetivo de ataque (por ejemplo, "acceder a datos confidenciales").
- Luego se identifican los métodos posibles para lograrlo (ataque por fuerza bruta, inyección SQL, explotación de vulnerabilidades, etc.).
- Cada rama del árbol representa un conjunto de condiciones que deben cumplirse para que el ataque tenga éxito.

---

### **3. Bibliotecas de Amenazas (Threat Libraries)**

Las **bibliotecas de amenazas** son bases de datos de amenazas conocidas que ayudan a identificar riesgos en un sistema.

- Proveen listas de vulnerabilidades organizadas por tipo de sistema (por ejemplo, aplicaciones web, redes, dispositivos IoT).
- Permiten reutilizar el conocimiento de amenazas previas sin empezar desde cero cada vez.
- Ejemplos de fuentes de bibliotecas de amenazas:
    - **CWE (Common Weakness Enumeration)**: Lista de debilidades comunes en software.
    - **CAPEC (Common Attack Pattern Enumeration and Classification)**: Base de datos de patrones de ataque.
    - **MITRE ATT&CK**: Marco que describe tácticas y técnicas usadas por atacantes reales.

Estas bibliotecas permiten comparar un sistema con ataques documentados y mejorar su seguridad basándose en experiencias previas.

---

### **Conclusión**

Para analizar qué puede salir mal en un sistema, es útil:

1. Usar STRIDE para clasificar amenazas.
2. Construir árboles de amenazas para entender cómo un ataque puede ocurrir.
3. Consultar bibliotecas de amenazas para aprender de vulnerabilidades conocidas.

Este enfoque estructurado ayuda a diseñar sistemas más seguros y a anticiparse a posibles ataques.