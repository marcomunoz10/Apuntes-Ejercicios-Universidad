Una **unidad transitoria** en el contexto de **systemd** es un tipo de unidad que se crea y gestiona temporalmente en el sistema. A diferencia de las unidades persistentes, que se definen en archivos de configuración y permanecen disponibles incluso después de un reinicio, las unidades transitorias solo existen mientras se ejecutan y no se almacenan en el sistema de archivos.

```bash
systemd-run --unit=mi-tarea.timer /usr/bin/some-script.sh
```
* `systemd-run` para [[Crear y ejecutar un servicio]]
- **`--unit=mi-tarea.timer`**: Estás creando una unidad llamada `mi-tarea.timer`.
- **`/usr/bin/some-script.sh`**: Este es el script que deseas ejecutar. Al ejecutarse, systemd gestionará este script como una unidad transitoria.

### Uso Práctico

Las unidades transitorias son especialmente útiles para tareas como:

- Ejecutar scripts de mantenimiento o de un solo uso.
- Pruebas y desarrollo, donde no necesitas que los servicios persistan.
- Ejecución de comandos temporales que no requieran un arranque automático en el sistema.