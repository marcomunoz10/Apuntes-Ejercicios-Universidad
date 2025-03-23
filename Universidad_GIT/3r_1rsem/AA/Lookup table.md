Una **Lookup Table (LUT)** o **tabla de consulta** es una estructura de datos que se utiliza para mapear entradas a salidas de manera eficiente. Su propósito principal es reducir el tiempo de cálculo o procesamiento al almacenar resultados predefinidos para una serie de entradas posibles, en lugar de calcular el resultado cada vez que es necesario. De esta forma, se acelera el acceso a la información o a las respuestas a ciertas operaciones.

### Características principales de una Lookup Table:
1. **Eficiencia**: Una LUT almacena las respuestas a operaciones o cálculos para diferentes entradas. Si se necesita conocer el resultado para una entrada específica, en lugar de realizar el cálculo desde cero, se busca directamente en la tabla, lo que ahorra tiempo.
2. **Memoria**: Almacenar todas las combinaciones posibles de entrada y su correspondiente salida puede requerir más memoria, pero este uso adicional de memoria puede estar justificado si acelera el rendimiento del sistema.
3. **Simplicidad de acceso**: Generalmente, se accede a los valores de una LUT mediante el uso de un índice, lo que hace que el tiempo de acceso sea constante (\(O(1)\)).

### Ejemplos de uso de Lookup Tables:
1. **Gráficos por computadora**: Las LUT se usan para transformar valores de colores o para aplicar correcciones gamma en imágenes.
2. **Criptografía**: En criptografía, las LUT se usan para almacenar valores hash pre-calculados o ciertos resultados de funciones complejas, como S-boxes en algoritmos de cifrado.
3. **Cálculos matemáticos complejos**: En lugar de calcular funciones matemáticas (como senos, cosenos, etc.) en tiempo real, las LUT pueden almacenar los resultados para una gama de valores de entrada y así acceder rápidamente a ellos.
4. **Compresión de datos**: En compresión de imágenes, como JPEG, se utilizan LUT para acelerar la codificación y decodificación.
5. **Controladores y hardware**: Los microcontroladores y sistemas embebidos a menudo usan LUT para ahorrar en tiempo de procesamiento, ya que su poder de cómputo es limitado.

### Ejemplo simple de una Lookup Table
Supongamos que queremos hacer una LUT para una función matemática básica como el seno de un ángulo en grados:

| Ángulo (°) | Seno |
|------------|------|
| 0          | 0    |
| 30         | 0.5  |
| 45         | 0.707|
| 60         | 0.866|
| 90         | 1    |

En lugar de calcular el seno de un ángulo en tiempo real, simplemente buscamos el valor en la tabla.

En resumen, una **Lookup Table** es una herramienta útil para optimizar procesos que implican cálculos repetidos o accesos frecuentes a información predefinida, mejorando así la eficiencia de los sistemas.