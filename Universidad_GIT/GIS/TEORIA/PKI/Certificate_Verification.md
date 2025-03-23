
1. **Anclas de confianza (Trust anchors)**:  
   Son las **entidades raíz** o **autoridades de certificación (CAs)** en las que confía un sistema o aplicación para verificar la validez de un certificado. Estas entidades raíz tienen certificados auto-firmados (self-signed certificates) que se consideran confiables, ya que la confianza se establece desde el principio. Si un certificado tiene una cadena de confianza que llega a una de estas **anclas de confianza**, se considera válido.

2. **Certificados auto-firmados (Self-signed certificates)**:  
   Son certificados que han sido firmados por la misma entidad que los emite, es decir, el **emisor** y el **sujeto** del certificado son la misma persona o entidad. Aunque estos certificados pueden ser válidos en términos técnicos, **no se consideran confiables por defecto** en muchas aplicaciones, ya que no tienen una autoridad de certificación externa que valide su autenticidad. Sin embargo, pueden ser útiles para pruebas internas o en sistemas cerrados donde se confíe en el emisor.

### Resumen:
- **Anclas de confianza** son las **CAs** de confianza desde el principio para verificar certificados.
- **Certificados auto-firmados** son firmados por la misma entidad que los emite y suelen no ser confiables en sistemas que no reconocen a la entidad.