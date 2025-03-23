### Objetivo
Extraer un conjunto específico de bits de un número binario (`x`).

### Idea

1. **Aplicar una máscara** que permita aislar los bits que te interesan.
2. **Desplazar esos bits hacia la derecha** para que queden alineados en la posición más baja (bits menos significativos).

### Operación
La fórmula usada es:

```c
(x & mask) >> shift;
```

1. **`x & mask`**: Aplica la operación AND bit a bit entre `x` y una máscara (`mask`). Para dejar solo los bits que interesan.
2. **`>> shift`**: Después de aplicar la máscara, se desplazan los bits seleccionados hacia la derecha el número de posiciones indicado por `shift` para desplazar el conjunto de bits que nos interesa a la derecha del todo.

### Código en C++


```c
#include <iostream>
#include <bitset>

using namespace std;

unsigned int createMask(int shift, int fieldSize) {
    return ((1 << fieldSize) - 1) << shift;
}

unsigned int extractField(unsigned int x, int shift, int fieldSize) {
    unsigned int mask = createMask(shift, fieldSize);
    return (x & mask) >> shift;
}

int main() {
    unsigned int x = 0b1011110101101101;

    int shift = 7; 
    int fieldSize = 4; 

    unsigned int result = extractField(x, shift, fieldSize);
    cout << "Valor original x:" << endl;
    cout << bitset<16>(x) << endl;  
    cout << "Campo extraido (shift=" << shift << ", size=" << fieldSize << "):" << endl;
    cout << bitset<16>(result) << endl;  

    return 0;
}

```

