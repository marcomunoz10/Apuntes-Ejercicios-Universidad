Un **certificado X.509** contiene varios campos clave que lo identifican, como el emisor, el titular, la clave pública y la firma digital. Además, tiene un número de serie único para la identificación y un período de validez para indicar cuándo es válido. Las **extensiones** permiten añadir información adicional, como los usos específicos del certificado. Todo esto garantiza la seguridad y autenticidad en sistemas de criptografía y comunicación.

**Campos del Certificado X.509**

1. **Versión**: v3 (2)  
   Se refiere a la versión del formato de certificado X.509. La versión 3 es la más común y permite el uso de **extensiones** adicionales.

2. **Número de serie**: Distingue a cada certificado emitido por una misma Autoridad de Certificación (**CA**).

3. **Emisor (Issuer)**:  
   Es el nombre distinguido de X.500 (DN) de la **Autoridad de Certificación (CA)** que emite el certificado. Este campo indica quién ha firmado y emitido el certificado.
   
El número de serie, combinado con el nombre del emisor, forma un **ID único** para el certificado.

4. **Periodo de validez**:  
   Incluye las fechas de **inicio** y **fin** del período en el que el certificado es válido. Fuera de este período, el certificado no debe ser considerado válido.

5. **Sujeto (Subject)**:  
   Es el nombre distinguido de X.500 (DN) del **titular del certificado**. Puede ser una persona, una organización o un dispositivo al que se le ha emitido el certificado.

6. **Clave pública del sujeto**:  
   La **clave pública** del sujeto que se encuentra en el certificado. Esta clave se utiliza para cifrar datos o verificar firmas digitales.

7. **Extensiones**:  
   Son campos adicionales que proporcionan información extra sobre el certificado.
   - **Extensiones críticas vs. no críticas**:
    
    - Las **extensiones no críticas** pueden ser ignoradas si no son reconocidas.
        
    - Las **extensiones críticas** deben ser reconocidas; de lo contrario, el certificado debe ser rechazado.
        
- **Restricciones básicas**:
		El sujeto no siempre es el CA.
		La profundidad máxima del camino es el nº de certificados intermedios.
    
- **Otras**: uso de clave, nombre alternativo del sujeto, restricciones de nombre, etc.

8. **Firma digital**:  
   Es la **firma digital** de la **CA** sobre el certificado, garantizando la autenticidad e integridad del certificado. La firma asegura que el certificado no ha sido alterado y que proviene de la fuente de confianza.

