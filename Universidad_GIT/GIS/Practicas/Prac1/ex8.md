
Sovint els usuaris escullen contrasenyes que no són comunes en relació a les contrasenyes triades per altres usuaris, però que són fàcilment deduïbles per un atacant que disposa d’informació contextual. En aquest exercici, executarem atacs de diccionari fent servir diccionaris construïts manualment a partir de la informació contextual que es disposa sobre el propietari de la contrasenya que es vol atacar.

Per cada hash, proporcioneu la funció hash utilitzada, la preimatge i el diccionari utilitzat. Expliqueu com heu construït el diccionari.
Nota: Podeu fer servir l’estratègia que considereu oportuna per aconseguir els diccionaris. Una alternativa és construir-los manualment o fer servir diccionaris de tercers (que trobareu a Internet).

h8_1: 475030fe8a8e6c11c71461aafc51bee15c3fb4511daef0cc2daf17879ea7153a 
h8_2: dc75c9921b47d40ee704c58778a2cc1f6f4f5f4c144f019ba897ffe3613c820d

La informació contextual que disposeu per a fer l’exercici és la següent:
1. El hash h1 correspon a la contrasenya de la Marta, una noia que està enamorada de la ciutat on viu.
2. El hash h2 correspon a la contrasenya d’en Marc, un mestre català que acaba d’aprovar les oposicions i
està pendent que li assisnin escola de destí.




# 1 Endivinar tipus de hash

```shell
echo -n "475030fe8a8e6c11c71461aafc51bee15c3fb4511daef0cc2daf17879ea7153a" | hash-identifier
```

Es sha-256


# 2 Buscar diccionario

Se ha buscado en internet `wordlist cities github` y se ha descargado list.txt.

# 3. Desencriptarlo con john
```shell
john --wordlist=test.txt --format=raw-sha256 hash.txt
```

```shell
└─$ john --wordlist=list.txt --format=raw-sha256 hash1.txt
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-SHA256 [SHA256 256/256 AVX2 8x])
Warning: poor OpenMP scalability for this hash type, consider --fork=6
Will run 6 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
Montblanc        (?)     
1g 0:00:00:00 DONE (2025-03-12 21:15) 20.00g/s 1580Kp/s 1580Kc/s 1580KC/s Aabenraa..Yeoncheongun
Use the "--show --format=Raw-SHA256" options to display all of the cracked passwords reliably
Session completed. 
```

La contraseña es montblanc

# 4 Comprobar

Con este script se ha comprobado que el `hash1` sea igual que hacer el hash en sha-256 de Montblanc:

```python
import hashlib

# Contraseña que quieres hashear
password = "Montblanc"

# Crear un objeto hash utilizando SHA-256
hashed_password = hashlib.sha256(password.encode()).hexdigest()

# Mostrar el hash resultante
print("SHA-256 Hash de 'Montblanc':", hashed_password)
```


----
# Parte 2

Para encontrar una lista de escuelas catalanas se ha buscado `llista d'escoles catalanes`.
Salía un mapa pero podia descargalo en xlsx. Resulta ser que es en formato excel. Me lo he descargado, lo he abierto en un excel y he copiado la segunda columna, correspondiente al nombre de las escuelas. He pegado el nombre de las escuelas en un .txt y le he aplicado el john:

```shell
john --wordlist=prova.txt --format=raw-sha256 hash2.txt
```

Me ha dado como contraseña Sadako.

Al hashear Sadako me da la misma contraseña.