Para **"set the k-th bit"** (establecer o poner el k-ésimo bit a 1) en un número en C++, necesitas realizar una operación a nivel de bits. Esto se hace usando el operador **OR bit a bit (`|`)**. Aquí te explico el proceso y te proporciono un código en C++.

### Explicación del proceso:

1. **Crear una máscara** con un `1` en la posición k, que será el bit que deseas establecer a `1`.
2. **Aplicar una OR bit a bit** entre el número original y la máscara. Esto asegurará que el bit en la posición k sea `1`, sin alterar los otros bits del número.

El enfoque es simple:
- `1 << k`: Desplaza el número `1` k posiciones a la izquierda, creando un número con un único `1` en la posición k y `0`s en los demás bits.
- `n | (1 << k)`: Aplica una OR bit a bit entre el número `n` y la máscara creada, lo que fuerza el bit en la posición k a `1`, y deja los demás bits sin cambios.

### Código en C++

Aquí tienes un ejemplo de código en C++ que realiza esta operación:

```cpp
#include <iostream>
using namespace std;

int setKthBit(int n, int k) {
    return n | (1 << k);
}

int main() {
    int n = 5;  // Número original (en binario: 101)
    int k = 1;  // Queremos establecer el bit en la posición 1 (contando desde 0)
    
    cout << "Número original: " << n << " (binario: " << bitset<8>(n) << ")" << endl;
    
    // Llamada para establecer el k-ésimo bit
    int newNumber = setKthBit(n, k);
    
    cout << "Nuevo número: " << newNumber << " (binario: " << bitset<8>(newNumber) << ")" << endl;
    
    return 0;
}
```

### Explicación del código:

1. **Función `setKthBit`**:
   - Recibe un número `n` y un índice de bit `k`.
   - Devuelve el resultado de aplicar la operación `n | (1 << k)` que establece el k-ésimo bit a 1.

2. **`main`**:
   - Se define un número `n` (por ejemplo, 5, que es `101` en binario).
   - Se quiere establecer el bit en la posición 1, lo que debería cambiar el número a `111` en binario (es decir, 7 en decimal).
   - Se usa `bitset<8>` para mostrar el valor del número en formato binario de 8 bits.

### Ejemplo de salida:

```
Número original: 5 (binario: 00000101)
Nuevo número: 7 (binario: 00000111)
```

En este ejemplo, el número original era `5` (101 en binario) y al establecer el bit en la posición 1, obtenemos el número `7` (111 en binario).