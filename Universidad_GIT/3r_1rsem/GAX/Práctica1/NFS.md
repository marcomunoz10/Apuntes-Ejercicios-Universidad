Servidor:

```shell
sudo apt update
sudo apt install nfs-kernel-server
sudo mkdir /nfs-shared
sudo nano /etc/exports
/nfs-shared RED_MAQUINA_CLIENTE(rw,sync,no_subtree_check)
```

Una vez que hayas configurado `/etc/exports`, debes aplicar los cambios y reiniciar el servicio NFS.
```shell
sudo exportfs -ra
sudo systemctl restart nfs-kernel-server
```


```shell
systemctl restart|enable|start|stop nfs-kernel-server 
```

Cliente:
```shell
sudo apt-get install nfs-common
mkdir /mnt/nfs
sudo chmod u+w /mnt
sudo mount -t nfs SERVER_IP:/ruta/servidor /ruta/cliente
```

Para hacer que el montaje sea permanente:
```shell
cd /etc/fstab
IP_B:/ruta/servidor    /ruta/cliente    nfs    defaults    0  0
```

**Con touch ser hereda los permisos del directorio**

Verificar el montaje
```shell
df -h
```