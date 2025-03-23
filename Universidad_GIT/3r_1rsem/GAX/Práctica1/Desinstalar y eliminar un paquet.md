Desinstal·lar i eliminar la configuració del paquet systemd-resolved, ja que entra en conflicte amb dnsmasq en utilitzar el mateix port: 
```shell
apt remove --purge systemd-resolved
```
