![[Pasted image 20250220235212.png]]


La imagen muestra una explicación sobre la **probabilidad de adivinar una contraseña** en un ataque de fuerza bruta.

### **Conceptos clave**

$$ R=b^n $$


R es el tamaño del espacio de contraseñas, donde:
    - b es el número de posibles caracteres en cada posición de la contraseña.
    - n es la longitud de la contraseña.
- **G**: Número de intentos que el atacante puede hacer por unidad de tiempo.
- **T**: Tiempo total disponible para el ataque.


La **b** representa el número de posibles caracteres que se pueden usar en cada posición de la contraseña.

Por ejemplo:

- Si la contraseña usa **solo números (0-9)** → b=10b = 10b=10 (porque hay 10 opciones por cada posición).
- Si usa **solo letras minúsculas (a-z)** → b=26b = 26b=26.
- Si usa **mayúsculas y minúsculas (A-Z, a-z)** → b=52b = 52b=52.
- Si incluye **números, mayúsculas, minúsculas y símbolos** → b puede ser **más de 90**, dependiendo del conjunto de caracteres permitidos.

Ejemplo:  
Si una contraseña tiene **4 caracteres** y solo usa números (10b=10):
$$
R=10^4 = 10000$$
### **Fórmula para la probabilidad de éxito q**

$$ q= \frac{G T}{R} $$


- Si $$GT \leq R$$, el atacante hace **menos intentos** que el número total de combinaciones posibles.
- Se asume que todas las contraseñas son igualmente probables, lo que hace que este cálculo se aplique a ataques de **fuerza bruta**.

En resumen, la probabilidad de éxito aumenta si el atacante puede hacer más intentos por segundo (G) o si tiene más tiempo (T). Para reducir el riesgo, se recomienda **usar contraseñas más largas** (n grande) y con más variedad de caracteres (b grande).