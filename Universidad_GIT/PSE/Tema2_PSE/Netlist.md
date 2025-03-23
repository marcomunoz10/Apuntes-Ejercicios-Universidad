# Netlist

Un **netlist** (lista de conexiones) es un archivo o conjunto de datos que describe las conexiones eléctricas entre los componentes de un circuito electrónico. Es una representación textual del circuito, generada por herramientas de diseño como los esquemáticos y los programas de diseño de PCB (como KiCad, Altium, Eagle).

### 📌 Contenido de un Netlist:

1. **Componentes**
2. **Nodos o redes**: Conexiones entre pines de los componentes.
3. **Propiedades**

### 🔹 Ejemplo de un Netlist simple:

```
R1  N1 N2  1k
C1  N2 N3  10uF
U1  N1 N3 N4  74LS00
```

Aquí, `R1` es una resistencia de 1kΩ conectada entre `N1` y `N2`, `C1` es un capacitor de 10µF entre `N2` y `N3`, y `U1` es un CI conectado a diferentes nodos.

### 🛠️ ¿Para qué sirve?

- **Simulación de circuitos** en herramientas como SPICE.
- **Diseño de PCB** para traducir esquemáticos a conexiones físicas.
- **Verificación y depuración** de conexiones en un circuito.

