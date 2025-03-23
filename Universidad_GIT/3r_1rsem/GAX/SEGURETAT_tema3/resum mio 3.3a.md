# **Firewalls**

Bloquea mensajes que entran y salen de una intranet y no cumplen con determinadas reglas. Es frecuente conectarla a una *zona desmilitarizada* `dmz` en la que se ubican servidores.

Tipos:

* **Nivel de aplicación**. Aplica mecanismos de seguridad para los datos de aplicaciones específicas, tales como servidores HTTP (se puede filtrar por URL), FTP. Se le puede denominar proxy. Permite controlar `Intranets` i `WAF` (_Web Application Firewall_)* .
* **Nivel de transporte/sesión**. Capa 4 y 5. Aplica cuando se establece comunicación `TCP/UDP`
* **Nivel de paquetes**. Capa 3/4. Se aplican filtros según `IP destino/origen`.
* Capa **Network**. Capa 2/3. Actúa sobre cabeceras e `IP`.
		* WAF (Filtra tráfico `HTTP` hacia y desde una app WEB. Protege inyección SQL, XSS y CSRF)
* A nivel personal.

# Iptables

(está dentro del kernel de LINUX)
Características de éste firewall:
- Actúa en capa 3/4 (según chat).
- Mantiene un registro de cada conexión que pasa a través de él.
- Filtrado por MAC y `flags`. 
- Limitaciones en la velocidad.
- Trabaja con tablas `NAT` y `Filtro` y cadenas (Forward, input...)
- Solo tiene un objetivo por regla (`-j ACCEPT` ...)

Por temas de escalabilidad, rendimiento y mantenimiento del código se inventó `NFTables`.

Hay tres tablas:
1. **Mangle**. Filtra por flags (bits) en la cabecera de TCP.
2. **Filter**. Responsable del filtrado de paquetes. Las 3 cadenas más importantes:
		- **Forward** filtro a los que pasan **a través** del firewall. (Tráfico entre dos redes que pasa a través del dispositivo donde está configurado el firewall).
		- **Input** filtro a los que **entran** al firewall.
		- **Output** filtro de **salida** del firewall.
3. **NAT**. Responsable de la traducción de direcciones de red. Las 2 chains más importantes:
		- **Post-routing**. La dirección de **origen** debe ser cambiada (ej. envío un paquete y tengo ip privada, necesito una ip pública) Regla **MASQUERADE**
		- **Pre-routing**. La dirección de **destino** debe ser cambiada. (para que le llegue a la ip privada.) Regla **DNAT**
Reglas:
* ACCEPT: el paquete es aceptado. Iptables ends.
* DROP: el paquete es bloqueado. Iptables ends.
* LOG : La info del paquete es enviado a syslog . iptables continua con la siguiente regla. REJECT (igual q e el drop pero envía un mensaje de error.) 
* DNAT : utilizado para la traducción de dirección de **destino**
* MASQUERADE : utilizado para la traducción de dirección de **origen**. 

Opciones de relga (Switch):

- **-t table**: Si no se especifica, la tabla _filter_ es la escogida.
- **-j target**: Salta a una cadena objetivo cuando se verifica la presente regla.
- **-A**: Agrega una regla al final de la cadena.
- **-F**: Limpia. Borra todas las reglas en la tabla seleccionada.
- **-p (protocol-type)**: Especifica el protocolo (icmp, tcp, udp, y all).
- **-s (ip-address)**: Especifica la dirección IP de origen.
- **-d (ip-address)**: Especifica la dirección IP de destino.
- **-i (interface-name)**: Especifica la interfaz de entrada (entrada de paquetes).
- **-o (interface-name)**: Especifica la interfaz de salida (salida de paquetes).

hay más, algunas aparecen más adelante como por ejemplo `-P` indica política por defecto

#### NFTables:
* No hay tablas, trabaja con cadenas predeterminadas.
* Puede incluir más de un objetivo por regla.


### **Ejemplos de Configuración de Firewall**

#### **1. Aceptar paquetes TCP hacia una IP específica:**

```bash
iptables -A INPUT -s 0/0 -i eth0 -d 192.168.1.1 -p TCP -j ACCEPT
```

- **Explicación**: Permite paquetes TCP que lleguen por la interfaz `eth0`, desde cualquier IP (`0/0`), a la IP `192.168.1.1`.

---

#### **2. Aceptar paquetes TCP para enrutamiento con condiciones específicas:**

```bash
iptables -A FORWARD -s 0/0 -i eth0 -d 192.168.1.58 -o eth1 -p TCP --sport 1024:65535 --dport 80 -j ACCEPT
```

- **Explicación**: Permite el enrutamiento de paquetes TCP con puerto de origen entre `1024-65535` y destino `80`, entre las interfaces `eth0` y `eth1`.

---

#### **3. Limitar solicitudes ICMP (ping):**

```bash
iptables -A INPUT -p icmp --icmp-type echo-request -m limit --limit 3/s -i eth0 -j ACCEPT
```

- **Explicación**: Limita las solicitudes `echo-request` (ping) a un máximo de 3 paquetes por segundo en la interfaz `eth0`.

---

#### **4. Guardar y restaurar reglas:**

- **Guardar reglas:**
    
    ```bash
    iptables-save > /etc/sysconfig/iptables
    ```
    
- **Restaurar reglas:**
    
    ```bash
    iptables-restore -c < /etc/sysconfig/iptables
    ```
    
- **Explicación**: Estos comandos guardan y restauran las reglas de firewall desde un archivo.
    

---

#### **5. Limpiar las reglas actuales:**

```bash
# Eliminar todas las reglas y cadenas personalizadas
iptables -F
iptables -X
iptables -Z

# Limpiar reglas en la tabla NAT
iptables -t nat -F
```

- **Explicación**: Elimina todas las reglas y cadenas personalizadas, y limpia las reglas en la tabla NAT.

---

#### **6. Establecer la política por defecto:**

```bash
# Establecer política por defecto a ACCEPT para todas las cadenas
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT

# Establecer política por defecto a ACCEPT en la tabla NAT
iptables -t nat -P PREROUTING ACCEPT
iptables -t nat -P POSTROUTING ACCEPT
```

- **Explicación**: Establece que, por defecto, se acepten todos los paquetes en las cadenas principales y en la tabla NAT.

---

#### **7. Denegar todo por defecto (opción más restrictiva):**

```bash
#!/bin/sh
echo -n "Aplicando Reglas de Firewall..."
# FLUSH de reglas anteriores
iptables -F
iptables -X
iptables -Z
iptables -t nat -F

# Política por defecto: DROP (denegar todo por defecto)
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP
```

- **Explicación**: Este script configura el firewall para denegar todo el tráfico por defecto (DROP), permitiendo solo lo que se especifique posteriormente.

---

#### **8. Permitir tráfico en la interfaz de loopback (localhost):**

```bash
/sbin/iptables -A INPUT -i lo -j ACCEPT
/sbin/iptables -A OUTPUT -o lo -j ACCEPT
```

- **Explicación**: Permite todo el tráfico en la interfaz de loopback (`lo`), esencial para la comunicación interna del sistema.

---

#### **9. Permitir tráfico desde una IP específica:**

```bash
# Permitir tráfico desde una IP específica
iptables -A INPUT -s 195.65.34.234 -j ACCEPT
```

- **Explicación**: Permite tráfico entrante desde la IP `195.65.34.234`.

---

#### **10. Permitir tráfico desde una IP para MySQL:**

```bash
# Permitir tráfico desde la IP de un amigo para acceder a MySQL (puerto 3306)
iptables -A INPUT -s 231.45.134.23 -p tcp --dport 3306 -j ACCEPT
```

- **Explicación**: Permite conexiones TCP desde la IP `231.45.134.23` hacia el puerto `3306` (MySQL).

---

#### **11. Permitir tráfico FTP desde otra IP:**

```bash
# Permitir tráfico FTP (puertos 20 y 21) desde la IP de un amigo (FTP)
iptables -A INPUT -s 80.37.45.194 -p tcp --dport 20:21 -j ACCEPT
```

- **Explicación**: Permite conexiones FTP desde la IP `80.37.45.194`.

---

#### **12. Permitir tráfico web (puerto 80):**

```bash
# Permitir tráfico HTTP (puerto 80)
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

- **Explicación**: Permite conexiones HTTP (puerto `80`).

---

#### **13. Bloquear puertos no deseados:**

```bash
# Bloquear puertos FTP, MySQL, SSH y Webmin
iptables -A INPUT -p tcp --dport 20:21 -j DROP
iptables -A INPUT -p tcp --dport 3306 -j DROP
iptables -A INPUT -p tcp --dport 22 -j DROP
iptables -A INPUT -p tcp --dport 10000 -j DROP
```

- **Explicación**: Bloquea el acceso a puertos no deseados (FTP, MySQL, SSH y Webmin).

---

#### **14. Confirmar configuración:**

```bash
echo "OK. Verifique con iptables -L -n"
```

- **Explicación**: Muestra un mensaje indicando que la configuración ha finalizado y sugiere verificar las reglas aplicadas con `iptables -L -n`.

---

#### **15. Permitir acceso a una red LAN:**

```bash
# Permitir acceso desde la red LAN (192.168.10.0/24) a través de la interfaz eth1
iptables -A INPUT -s 192.168.10.0/24 -i eth1 -j ACCEPT
```

- **Explicación**: Permite el acceso desde la red `192.168.10.0/24` a la interfaz `eth1`.

---

#### **16. Realizar enmascaramiento (NAT) para la LAN:**

```bash
# Enmascaramiento de la red LAN (192.168.10.0/24) al salir por la interfaz eth0
iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -o eth0 -j MASQUERADE
```

- **Explicación**: Realiza enmascaramiento (NAT) para que las IPs privadas de la LAN aparezcan como la IP pública de `eth0` al salir hacia Internet.

---

#### **17. Permitir el reenvío de paquetes (Forwarding):**

```bash
# Permitir el reenvío de paquetes
echo 1 > /proc/sys/net/ipv4/ip_forward
```

- **Explicación**: Habilita el reenvío de paquetes, permitiendo que el tráfico fluya entre interfaces.

---

#### **18. Cerrar puertos conocidos para todos los orígenes:**

```bash
# Cerrar puertos TCP 1-1024 para todos los orígenes
iptables -A INPUT -s 0.0.0.0/0 -p tcp --dport 1:1024 -j DROP

# Cerrar puertos UDP 1-1024 para todos los orígenes
iptables -A INPUT -s 0.0.0.0/0 -p udp --dport 1:1024 -j DROP
```

- **Explicación**: Bloquea el acceso a los puertos TCP y UDP en el rango `1-1024` para todos los orígenes.

---

### **Opción DENEGAR todo por defecto**

Es un enfoque más restrictivo, donde se deniega todo el tráfico por defecto y solo se permiten los puertos y conexiones necesarias a través de reglas adicionales, como se explicó en el ejemplo anterior.

---

Este conjunto de ejemplos cubre diversas situaciones típicas en la configuración de `iptables`, desde la aceptación de tráfico desde IPs específicas hasta el bloqueo de puertos no deseados, y proporciona tanto ejemplos de políticas por defecto como ejemplos de configuraciones de NAT y enrutamiento.
# **DMZ**

Si una empresa tiene un servidor web lo pondrá aparte, en una zona desmilitarizada DMZ.
![[Pasted image 20241215122805.png]]


