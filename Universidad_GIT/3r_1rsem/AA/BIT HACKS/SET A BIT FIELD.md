#### **Problema:**
El objetivo es establecer un campo de bits en una variable (denominada `x`) a un valor específico `y`


#### **Idea:**
1. Se usa una **máscara (mask)** para limpiar (poner a 0) los bits específicos en `x` que queremos modificar.
2. Luego, **desplazamos** el valor `y` hacia la posición correcta dentro del campo de bits y lo **combinamos** con la palabra original `x`.

#### **Fórmula:**
```c
x = (x & ~mask) | (y << shift);
```

- **`x & ~mask`**: Aquí se usa la negación de la máscara (`~mask`) para poner a 0 los bits que queremos modificar en `x`, y se conserva el resto de los bits.
- **`y << shift`**: Desplazamos el valor `y` hacia la izquierda (usando el operador de desplazamiento `<<`) para que coincida con la posición donde queremos insertar el valor.
- **`|`**: Finalmente, combinamos los bits modificados de `x` y el valor desplazado de `y` usando un OR bit a bit (`|`).

#### **Ejemplo:**
- `shift = 7`: Significa que el valor de `y` se desplazará 7 posiciones hacia la izquierda.
- El valor inicial de `x` es `1011110101101101` (binario).
- El valor de `y` es `0000000000000011` (queremos establecer estos bits).
- La **máscara** es `0000001111000000`, que indica los bits a modificar en `x`.
  
Pasos:
1. **`x & ~mask`**: Limpia los bits seleccionados por la máscara en `x`. El resultado es `1011110001101101`.
2. **`y << shift`**: Desplaza el valor de `y` 7 posiciones a la izquierda, dando `0000001100000000`.
3. **Combinación**: Al aplicar el OR (`|`), obtenemos el resultado final `1011111001101101`.

### Código en C:

Aquí tienes el código en C para realizar esta operación:

```c
#include <stdio.h>

unsigned int setBitField(unsigned int x, unsigned int y, unsigned int mask, int shift) {
    // Limpiamos el campo de bits usando la máscara e insertamos el nuevo valor desplazado
    return (x & ~mask) | (y << shift);
}

int main() {
    unsigned int x = 0b1011110101101101;  // Valor inicial de x
    unsigned int y = 0b0000000000000011;  // Valor que queremos insertar
    unsigned int mask = 0b0000001111000000;  // Máscara que define los bits a modificar
    int shift = 7;  // Número de posiciones para desplazar el valor de y

    // Aplicar la operación
    unsigned int result = setBitField(x, y, mask, shift);

    // Imprimir el resultado
    printf("Resultado: %04X\n", result);  // Mostrar en formato hexadecimal
    return 0;
}
```

### Explicación del código:
1. **`setBitField`**: Esta función realiza la operación de establecer un campo de bits utilizando la fórmula mencionada.
2. **Máscara y desplazamiento**: La máscara define qué bits en `x` serán modificados y el desplazamiento especifica en qué posición de bits se debe insertar el valor `y`.
3. **Resultado**: El valor final es impreso en formato hexadecimal usando `%04X`, pero también puedes imprimirlo en binario si lo prefieres.

Este código muestra cómo modificar bits específicos dentro de una palabra utilizando operaciones bit a bit en C.