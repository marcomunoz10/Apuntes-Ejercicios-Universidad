1. instalar fail2ban
2. ir a `/etc/fail2ban` y crear carpeta `jail.local`
3. Añadir configuración:
```shell
[atac-ddos]
enabled  = true
port     = http,https
filter   = atac-ddos
logpath  = /var/log/apache2/access.log  # O el teu fitxer de log (per exemple, nginx)
maxretry = 5                             # Nombre màxim d'intents abans de bloquejar
findtime = 10                            # Temps en segons per contar els intents
bantime  = 600                           # Temps (en segons) que es mantindrà bloquejada la IP
action   = iptables[name=HTTP, port=http, protocol=tcp]
```
4. crear el filtre
`sudo nano /etc/fail2ban/filter.d/atac-ddos.conf`
```shell
[Definition]
failregex = ^<HOST> -.*"(GET|POST).*HTTP.*$
ignoreregex =
```

5. `sudo systemctl restart fail2ban`
6. `sudo fail2ban-client status atac-ddos`
7. me pone error, dice falllada al acceder al camino del socket
8. era porque tenia que hacer copia de `jail.conf` a `jail.local`. web
   https://asir.gonzaleztroyano.es/projects/iaw/seguridad/configuracion-fail2ban.html
9. al hacer `sudo journaltcl -u fail2ban` me dice que es imposible acceder a `iptables` en `action.d`. Miro permisos.
10. `sudo chmod 644 /etc/fail2ban/action.d/iptables.conf`
11. ahora se me queja de logs sshd, pero realmente no voy a utilizarlo.
		Busco la linea donde aparece [ssh] y pongo `enabled = false`

12. Procedo a intentar volver a crear la jail
```shell
[apache-http-auth]
enabled  = true
port     = http,https
filter   = apache-auth
logpath  = /var/log/apache2/*access.log
maxretry = 3
bantime  = 600
```

filter:
```shell
[Definition]
failregex = ^<HOST> -.*"GET.*HTTP.*$
ignoreregex =
```