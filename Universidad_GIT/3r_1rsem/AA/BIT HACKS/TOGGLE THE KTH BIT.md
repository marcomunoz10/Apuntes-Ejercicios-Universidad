Para realizar el **"toggle the k-th bit"** (alternar el k-ésimo bit) en un número, utilizas una operación a nivel de bits llamada **XOR** (exclusive OR), representada en C++ por el operador `^`. Este operador alterna (invierte) el valor de un bit: si el bit es `0`, lo convierte en `1`, y si es `1`, lo convierte en `0`.

### Explicación del proceso:

1. **Crear una máscara** con un `1` en la posición del bit que deseas alternar.
2. **Aplicar una XOR bit a bit** entre el número original y la máscara. El operador XOR tiene la propiedad de que cuando comparas un bit con `1`, se alterna:
   - `1 ^ 1 = 0`
   - `0 ^ 1 = 1`
   
Por lo tanto, aplicar XOR con `1 << k` alternará el bit en la posición `k` sin afectar los otros bits.

### Pasos:
- **Crear la máscara**: `1 << k` genera un número con un único `1` en la posición `k` y `0`s en todas las demás posiciones.
- **Aplicar XOR**: `n ^ (1 << k)` alterna el bit en la posición `k` en el número `n`.

### Código en C++

Aquí tienes un programa en C++ que alterna el k-ésimo bit de un número:

```cpp
#include <iostream>
#include <bitset>
using namespace std;

// Función para alternar el k-ésimo bit
int toggleKthBit(int n, int k) {
    return n ^ (1 << k);
}

int main() {
    int n = 5;  // Número original (en binario: 101)
    int k = 1;  // Queremos alternar el bit en la posición 1 (contando desde 0)
    
    cout << "Número original: " << n << " (binario: " << bitset<8>(n) << ")" << endl;
    
    // Llamada para alternar el k-ésimo bit
    int newNumber = toggleKthBit(n, k);
    
    cout << "Nuevo número tras alternar el bit: " << newNumber << " (binario: " << bitset<8>(newNumber) << ")" << endl;
    
    return 0;
}

```

# Explicación del código:

1. **Función `toggleKthBit`**:
   - Recibe un número `n` y un índice de bit `k`.
   - Devuelve el resultado de aplicar la operación `n ^ (1 << k)`, lo que alterna el k-ésimo bit.


2. **`main`**:
   - Define un número `n` (por ejemplo, 5, que es `101` en binario).
   - El valor de `k` indica que queremos alternar el bit en la posición `1`.
   - Se usa `bitset<8>` para mostrar el valor del número en formato binario de 8 bits.

### Ejemplo de salida:

```
Número original: 5 (binario: 00000101)
Nuevo número tras alternar el bit: 7 (binario: 00000111)
```

### Explicación del resultado:
- El número original es `5`, que en binario es `101`.
- Alternar el bit en la posición `1` cambia el `0` a `1`, por lo que el número se convierte en `7` (`111` en binario).
  
Si vuelves a aplicar la función a ese número, alternarás de nuevo el bit y regresarás al valor original:

```cpp
int revertedNumber = toggleKthBit(newNumber, k);
cout << "Número tras alternar de nuevo: " << revertedNumber << " (binario: " << bitset<8>(revertedNumber) << ")" << endl;
```

La salida será:

```
Número tras alternar de nuevo: 5 (binario: 00000101)
```

Esto muestra que alternar el mismo bit dos veces restaura el valor original.



