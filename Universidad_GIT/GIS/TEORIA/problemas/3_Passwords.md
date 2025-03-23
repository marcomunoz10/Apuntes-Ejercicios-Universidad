
### Password Guessing
- Online
- Offline:
	- Brute force
	- Dictionary:
		- ClearText (em de fer el càlcul de la funció de passwords)
		- Precomputed (Rainbow Tables)

## Exercici1

Exemple:
`ACDB345`
puc fer la funció reduction d'un hash d'ACD, que podria ser que una part de la contrassenya em doni aquest valor.

```
Passwords: d1 d2 d3

H(d1,d2,d3) = d1 + d2 + d3

H(2,3,4) = 9; H(551) = 11

```

```
· R(hashed_passwords[3] = hashed_passwords[3]*11 mod(1000))
	R(9) = 099 --> podría ser que aquest resultat fos la password de hashed_passwords[5], per exemple
```

# Com es fa?

1. Es creen les cadenes. S'agafa un possible password (123)

```
password = 123
print(hash(password))
>>> 6
>>> reduction(6)
>>> 066
>>> hash(066)
>>> 12
>>> reduction(12)
>>> 132
>>> hash(132)
>>> 6
```

```
>>> password = 356
>>> hash(356)
>>> 14
>>> reduction(14)
>>> 154
>>> hash(154)
>>> 10
>>> reduction(10)
>>> 110
>>> hash(110)
>>> 2
>>> reduction(2)
>>> 022
>>> hash(022)
>>> 4
>>> reduction(4)
>>> 44
>>> hash(44)
>>> 8
```

Al final es publica la taula (les cadenes comprimides --> el primer i l'últim element) en el primer cas seria `123` i `6`.

a) 
Per poder trobar la password em de fer un atac de força bruta fins que tingui 14.
S'ha d'assumir q dintre d'aquestes cadenes sortirà un 14.

1. Buscar cadena que contè y = 14
```
inici P1 --> -..... --> 14 --> .... --> y1 final
```

Es busca el final de la cadena. No saps q el 14 està al mig.

```
>>> reduction(14)
>>> 154
>>> hash(154)
>>> 10 -- he arribat al final d'una cadena? sí, llavors saps que estàs en la cadena de 040 --> 10 
```

![[Pasted image 20250221120115.png]]

Un cop es troba la cadena s'ha de trobar el password. Les cadenes no es pot anar cap enrere. S'ha de tornar a començar des del principi fins arribar al 14.

2. Reconstruir la cadena.
```
>>> 040
>>> hash(040)
>>> 4
>>> reduction(4)
>>> 44
>>> hash(44)
>>> 8
>>> reduction(8)
>>> 088
>>> h(088)
>>> 16
>>> r(16)
>>> 176
>>> h(176)
>>> 14
```





