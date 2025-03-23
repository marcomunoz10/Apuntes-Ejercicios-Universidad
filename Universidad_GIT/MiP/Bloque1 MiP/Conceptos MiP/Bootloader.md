El **Bootloader** (cargador de arranque) es un software esencial que se ejecuta **antes del sistema operativo** y se encarga de **iniciar y cargar el SO en la memoria**.

Cuando enciendes un dispositivo, el Bootloader verifica el hardware, carga el kernel del sistema operativo y le da el control del equipo. Sin él, el sistema no podría arrancar.

---

### 🚀 **¿Cómo funciona el Bootloader?**

1️⃣ **Energía encendida** → El procesador ejecuta el código de arranque desde la memoria ROM (normalmente BIOS o UEFI en PCs).  
2️⃣ **Autoprueba (POST)** → Se revisa el hardware (RAM, CPU, disco).  
3️⃣ **Carga del Bootloader** → Se ejecuta desde la memoria de arranque.  
4️⃣ **Selección del sistema operativo** → Si hay varios SO, permite elegir uno (como en un arranque dual).  
5️⃣ **Carga del kernel** → Se inicializa el núcleo del sistema operativo y comienza la ejecución normal.