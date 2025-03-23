PXE (Preboot Execution Environment) es un estándar que permite a una computadora arrancar a través de la red en lugar de usar un disco duro, una unidad USB o un CD/DVD. Se utiliza principalmente en entornos donde se requiere la instalación o mantenimiento remoto de sistemas operativos en múltiples dispositivos.

### **¿Cómo funciona PXE?**

PXE opera a través de una red local (LAN) y requiere de varios componentes clave:

1. **Cliente PXE**: Es la computadora que desea arrancar desde la red. Debe tener una tarjeta de red con soporte PXE.
2. **Servidor DHCP**: Asigna una dirección IP al cliente y le proporciona la ubicación del servidor TFTP desde donde descargará el software de arranque.
3. **Servidor TFTP**: Almacena y proporciona los archivos de arranque necesarios, como un kernel de sistema operativo o un programa de instalación.
4. **Servidor de arranque (Boot Server)**: Puede ser un servidor PXE o un servidor de instalación de sistemas operativos, como Windows Deployment Services (WDS) o un servidor Linux con herramientas como iPXE o Syslinux.

### **Proceso de arranque con PXE**

1. El cliente envía una solicitud DHCP para obtener una dirección IP y los detalles del servidor PXE.
2. El servidor DHCP responde con una dirección IP y la ubicación del servidor TFTP.
3. El cliente descarga el [[UNIVERSIDAD/MiP/Bloque1 MiP/Conceptos MiP/Bootloader|Bootloader]] desde el servidor TFTP.
4. Se carga el sistema operativo o el instalador según la configuración.

### **Ventajas de PXE**

- **Automatización**: Permite la instalación desatendida de sistemas operativos en múltiples máquinas.
- **Sin necesidad de medios físicos**: No requiere USBs o discos para instalar el sistema operativo.
- **Administración centralizada**: Ideal para administradores de sistemas que gestionan múltiples dispositivos.
