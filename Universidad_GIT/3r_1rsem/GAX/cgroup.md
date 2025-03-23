Un **control group**, comúnmente conocido como **cgroup**, es una característica del núcleo de Linux que permite la limitación, el monitoreo y el aislamiento de recursos de un grupo de procesos. Los cgroups son una parte fundamental del sistema de gestión de recursos en Linux, y son ampliamente utilizados en contenedores y entornos de virtualización.

### Características Principales de los cgroups

1. **Limitación de Recursos**:
   - Permiten establecer límites en el uso de recursos como CPU, memoria, disco y red para grupos específicos de procesos. Por ejemplo, se puede limitar el uso de memoria a 512 MB para un grupo de procesos.

2. **Monitoreo de Recursos**:
   - Facilitan la recopilación de estadísticas sobre el uso de recursos por parte de los procesos en un cgroup. Esto incluye información sobre el uso de CPU, memoria, I/O, etc.

3. **Aislamiento**:
   - Aislando los recursos entre diferentes grupos de procesos, se pueden evitar interferencias entre aplicaciones que compiten por los mismos recursos del sistema.

4. **Jerarquía**:
   - Los cgroups están organizados en una jerarquía. Un cgroup puede contener otros cgroups, lo que permite estructurar de manera organizada los recursos y las configuraciones.

5. **Integración con Systemd**:
   - En sistemas que utilizan **systemd**, los cgroups son utilizados para gestionar las unidades de servicio, lo que permite un control granular sobre cómo los servicios consumen recursos.

Comandos:

1. Visualiza y gestiona los cgroups:
```bash
systemd-cgls   
```
2. Monitoriza el uso de recursos a tiempo real:
```bash
systemd-cgtop
```
