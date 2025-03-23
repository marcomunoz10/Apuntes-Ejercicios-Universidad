El **hidden** (o **capa oculta**) en una red neuronal es la capa intermedia entre la **entrada (input)** y la **salida (output)**. Su función principal es **aprender patrones y características de los datos de entrada** para mejorar la predicción.

---

### 🔹 **Ejemplo en el Reconocimiento de Dígitos**

Sigamos con el ejemplo de la red neuronal que reconoce números del 0 al 9.

#### **Estructura de la red**

- **Capa de entrada (input):**  
    🔹 Representa los píxeles de la imagen (ej. una imagen de 28x28 píxeles tiene **784** neuronas de entrada).
    
- **Capa oculta (hidden):**  
    🔹 Contiene neuronas que procesan la información y aprenden características del número.  
    🔹 En la imagen que compartiste, parece haber **135 neuronas en la capa oculta**.
    
- **Capa de salida (output):**  
    🔹 Tiene **10 neuronas**, una por cada número del 0 al 9.
    

---

### 🔹 **¿Cómo funciona la capa oculta?**

1. **La imagen (input) pasa por la capa oculta.**
    
    - Cada **neurona en la capa oculta** recibe señales de todas las neuronas de entrada.
    - Se aplican **pesos (weights) y bias (bias_h)** para modificar los valores.
    - Luego, se usa una **función de activación** (ej. ReLU, Sigmoid, etc.) para generar una nueva representación de los datos.
2. **La salida de la capa oculta se envía a la capa de salida.**
    
    - La información procesada se convierte en probabilidades para determinar cuál número es el más probable.

---

### 🔹 **Ejemplo Numérico**

Supongamos que tenemos:

- Imagen de un **"3"** como entrada.
- La capa oculta tiene **135 neuronas**.
- La red calcula los valores ocultos antes de pasar a la salida:

```
hidden = [0.2, 0.8, 0.1, ..., 0.6]  # 135 valores
```

Estos valores son una **representación abstracta** de la imagen. La red ha aprendido que ciertas combinaciones de valores en la capa oculta corresponden a un "3".

Después, estos valores se envían a la capa de salida para calcular la predicción final.

---

### 🔹 **¿Cómo se corrige la capa oculta con el backpropagation?**

Si la predicción es incorrecta, la red ajusta sus pesos y **bias_h** en la capa oculta para mejorar en el siguiente intento.

El ajuste se hace con:

ΔWeightsinput−hidden=−η×Error×Derivada de activacioˊn\Delta Weights_{input-hidden} = -\eta \times Error \times \text{Derivada de activación} ΔBiash=−η×Error\Delta Bias_h = -\eta \times Error

Esto permite que la red aprenda mejor **cómo transformar la imagen en una salida correcta**.

---

### 🔹 **Conclusión**

La **capa oculta** es el corazón del aprendizaje de una red neuronal. Es donde la red encuentra **patrones y características** que ayudan a identificar correctamente los números, letras, imágenes, etc.

Si no existiera una capa oculta, la red solo podría aprender relaciones muy simples, pero con una **hidden layer** es capaz de generalizar mejor la información.

¿Te queda más claro? 😃