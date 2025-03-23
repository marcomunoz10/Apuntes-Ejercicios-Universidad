**`systemd-networkd`** es un servicio incluido en el ecosistema de [[Systemd]] que se encarga de gestionar las interfaces de red en un sistema Linux. Es un **gestor de redes** que puede configurar las interfaces de red, establecer direcciones IP, rutas, y realizar muchas tareas relacionadas con la gestión de red.

#### ¿Qué hace `systemd-networkd`?

1. **Gestiona interfaces de red**: `systemd-networkd` gestiona interfaces de red como Ethernet, Wi-Fi, tuneles VPN, y otras interfaces virtuales. Puede activar y desactivar estas interfaces según sea necesario y aplicarles configuraciones específicas.
    
2. **Configuración automática**: Puede detectar interfaces de red automáticamente y aplicar configuraciones predefinidas cuando una interfaz se conecta. Esto lo hace ideal para entornos dinámicos, como servidores o máquinas virtuales que pueden tener diferentes interfaces o configuraciones de red.
    
3. **Soporte para redes complejas**: `systemd-networkd` soporta configuraciones avanzadas de red como:
    
    - **Redes estáticas y dinámicas**: Puede asignar direcciones IP estáticas o dinámicas (a través de DHCP).
    - **VLANs**: Gestión de redes [[VLAN]] (Virtual Local Area Networks).
    - **Bonding y Bridging**: Configuración de interfaces en modo "bonding" (agrupación de interfaces para redundancia o rendimiento) o "bridging" (puentes de red).
    - **Túneles**: Soporte para túneles como GRE, sit, o tun/tap.
4. **Integración con `systemd-resolved`**: Puede trabajar junto con **`systemd-resolved`**, que es un servicio responsable de resolver nombres de dominio (DNS), lo que permite una gestión integrada de redes y resolución de nombres.
    
5. **Soporte de red para contenedores y virtualización**: `systemd-networkd` se utiliza comúnmente en entornos de virtualización y contenedores donde las redes pueden ser dinámicas y donde se pueden crear y destruir interfaces de red rápidamente.
    

#### Configuración de `systemd-networkd`:

Las configuraciones de red que `systemd-networkd` utiliza están ubicadas en **`/etc/systemd/network/`** y se gestionan a través de archivos de configuración `.network` y `.netdev`.

Ejemplo básico de archivo `.network` para configurar una dirección IP estática en una interfaz `eth0`:

ini

Copiar código

`[Match] Name=eth0  [Network] Address=192.168.1.100/24 Gateway=192.168.1.1 DNS=8.8.8.8`

En este archivo:

- **[Match]**: Define a qué interfaz aplicar esta configuración (en este caso, `eth0`).
- **[Network]**: Define las configuraciones de red, como la dirección IP, la puerta de enlace (gateway) y el servidor DNS.

#### Comandos útiles de `systemd-networkd`:

- **

- **Ver el estado de `systemd-networkd`**: Para ver si el servicio está activo y en ejecución:
    
    bash
    
    Copiar código
    
    `systemctl status systemd-networkd`
    
- **Reiniciar `systemd-networkd`**: Si cambias la configuración de red y deseas aplicar los cambios:
    
    bash
    
    Copiar código
    
    `systemctl restart systemd-networkd`
    
- **Ver la configuración de red actual**: Para obtener un resumen de las configuraciones de red gestionadas por `systemd-networkd`:
    
    bash
    
    Copiar código
    
    `networkctl status`
    
- **Ver detalles de una interfaz específica**: Si deseas ver información más detallada sobre una interfaz en particular, como `eth0`:
    
    bash
    
    Copiar código
    
    `networkctl status eth0`
    

### Ventajas de `systemd-networkd`:

1. **Integración completa con `systemd`**: Como forma parte de `systemd`, se gestiona de la misma manera que los demás servicios, usando herramientas estándar como `systemctl` y `journalctl`.
    
2. **Redes dinámicas y automatizadas**: Es excelente para entornos en los que las configuraciones de red cambian frecuentemente o en máquinas virtuales y contenedores.
    
3. **Soporte para configuraciones avanzadas**: Puedes configurar redes complejas sin necesidad de herramientas externas.
    
4. **Menos dependencia de servicios externos**: `systemd-networkd` es ligero y no requiere de otros demonios o herramientas, lo que lo hace una opción popular en servidores y entornos de virtualización.