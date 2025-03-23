En linux se guarda en `/etc/passwords` o `/etc/shadow` con la forma de:

```shell
$<id>$<salt>$<encrypted-pass>
$<id>$<params>$<salt>$<encrypted-pass>
```

Donde **id** es el identificador del tipo de encriptación hash.

## Windows
Se utiliza **SAM** (_Security Accounts Manager_) file:

La función hash se realiza mediante LM(LAN Manager) basado en DES o a través de NTLM (NT Lan Manager) basado en MD4 a partir de Win VIsta