#ac

#### Cambiar compilador
module add gcc/12.1.1
module load gcc/12.1.1
gcc -v

Características hardware
lscpu

##### Copia todos los archivos de un path en el directorio actual:
cp ../alumnos/LAB1/* . --> el punto quiere decir directorio actual.
para abrir archivo html:
xdg-open Lab1V1.html

para mirar codigo asembly

perf record -e instructions,cycles” y ”perf report”
### 1. **Compilación con diferentes niveles de optimización**

En GCC, las banderas de optimización ajustan cómo se genera el código para mejorar el rendimiento. A continuación se describen las opciones más comunes:

#### -O1 (Optimización básica)
- **Descripción**: Realiza optimizaciones básicas sin aumentar mucho el tiempo de compilación o el tamaño del código generado.
- **Efecto**: Mejora el rendimiento eliminando código innecesario, simplificando expresiones, y ejecutando ciertas optimizaciones sencillas.
- **Uso**: Útil si necesitas un equilibrio entre tiempo de compilación y rendimiento.

#### -O2 (Optimización intermedia)
- **Descripción**: Habilita más optimizaciones que -O1, incluyendo mejoras en el uso de registros, eliminación de código muerto, y más.
- **Efecto**: Aumenta el rendimiento sin sacrificar la estabilidad, y no incrementa significativamente el tamaño del binario.
- **Uso**: Comúnmente usado cuando se quiere un rendimiento moderado sin cambios en la precisión.

#### -O3 (Optimización agresiva)
- **Descripción**: Habilita optimizaciones adicionales que pueden generar código más grande y lento de compilar.
- **Efecto**: Hace más agresiva la optimización, incluyendo expansión de funciones en línea, desenrollado de bucles, y más.
- **Uso**: Ideal para maximizar el rendimiento cuando el tamaño del ejecutable o el tiempo de compilación no son preocupantes.

#### -Ofast (Optimización agresiva con menos precisión)
- **Descripción**: Similar a -O3, pero también desactiva el cumplimiento estricto del estándar IEEE para las operaciones de punto flotante.
- **Efecto**: Mejora el rendimiento eliminando verificaciones de seguridad, lo que puede llevar a ligeros cambios en los resultados de las operaciones en coma flotante.
- **Uso**: Útil en aplicaciones donde la velocidad es crucial y se pueden tolerar pequeños errores en los cálculos de punto flotante.

#### -march=native (Optimización para la arquitectura de la CPU actual)
- **Descripción**: Optimiza el código específicamente para el procesador en el que estás compilando.
- **Efecto**: Utiliza todas las características de tu CPU (como instrucciones SIMD o AVX) para mejorar el rendimiento.
- **Uso**: Muy útil en programas que sólo se ejecutarán en una máquina específica, ya que puede aprovechar al máximo su hardware.

### 2. **Uso de `perf` para analizar el rendimiento**

Una vez que has compilado tu programa con diferentes niveles de optimización, puedes usar `perf` para medir el rendimiento del programa:

- **Comando**: 
  ```bash
  perf stat ./P1
  ```
  Esto te proporcionará estadísticas de rendimiento como el número de ciclos de CPU, instrucciones ejecutadas, fallos de caché, entre otros.

### 3. **Ejemplo de compilación y análisis de rendimiento**

#### Paso 1: Compilar con optimizaciones

Compila el programa con diferentes banderas de optimización:

- Sin optimización:
  ```bash
  gcc P1.c -o P1
  ```

- Optimización básica (-O1):
  ```bash
  gcc P1.c -O1 -o P1_O1
  ```

- Optimización agresiva (-Ofast):
  ```bash
  gcc P1.c -Ofast -o P1_Ofast
  ```

- Optimización específica para tu CPU (-march=native):
  ```bash
  gcc P1.c -Ofast -march=native -o P1_native
  ```

#### Paso 2: Ejecutar `perf` para medir rendimiento

Ejecuta `perf` en cada uno de los binarios generados para ver cómo afecta la optimización al rendimiento:

- Para el binario sin optimización:
  ```bash
  perf stat ./P1
  ```

- Para el binario con `-O1`:
  ```bash
  perf stat ./P1_O1
  ```

- Para el binario con `-Ofast`:
  ```bash
  perf stat ./P1_Ofast
  ```

- Para el binario optimizado para la CPU (`-march=native`):
  ```bash
  perf stat ./P1_native
  ```

### 4. **Interpretación de resultados**

Al comparar las estadísticas de `perf`, podrás ver cómo las diferentes optimizaciones afectan el rendimiento en términos de:

- **Ciclos de CPU**: Menor número de ciclos indica que el programa se ejecuta más eficientemente.
- **Instrucciones**: El número de instrucciones ejecutadas puede variar dependiendo de cómo se optimiza el código.
- **Tiempo total de ejecución**: El objetivo es reducir el tiempo total de ejecución del programa.

### 5. **Conclusión**

- **-O1**: Buena opción si necesitas optimización básica con seguridad.
- **-O3** o **-Ofast**: Son excelentes para obtener el máximo rendimiento, pero pueden generar binarios más grandes o comprometer la precisión de los cálculos.
- **-march=native**: Aprovecha el hardware específico de tu CPU para obtener el mejor rendimiento en esa máquina.

Usar estas opciones junto con `perf` te permite ajustar el rendimiento de tus programas a sus necesidades específicas, equilibrando entre velocidad, precisión y tamaño del ejecutable.