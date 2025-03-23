Para configurar la VPN basada en WireGuard y cumplir con el ejercicio propuesto:

### Configuración

1. **Preparar MV-A y MV-B como Router y Cliente:**
    
    - **Instalación de WireGuard** (en ambas máquinas):
        
        ```bash
        sudo apt update
        sudo apt install wireguard
        ```
        
    - **Generar claves en cada máquina:**
        
        ```bash
        wg genkey | sudo tee /etc/wireguard/private.key
        chmod go= /etc/wireguard/private.key
        cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
        ```
        
2. **Configuración del servidor (MV-A):**
    
    - Crear `/etc/wireguard/wg0.conf`:
        
        ```shell
        [Interface]
        Address = 10.0.0.1/24
        PrivateKey = <PRIVATE_KEY_MV_A>
        ListenPort = 51820
        
        # Habilitar el reenvío de paquetes
        PostUp = iptables -A FORWARD -i wg0 -j ACCEPT
        PostUp = iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
        PostDown = iptables -D FORWARD -i wg0 -j ACCEPT
        PostDown = iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
        
        [Peer]
        PublicKey = <PUBLIC_KEY_MV_B>
        AllowedIPs = 10.0.0.2/32
        ```
        
3. **Configuración del cliente (MV-B):**
    
    - Crear `/etc/wireguard/wg0.conf`:
        
        ```shell
        [Interface]
        Address = 10.0.0.2/24
        PrivateKey = <PRIVATE_KEY_MV_B>
        
        [Peer]
        PublicKey = <PUBLIC_KEY_MV_A>
        Endpoint = 20.20.20.189:51820
        AllowedIPs = 0.0.0.0/0
        ```
        
4. **Activar IP Forwarding en MV-A:**
    
    - Editar `/etc/sysctl.conf`:
        
        ```
        net.ipv4.ip_forward=1
        ```
        
    - Aplicar cambios:
        
        ```bash
        sudo sysctl -p
        ```
        
5. **Arrancar la VPN:**
    
    - MV-A y MV-B:
        
        ```bash
        sudo systemctl start wg-quick@wg0
        ```
        

### Verificaciones

1. **Ping desde MV-B:**
    
    ```bash
    ping -c 4 google.com
    curl -I http://www.uab.cat
    ```
    
2. **Captura de paquetes en MV-A:**
    
    - En la interfaz `eth1` y `wg0`:
        
        ```bash
        sudo tcpdump -i eth1
        sudo tcpdump -i wg0
        ```
        
3. **Prueba desde MV-D:**
    
    - En MV-D, repite:
        
        ```bash
        ping -c 4 google.com
        curl -I http://www.uab.cat
        ```
        
4. **Capturas de pantalla:**
    
    - Documenta las pruebas, salidas de `ping` y `curl`, y las capturas de `tcpdump`.
5. **Analizar tráfico:**
    
    - Compara las diferencias entre los paquetes en `eth1` y `wg0`.

### Consideraciones

- La MV-A debe estar configurada como router, asegurando que enruta correctamente paquetes desde `wg0` a `eth0` y viceversa.
- La MV-D no está directamente conectada a `Middle`, por lo que necesitarás asegurarte de que sus paquetes puedan pasar a través de MV-B o configurar rutas apropiadas.