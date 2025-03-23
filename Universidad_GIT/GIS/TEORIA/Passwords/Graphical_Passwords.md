Los esquemas de contraseñas gráficas pueden clasificarse en tres categorías principales:

- Sistemas basados en recuerdo
- Sistemas basados en reconocimiento
- Sistemas basados en recuerdo guiado

### **Recall-Based systems**

El usuario recuerda y señala un patrón secreto (un dibujo). DAS --> Draw a Secret Scheme. La codificación es la secuencia de coordenadas de las celdas de cuadrículas atravesadas en el dibujo.

**Aspectos a tener en cuenta**:
* El sistema es susceptible a **shoulder surfing**.
* Los usuarios tienden a dibujar imágenes simétricas y utilizan pocos trazos, lo que limita las posibles contraseñas que realmente crean.
* Además, hay un fenómeno llamado **mapeo de muchos a uno**. Esto significa que varios dibujos hechos por diferentes usuarios pueden terminar representando la misma contraseña codificada en el sistema. Este mapeo reduce el número de contraseñas únicas que realmente existen, a pesar de que la teoría parece sugerir que hay muchas más contraseñas posibles.

### **Recognition-Based systems**

Los usuarios deben recordar un portfolio de imágenes durante la creación de la contraseña y tienen que reconocer las imágenes correctas de un conjunto de contraseñas.

La **seguridad** de estos sistemas es **baja** porque las contraseñas que se generan a partir de imágenes tienen un número limitado de combinaciones, lo que las hace más fáciles de adivinar en comparación con contraseñas tradicionales.

Caso de uso: http://passfaces.com/

```
For n = 4 rounds of M = 9 images per panel, with one
image per panel from the user portfolio, the theoretical password space has cardinality M^n = 9^4 = 6561.
```


### **Cued-Recall systems**

Los **sistemas de recuerdo con pistas** requieren que los usuarios recuerden y seleccionen ubicaciones específicas dentro de una imagen. Estos sistemas son más fáciles que el recuerdo puro porque ayudan a los usuarios con pistas visuales, reduciendo la carga de memoria. Se les conoce también como **contraseñas gráficas basadas en clics**.
