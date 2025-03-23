El comando `ps` muestra información sobre los procesos en ejecución. Aquí tienes algunas de las opciones más comunes que puedes utilizar con `ps`:

1. **-e**: Muestra todos los procesos en el sistema.
2. **-f**: Muestra información completa sobre los procesos, incluyendo el PID, el PPID, el usuario, y más.
3. **-u usuario**: Muestra los procesos de un usuario específico.
4. **-a**: Muestra procesos de todos los usuarios (no solo los del terminal actual).
5. **-x**: Muestra procesos que no tienen un terminal asociado.
6. **-l**: Muestra un formato largo, con más detalles sobre cada proceso.
7. **-p PID**: Muestra información sobre un proceso específico dado su ID.
8. **-o formato**: Personaliza la salida mostrando solo los campos especificados.
9. **--sort=campo**: Ordena la salida por el campo especificado (por ejemplo, `%mem` o `pid`).

Ejemplo de uso:

```bash
ps -ef  # Muestra todos los procesos con información completa
```

Puedes combinar varias opciones para personalizar la salida según tus necesidades.