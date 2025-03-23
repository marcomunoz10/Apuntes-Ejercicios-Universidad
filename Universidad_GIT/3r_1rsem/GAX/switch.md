Un **switch** es un dispositivo de red fundamental que se utiliza para conectar múltiples dispositivos dentro de una red local (LAN). Su función principal es recibir, procesar y enviar datos a dispositivos específicos dentro de la red, en lugar de enviar datos a todos los dispositivos, como lo haría un hub.

### Características principales de un switch:

1. **Conexión de dispositivos**:
   - Un switch permite conectar varios dispositivos, como computadoras, impresoras, servidores y otros switches, utilizando cables Ethernet. Cada dispositivo se conecta a un puerto en el switch.

2. **Filtrado de tráfico**:
   - A diferencia de un hub, que transmite datos a todos los puertos, un switch filtra el tráfico de red. Analiza la dirección MAC (Media Access Control) de los dispositivos conectados y envía datos solo al dispositivo de destino. Esto mejora la eficiencia de la red al reducir la congestión y el tráfico innecesario.

3. **Aprendizaje de direcciones**:
   - Cuando un switch recibe un paquete de datos, registra la dirección MAC del dispositivo que envió el paquete y el puerto por el que llegó. De esta manera, construye una tabla de direcciones (tabla de MAC) que le permite saber dónde enviar futuros paquetes dirigidos a esa dirección MAC.

4. **Comunicación simultánea**:
   - Los switches permiten que múltiples dispositivos se comuniquen al mismo tiempo, mejorando el rendimiento de la red. Esto se debe a que cada puerto en un switch tiene su propio canal de comunicación.

5. **Soporte para VLANs**:
   - Muchos switches modernos son **switches gestionados** y permiten la configuración de VLANs (Redes de Área Local Virtuales), lo que permite segmentar el tráfico de red en diferentes subredes lógicas.

6. **Reducción de colisiones**:
   - Al dividir el tráfico en diferentes puertos y usar la tabla de direcciones MAC, un switch reduce las colisiones de datos que ocurren en la red, mejorando el rendimiento general.

### Tipos de switches:

1. **Switches no gestionados**:
   - Son dispositivos simples que funcionan de manera plug-and-play. No requieren configuración y son adecuados para redes pequeñas o entornos domésticos.

2. **Switches gestionados**:
   - Estos switches ofrecen funciones de administración y configuración avanzada, como la capacidad de crear VLANs, monitorear el tráfico, y establecer políticas de seguridad. Son ideales para redes empresariales y grandes infraestructuras.

3. **Switches de capa 2**:
   - Operan en la capa de enlace de datos (capa 2) del modelo OSI y utilizan direcciones MAC para el envío de datos. Son los más comunes y se utilizan principalmente para la conmutación de tráfico dentro de la misma red local.

4. **Switches de capa 3**:
   - Además de las funciones de un switch de capa 2, estos dispositivos pueden realizar funciones de enrutamiento, operando en la capa de red (capa 3). Pueden dirigir el tráfico entre diferentes subredes, lo que los hace útiles en redes más complejas.

### Cómo funciona un switch:

1. **Recepción de paquetes**: Cuando un dispositivo envía un paquete de datos, este llega al switch a través de uno de sus puertos.

2. **Análisis de direcciones MAC**: El switch examina la dirección MAC de destino del paquete y verifica su tabla de direcciones para determinar a qué puerto debe enviarlo.

3. **Envío de datos**: El switch envía el paquete únicamente al puerto correspondiente, dirigiendo así el tráfico solo al dispositivo de destino.

### Ejemplo práctico:

Imagina una oficina con cinco computadoras y una impresora conectadas a un switch. Cuando una computadora quiere enviar un documento a la impresora, el paquete de datos es enviado al switch. El switch, al conocer la dirección MAC de la impresora (y su puerto correspondiente), envía el documento directamente a la impresora sin enviar el tráfico a las otras computadoras. Esto hace que la comunicación sea más eficiente y reduce el uso innecesario de ancho de banda.
