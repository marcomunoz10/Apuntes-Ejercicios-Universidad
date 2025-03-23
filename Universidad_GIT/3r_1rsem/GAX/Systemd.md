
**`systemd`** es un **sistema de inicio (init system)** y un conjunto de herramientas básicas para la administración de un sistema Linux. Es responsable de iniciar, gestionar y supervisar todos los procesos del sistema una vez que el kernel ha arrancado. Es una de las tecnologías más importantes en muchas distribuciones de Linux modernas, como Ubuntu, Debian, Fedora, y muchas otras.

Antes de `systemd`, los sistemas Linux usaban otros sistemas de inicio, como **SysVinit** o **Upstart**, que eran menos flexibles y más difíciles de mantener.

#### Características principales de `systemd`:

1. **Gestión de servicios**: Uno de los componentes clave de `systemd` es su capacidad para gestionar servicios. Un servicio puede ser cualquier programa que se ejecute en segundo plano, como un servidor web (Apache, Nginx), un servidor de bases de datos, o un demonio de red. Estos servicios se gestionan mediante comandos como `systemctl`, que permite iniciar, detener, recargar, y ver el estado de cualquier servicio.
    
    Ejemplo: Iniciar el servidor web Apache:
    
    bash
    
    Copiar código
    
    `systemctl start apache2`
    
2. **Arranque paralelo**: A diferencia de los sistemas de inicio más antiguos que ejecutaban procesos secuencialmente, `systemd` permite iniciar servicios en paralelo, lo que acelera el tiempo de arranque del sistema.
    
3. **Unidades (`unit files`)**: `systemd` organiza todo mediante **unidades**. Una unidad es un archivo de configuración que define un servicio, un dispositivo, un sistema de montaje, etc. Las unidades más comunes son:
    
    - **.service**: Define un servicio (como `apache2.service`).
    - **.mount**: Define un punto de montaje.
    - **.timer**: Define tareas programadas (como un `cron job`).
    - **.network**: Define configuraciones de red.
4. **Dependencias y objetivos**: `systemd` entiende las relaciones de dependencia entre diferentes servicios y las maneja de manera eficiente. Además, usa "objetivos" (targets) en lugar de los niveles de ejecución tradicionales. Un objetivo agrupa varios servicios, como `multi-user.target` o `graphical.target`.
    
5. **Registro centralizado con `journalctl`**: `systemd` proporciona un sistema de registros centralizado llamado **`journald`**, que recopila y almacena los mensajes de registro (logs) del sistema y los servicios. Puedes consultar estos registros con el comando `journalctl`.
    
    Ejemplo: Ver los logs recientes de un servicio:
    
    bash
    
    Copiar código
    
    `journalctl -u apache2`
    

### Ventajas de `systemd`:

- **Consistencia**: Usa una única herramienta (`systemctl`) para gestionar todos los servicios y unidades del sistema.
- **Eficiencia**: Acelera el arranque y la gestión de procesos.
- **Registro avanzado**: Los logs detallados ayudan en el diagnóstico y la depuración de problemas del sistema.


A continuación, se presentan comandos y funcionalidades avanzadas de systemd:
[[Enmascarar y desenmascarar un servicio]]
[[Target]]
[[Análisis tiempo de arranque]]
[[Unidad transitoria]]
[[Crear y ejecutar un servicio]]
[[cgroup]]
[[journalctl]]
[[slices units]]
