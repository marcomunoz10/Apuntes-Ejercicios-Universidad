Activar el NAT en A: 
```shell
iptables -t nat -A POSTROUTING -o <internet_interface> -j MASQUERADE
```

### Partes del comando:

1. **`iptables`**: Es la utilidad de línea de comandos para configurar las reglas del cortafuegos (firewall) en sistemas Linux.
    
2. **`-t nat`**: Especifica que se va a agregar la regla en la tabla `nat` (Network Address Translation) se utiliza para modificar las direcciones IP en los paquetes (tráfico de red).
    
3. **`-A POSTROUTING`**: Aquí se indica que se quiere **añadir** (`-A`, que viene de "append") una regla en la **cadena** `POSTROUTING`. Esta cadena se aplica a los paquetes _después_ de que han sido enrutados, es decir, justo antes de que salgan por una interfaz de red. `-D` seria para quitar una regla.

    
4. **`-o <internet_interface>`**: La opción `-o` especifica la **interfaz de salida**. Aquí debes reemplazar `<internet_interface>` con el nombre de la interfaz que está conectada a Internet, como por ejemplo `eth0`, `wlan0`, etc. Esto indica que la regla solo se aplicará a los paquetes que salen por esa interfaz en particular.
    
5. **`-j MASQUERADE`**: El parámetro `-j` define la **acción** que se tomará cuando los paquetes coincidan con la regla. En este caso, la acción es `MASQUERADE`. Esto significa que el sistema debe modificar las direcciones IP de origen de los paquetes salientes, sustituyéndolas por la dirección IP externa de la interfaz de salida (`<internet_interface>`). El objetivo de la regla de `MASQUERADE` es hacer posible el NAT dinámico, lo que permite compartir una única dirección IP pública entre múltiples dispositivos de una red local.

Instalar el paquete `iptables-persistent` para guardar las reglas.
Utilizar el comando
```shell
netfilter-persistent save
```
para guardar las reglas.