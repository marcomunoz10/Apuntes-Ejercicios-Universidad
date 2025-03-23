https://www.youtube.com/watch?v=uPdN25TCi7k

La primera entrada de la rainbow table se le aplica un hash del texto plano.
![[Pasted image 20250220233802.png]]

Una vez que se tiene el hash se le aplica la función de Reducción.
reduction(): transforma el hash en una posible contraseña en texto plano (por ejemplo decodeando en base 64).
![[Pasted image 20250220233835.png]]

Se hace el mismo proceso otra vez (se hashea y se hace una reducción). Esto se hace las veces que queramos:
![[Pasted image 20250220234020.png]]



Los únicos valores que tenemos que guardar en la rainbow table es el primer y último valor.
![[Pasted image 20250220234143.png]]


La tabla es un conjunto de pares donde cada par contiene la palabra inicial `P1` y la última `P4`:
```python
table = {(P1, P4), ...}
```


Lo que intenta hacer el atacante con esto es obtener la contraseña a partir del hash. Entonces compara ese hash con por ejemplo `P4`:

![[Pasted image 20250220234742.png]]


Lo que se obtiene de esto es el `P3`:
![[Pasted image 20250220234821.png]]


---
Classe problemes
```
hashed_passwords = hash(ista_passwords)  
```

ara fa la funció al revés
```
hash_al_reves = reduction(hashed_passwords)
```
No es pot invertir hash, el hash no té funció inversa. Llavors el reduction de per exemple el hash de `P1` et pot donnar `P5` de manera arbitrària.


