journalctl es una herramienta integrada en systemd que muestra los [[logs]] de eventos y 
servicios.

### Ejemplos de Uso


1. **Ver Todos los Registros**:
   ```bash
   journalctl
   ```

2. **Ver Registros de un Servicio Específico**:
   ```bash
   journalctl -u nombre-del-servicio
   ```
   (Reemplaza `nombre-del-servicio` con el nombre del servicio que deseas consultar).

3. **Filtrar por Prioridad**:
   ```bash
   journalctl -p err
   ```
   (Esto mostrará solo los registros de nivel de error o superior).

4. **Monitorear en Tiempo Real**:
   ```bash
   journalctl -f
   ```

5. **Limitar el Número de Líneas Mostradas**:
   ```bash
   journalctl -n 100
   ```
   (Esto mostrará solo las últimas 100 líneas de los registros).

6. **Ver Registros de un Periodo Específico**:
   ```bash
   journalctl --since "2024-09-01" --until "2024-09-28"
   ```

7. **Ver los logs persistentes**:
```bash
journalctl --persistent
```
8. **Limitar el tamaño del journal**:
Limitar el tamaño del journal: Editar /etc/systemd/journald.conf y ajustar parámetros 
como SystemMaxUse, SystemKeepFree, etc.
Vaciar el journal:
```bash
journalctl --vacuum-size=100M
```