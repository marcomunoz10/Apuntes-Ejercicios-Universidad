Un **latch** es un circuito secuencial que **almacena un valor** y lo mantiene hasta que se actualiza con una nueva entrada. En términos de VHDL o hardware digital, un **latch para mantener el valor de `Y`** significa que `Y` retiene su valor anterior si no hay un cambio en la señal de control.

---

### 🔹 **¿Cómo funciona un latch?**

Un latch es **sensible al nivel**, lo que significa que cambia su salida mientras la señal de control (como `Enable`) está activa.

#### ✅ **Ejemplo de un latch en VHDL (Latch tipo D)**

Este código describe un **latch D** que mantiene el valor de `Y` mientras `Enable` está en `0` y lo actualiza cuando `Enable` es `1`.



---

### 🔹 **¿Cuál es la diferencia entre un latch y un flip-flop?**

|Característica|Latch|Flip-Flop|
|---|---|---|
|Sensibilidad|**Nivel** (activo en `1` o `0`)|**Borde** (subida o bajada de un clock)|
|Uso común|Almacenamiento simple|Diseño sincronizado|
|Riesgo|Puede generar problemas de tiempos ("latch transparency")|Más seguro contra glitches|

💡 **En sistemas embebidos y FPGAs, se recomienda usar flip-flops en lugar de latches** para evitar problemas de sincronización.

