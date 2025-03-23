Las **VLANs** (Redes de Área Local Virtuales, del inglés **Virtual Local Area Networks**) son una tecnología utilizada para segmentar redes de manera lógica, permitiendo que múltiples redes virtuales existan en una única infraestructura de red física. Es una forma de **crear redes separadas lógicamente**, aunque todas compartan el mismo hardware, como switches y routers. 

### ¿Por qué usar VLANs?

En redes tradicionales, todas las máquinas conectadas a un mismo [[switch]] comparten una red. Esto puede llevar a ciertos problemas:
- **Congestión de red**: Todas las máquinas comparten el mismo dominio de broadcast, lo que significa que cuando una máquina envía un mensaje de difusión, todas las demás lo reciben, incluso si no es necesario.
- **Seguridad**: Sin VLANs, todas las máquinas están en la misma red, lo que puede aumentar el riesgo de ataques internos o acceso no autorizado.
- **Escalabilidad limitada**: Sin VLANs, las grandes redes pueden volverse difíciles de gestionar y propensas a errores.

Las VLANs permiten **resolver estos problemas** al crear subredes virtuales dentro de una infraestructura de red física compartida. Cada VLAN actúa como si fuera una red independiente.

### Ventajas de las VLANs:

1. **Segregación del tráfico**: Las VLANs permiten separar el tráfico de red de diferentes usuarios, departamentos o tipos de tráfico (por ejemplo, datos, voz, videoconferencia) en diferentes subredes virtuales.
   
2. **Aumento de la seguridad**: El tráfico entre diferentes VLANs está aislado por defecto, lo que mejora la seguridad. Las máquinas en una VLAN no pueden comunicarse directamente con las de otra VLAN a menos que un router o un firewall lo permita.

3. **Reducción del dominio de broadcast**: Al dividir la red en varias VLANs, se reduce el tamaño del dominio de broadcast. Cada VLAN tiene su propio dominio, lo que mejora el rendimiento general de la red.

4. **Flexibilidad y escalabilidad**: Las VLANs permiten reorganizar la red sin cambiar la infraestructura física. Las máquinas pueden moverse entre VLANs lógicamente, sin necesidad de cambiar su conexión física.

### ¿Cómo funcionan las VLANs?

1. **Etiquetado de VLAN (VLAN Tagging)**: En muchas implementaciones, cuando un dispositivo envía tráfico en una red VLAN, el switch etiqueta los paquetes con un identificador VLAN (normalmente llamado **VLAN ID**) para asegurarse de que el tráfico se mantenga dentro de esa VLAN. Este etiquetado se realiza de acuerdo con el estándar **IEEE 802.1Q**.
   
2. **Switches gestionados**: Para implementar VLANs, necesitas un switch gestionado que permita configurar diferentes VLANs. Los switches gestionados permiten asignar ciertos puertos a determinadas VLANs y etiquetar el tráfico para asegurarse de que se mantenga dentro de su VLAN correspondiente.

3. **Puertos de acceso y troncales**:
   - **Puerto de acceso (Access Port)**: Un puerto de acceso en un switch está asociado a una VLAN específica. Todo el tráfico que pasa por este puerto está etiquetado o pertenece a esa VLAN.
   - **Puerto troncal (Trunk Port)**: Los puertos troncales permiten que el tráfico de múltiples VLANs viaje entre switches o entre un switch y un router. Los puertos troncales utilizan etiquetado VLAN (IEEE 802.1Q) para distinguir el tráfico de diferentes VLANs.

### Ejemplo de uso de VLANs:

Imagina una empresa con tres departamentos: **Finanzas**, **Recursos Humanos (RRHH)** y **TI**. La empresa quiere que los empleados de cada departamento estén en redes separadas por razones de seguridad, pero no quiere comprar switches separados para cada departamento. Con VLANs, la empresa puede hacer lo siguiente:

1. **VLAN 10**: Departamento de **Finanzas**.
2. **VLAN 20**: Departamento de **Recursos Humanos**.
3. **VLAN 30**: Departamento de **TI**.

Aunque todas las computadoras de los tres departamentos están conectadas al mismo switch físico, el tráfico de cada VLAN está separado. Esto significa que los empleados de Finanzas no pueden ver el tráfico de RRHH o TI, y viceversa. Si es necesario, un router (o un switch de capa 3) puede permitir la comunicación entre estas VLANs, pero con reglas y políticas de seguridad.

### Tipos de VLANs:

1. **VLAN de datos**: Es el tipo de VLAN más común y se utiliza para segmentar el tráfico de datos estándar entre los usuarios.

2. **VLAN de gestión**: Se utiliza para administrar dispositivos de red, como switches o routers, separando el tráfico de gestión del tráfico de datos de los usuarios para aumentar la seguridad.

3. **VLAN de voz**: Se utiliza para el tráfico de voz sobre IP (VoIP), separando el tráfico de voz del tráfico de datos para garantizar una calidad de servicio adecuada.

4. **VLAN nativa**: En los puertos troncales, la VLAN nativa es la que no está etiquetada (untagged) por el protocolo 802.1Q. El tráfico sin etiqueta en un enlace troncal se asume que pertenece a la VLAN nativa.

### Ejemplo de configuración con `systemd-networkd`:

Si estás utilizando [[systemd-network]] para gestionar tu red y quieres crear una VLAN, necesitas definir un archivo de configuración `.netdev` para la VLAN.

Ejemplo de un archivo `/etc/systemd/network/10-vlan.netdev` que crea una VLAN con el ID `100` en la interfaz `eth0`:

```ini
[NetDev]
Name=vlan100
Kind=vlan

[VLAN]
Id=100
```


- **[NetDev]**: Indica que estamos definiendo un dispositivo de red (en este caso, una VLAN).
- **Name=vlan10**: Nombra la interfaz VLAN que se creará como `vlan10`.
- **Kind=vlan**: Especifica que el tipo de dispositivo es una VLAN.
- **[VLAN]**: Sección donde se define el ID de la VLAN.
- **Id=10**: Define el ID de la VLAN como `10`.

Este archivo crea una VLAN llamada `vlan100` en la interfaz `eth0` con el ID `100`. Luego puedes usar esa interfaz VLAN en un archivo `.network` para asignarle una dirección IP, configurar un gateway, etc.
Ahora necesitas asignar una dirección IP a la VLAN `vlan10`. Crearás otro archivo para esta configuración, llamémoslo `20-vlan10.network`.

**Contenido de `20-vlan10.network`**:

Archivo `/etc/systemd/network/20-vlan.network`:

```ini
[Match]
Name=vlan100

[Network]
Address=192.168.100.10/24
Gateway=192.168.100.1
DNS=8.8.8.8
```

Este archivo asigna una dirección IP estática a la VLAN `vlan100`.

### Conclusión:

Las **VLANs** son una poderosa herramienta para segmentar y gestionar redes en entornos empresariales y de centros de datos, proporcionando **seguridad, escalabilidad y flexibilidad**. Con **VLANs**, puedes hacer que varias redes virtuales existan dentro de la misma infraestructura física, lo que te permite mejorar la eficiencia de la red sin necesidad de hardware adicional.