```shell
chown www-data.www-data /var/www/html/test -R
```

El comando `chown` en Linux se utiliza para cambiar el **propietario** y el **grupo** de archivos o directorios.

### Desglose del comando:

1. **`chown`**: El comando para cambiar propietario y grupo.
2. **`www-data.www-data`**:
    - La parte antes del punto (`www-data`) especifica el nuevo propietario.
    - La parte después del punto (`www-data`) especifica el nuevo grupo.
3. **`/var/www/html/test`**: Especifica el archivo o directorio al que se aplicará el cambio.
4. **`-R`**: (Recursivo) Aplica el cambio no solo al directorio especificado, sino también a todos sus subdirectorios y archivos.