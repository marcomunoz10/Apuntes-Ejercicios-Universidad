En [[Systemd]] un `target` es una unidad especial que agrupa otros servicios para definir un estado o un nivel de operación de sistema. Sirven para determinar qué estados tienen que estar activos en un determinado punto de tiempo.

### ¿Qué hace un target?
Cuando se activa un target se activan los servicios y unidades que se hayan definido como dependencias de ese target. 
Ejemplo: un sistema quiere estar en modo multiusuario se invoca el `multi-user.target`

### Ejemplos de targets comunes:

1. **`graphical.target`**:
    - Este target se usa para iniciar el sistema en modo gráfico (interfaz de escritorio). Incluye todos los servicios del target `multi-user.target` más aquellos necesarios para iniciar una interfaz gráfica.

2. **`multi-user.target`**:
    - Este target es para un entorno multiusuario sin interfaz gráfica. Activa la mayoría de los servicios del sistema, como la red y otros servicios esenciales, pero sin iniciar un servidor gráfico.

3. **`rescue.target`**:
    - Inicia el sistema en modo de rescate. Se utiliza para tareas de mantenimiento y recuperación del sistema. Solo los servicios mínimos están activos y generalmente tiene solo un terminal disponible.

4. **`emergency.target`**:
    - Inicia el sistema en modo de emergencia, un estado aún más restringido que el modo de rescate. Solo se monta el sistema de archivos raíz en modo lectura y se ofrece una terminal de emergencia para solucionar problemas graves.

1. **`reboot.target`** y **`poweroff.target`**:
    - Estos targets se usan para reiniciar o apagar el sistema respectivamente.

### Comandos

1. **Ver el target actual** 
```bash
systemctl get-default
```

2. **Cambiar el target actual** 
Detiene todos los servicios que **no forman parte del nuevo target** que estás activando, y **solo mantiene o inicia los servicios** que están explícitamente definidos en ese target.
```bash
systemctl isolate nombre-del-target
```

3. **Establecer el target por defecto**
```bash
systemctl set-default nombre-del-target
```
