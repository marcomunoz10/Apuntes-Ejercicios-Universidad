Para analizar el tiempo de arranque `systemd-analyze`

Para que sea más detallado `systemd-analyze blame`



Si se quiere visualizar gráficamente se tiene que hacer lo siguiente sin ser root.

```console
systemd-analyze plot>boot.svg
firefox boot.svg
```