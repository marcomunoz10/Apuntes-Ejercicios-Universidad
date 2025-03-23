La función `createMask` se define de la siguiente manera:

```cpp
unsigned int createMask(int shift, int fieldSize) {     return ((1 << fieldSize) - 1) << shift; }
```


1.  `1 << fieldSize`
- Esta operación toma el número `1` (que es `0b 0000 0001` en binario) y lo desplaza hacia la izquierda `fieldSize` posiciones.
*  Si `fieldsize` es 4, entonces: `1 << 4` --> `0b 0001 0000`

2.  `(1 << fieldSize) - 1`:
* `(1 << fieldsize) - 1` al restar 1 en binario ser verá de esta manera: `0000 0000 0000 1111`. Para aclarar, el primer número en decimal es 16 (`0b 0001 0000`) y al restarle 1 seria 15 (`0b 0000 1111`).

3. `((1 << fieldSize) - 1) << shift`
* Se desplaza `shift` bits a la izquierda.