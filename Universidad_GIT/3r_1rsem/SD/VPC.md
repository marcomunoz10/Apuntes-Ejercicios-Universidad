Una **VPC** (Virtual Private Cloud) es una red privada virtual dentro de un proveedor de servicios en la nube, como Amazon Web Services (AWS), Google Cloud Platform (GCP) o Microsoft Azure. Esta red es lógica, es decir, no existe físicamente como una red tradicional, pero permite controlar el tráfico y el acceso a los recursos desplegados en la nube de manera similar a como se haría en una red local o corporativa.

En una **VPC**, puedes definir un rango de direcciones IP privadas, crear subredes, configurar tablas de enrutamiento y aplicar políticas de seguridad, entre otras funciones, lo que te permite tener control total sobre el entorno de red de los recursos (servidores, bases de datos, aplicaciones, etc.) desplegados en la nube.

### Características clave de una VPC:
1. **Aislamiento**: Los recursos desplegados en una VPC están aislados de otros recursos del mismo proveedor, proporcionando un entorno de red privado.
2. **Configuración personalizada**: Puedes definir reglas de enrutamiento, seguridad, y otros parámetros de la red.
3. **Conectividad**: Las VPC permiten conectar tus recursos en la nube con otras redes privadas, redes públicas de Internet o redes locales (on-premises) mediante VPNs o conexiones dedicadas.
4. **Control sobre la red**: Puedes definir reglas de seguridad como firewalls (mediante *security groups* y *network ACLs* en AWS) para controlar el acceso a los recursos.

### ¿Por qué es necesario tener una **VPC** y al menos **1 subred**?

1. **Segmentación de la red**:
   - La subred es una porción de la VPC, y actúa como un contenedor lógico donde se implementan recursos, como máquinas virtuales (EC2 en AWS, por ejemplo). Tener al menos una subred es necesario porque es la forma en que puedes segmentar la red y controlar la ubicación de tus recursos.
   - Las subredes permiten segmentar la red en zonas con diferentes configuraciones de seguridad y acceso. Por ejemplo, podrías tener una subred pública, accesible desde Internet, y otra subred privada, donde los recursos solo se comunican de forma interna.

2. **Zonas de disponibilidad**:
   - Las subredes en muchos proveedores de nube están asociadas a una **zona de disponibilidad** (o **availability zone**), que es una ubicación geográfica aislada dentro de una región. Esto garantiza redundancia y alta disponibilidad. Para que los recursos puedan ser desplegados dentro de una región, es necesario al menos una subred, y a menudo se recomienda tener varias subredes en distintas zonas de disponibilidad para tolerancia a fallos.

3. **Aislamiento de tráfico y seguridad**:
   - Las subredes permiten controlar el flujo de tráfico a nivel granular. Por ejemplo, podrías tener una subred pública para recursos como servidores web, que deben ser accesibles desde Internet, y subredes privadas para bases de datos u otros servicios internos, que deben permanecer aislados del acceso público.

4. **Enrutamiento de tráfico**:
   - Para que los recursos dentro de la VPC puedan comunicarse entre sí, con otros recursos dentro de la red, o con el exterior, deben estar asociados a una subred con una tabla de enrutamiento específica. Sin una subred, no se puede definir cómo el tráfico debe fluir dentro y fuera de la red.

