# VPN

Proporciona acceso a una red privada y a los recursos contenidos en ella a través de una red pública.

Es técnicamente un enlace WAN entre los extremos pero el usuario lo verá como un _private network link_ y de aquí el nombre virtual private network.


### Tipos
1. De acceso remoto (conexión de un equipo individual a una red).
		VPN sobre LAN (para aislar zonas y servicios de una red interna. utiliza IPSec, SSL, WEP, WPA...)
	1. De sitio-a-sitio (conectar dos redes entre sí). Permite compartir una red virtual cohesionada a usuarios separados geográficamente. También para conectar dos redes similares o protocolos diferentes como IPv4 i IPv6.
### Se puede clasificar por

- Los protocolos utilizados para encapsular el tráfico (túnel).
- Por punto de acceso/terminación del túnel. Es decir, si el cliente o red del proveedor ofrecen site-to-site o conectividad de acceso remoto.
- los niveles de seguridad proporcionados
- la capa OSI que presenta la red de conexión (como layer 2 o 3)
### Provee
Confidencialidad, autentificación e integridad.

### Si no utilizamos VPN
ataques de identidad, privacidad, integridad, comunicaciones inseguras (Se deberán de hacer a nivel de aplicación imap, ssh o por túnes ssh)

### Protocolos más comunes de VPN
1. **IPSec** (_Internet Protocol Security_). Es el estándard. Los protocolos de túnel de capa 2 corren a través de IPSec. Empaqueta un paquete IP (encriptándolo) dentro de un paquete IPSec que viajará por el túnel. Al final de él, lo desencapsula y lo desencripta y se reenvía a su destino. 
2. **Transport Layer Security**. Permite hacer un túnel sobre toda la red o aplicarlo a una conexión individual. Soluciona problemas q tiene IPSec en sitios con NAT y FW.
3. **Datagram Transport Layer Security**. 
4. MPPE (PPTP, SSTP, L2TP)
5. MPVPN
6. SecureShell (SSH) VPN - OpenSSh ofrece la posibilidad de hacer un VPN tunneling (distinto del port forwarding) para asegurar conexiones remotas a una red o entre redes

### IPSec
Dos protocolos: AH y ESP. Dos modos, transport (protege dato. El IPSEC header va dsps q el header de la IP) y tunneling(protege dato y cabecera. Mensaje encapsulado dentro de paquete)

### SSL
Protege el canal de comunicación utilizando la llave encriptación de clave pública y trabajando en la capa de sesión/transporte. También provee encriptación de los datos, autentificación, integridad

### OpenVPN

- Tiene 1 clave simétrica (instalada en cliente y servidor) No necesita CA
- 4 Claves asimétricas (2 públicas y 2 privadas) Necesita CA

### Wireguard

Crea una VPN para cuando nos queremos conectar a una red no segura. Utiliza TLS y certificados para autentificar y establecer túneles entre sistemas.


