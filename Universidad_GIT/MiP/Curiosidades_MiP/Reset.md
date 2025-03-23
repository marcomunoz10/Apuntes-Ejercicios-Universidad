
[+] Reset frío: desconectar alimentación.
[+] Reset caliente: pulsar botón reiniciar.

La diferencia clave entre un **reset frío** y un **reset caliente** está en **qué partes del sistema se restablecen** y **qué estados se conservan o se pierden**.

### **Reset frío (Cold Reset)**

- **Cambia**:
    
    - Se apaga completamente el sistema, reiniciando todos los componentes de hardware.
    - Se borra completamente la **RAM**, los registros de la CPU y cachés.
    - Los dispositivos periféricos también se reinician.
    - En algunos sistemas, la configuración de firmware (BIOS/UEFI) puede volver a valores por defecto si estaba corrupta.
- **Conserva**:
    
    - El estado de almacenamiento no volátil (HDD, SSD, EEPROM, Flash).
    - La configuración de firmware si está almacenada de forma persistente.
- **Consecuencia**: Es un reinicio profundo, útil para resolver problemas graves como bloqueos de hardware o corrupción en la memoria.
    

---

### **Reset caliente (Warm Reset)**

- **Cambia**:
    
    - Se reinicia el **procesador** y los registros internos, pero sin cortar la alimentación.
    - Puede reiniciar solo algunas partes del hardware, dependiendo de cómo se implemente.
    - En sistemas embebidos o microcontroladores, puede reiniciar solo el núcleo del procesador sin afectar otros módulos.
- **Conserva**:
    
    - Algunos datos en la RAM pueden **sobrevivir**, dependiendo de la implementación.
    - Los periféricos pueden no reiniciarse por completo (ejemplo: una tarjeta de red puede seguir funcionando).
    - Algunos estados del sistema operativo pueden mantenerse (en ciertos casos).
- **Consecuencia**: Es más rápido que un reset frío, pero si el problema está en el hardware o en una corrupción profunda de la memoria, no siempre lo soluciona.
    

### **Ejemplo práctico**

- Apagar un PC y volverlo a encender → **Reset frío**
- Reiniciar Windows desde el menú o con **Ctrl + Alt + Supr** → **Reset caliente**
- En un microcontrolador, ejecutar un **watchdog reset** para recuperar un fallo sin apagarlo → **Reset caliente**

¿Necesitas más detalles en algún caso específico?