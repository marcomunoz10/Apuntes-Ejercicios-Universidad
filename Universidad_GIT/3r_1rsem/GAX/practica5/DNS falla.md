reiniciar reglas IPTABLES

```bash
#!/bin/bash

# Mostrar un mensaje informativo
echo "Eliminando todas las reglas de iptables..."

# Eliminar todas las reglas de filtrado
sudo iptables -F

# Eliminar todas las reglas de la tabla NAT
sudo iptables -t nat -F

# Eliminar todas las reglas de la tabla mangle
sudo iptables -t mangle -F

# Eliminar todas las cadenas personalizadas
sudo iptables -X

# Restaurar la política por defecto a ACCEPT
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -P OUTPUT ACCEPT

# Mostrar el estado actual de las reglas de iptables
echo "Reglas de iptables después de la eliminación:"
sudo iptables -L

# Confirmar la eliminación
echo "¡Las reglas de iptables han sido eliminadas y las políticas restauradas a ACCEPT!"

```