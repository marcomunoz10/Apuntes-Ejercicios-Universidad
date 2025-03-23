
### Header

L'element **`<header>`** s'utilitza per **definir una capçalera** d'una secció o d'una pàgina. Aquesta capçalera sol contenir elements com el títol (amb etiquetes com `<h1>`, `<h2>`, etc.), logotips, menús de navegació o altres elements introductors d'aquesta secció.

```html
<header>
            <a href="http://www.uab.cat" target="_blank"> <img src="uab.png" width="200px"></img></a>

            <h1> Escola d'Enginyeria </h1>

            <p> Benvinguts a l'Escola d'Enginyeria de la UAB </p>

            <hr/>

        </header>
```

h1: Títol principal de la pàgina.
h2: Subtítols principals de seccions.
h3: Subtítols de subseccions.


### Section

`<section>` s'utilitza per organitzar i estructurar el contingut d'una pàgina en diferents seccions relacionades temàticament. És útil quan vols dividir una pàgina en blocs lògics, cadascun amb el seu propi tema o funció.

Exemple:
```html
<section>
    <header>
	    <h2>Sobre nosaltres: </h2>
    </header>
    <p>Som una empresa dedicada a...</p>
</section>

<section>
            <header>
                <h3>Graus disponibles: </h3>
            </header>
            ...
</section>

```

`h2`: Utilitzat per a subtítols de seccions principals.
`h3`: Utilitzat per a subseccions de les seccions principals.


### Formulari
```html
<form method="post" action="">
</form>
```

- **`method="post"`**:
    
    - Especifica el **mètode HTTP** que s'utilitzarà per enviar el formulari. En aquest cas, és `POST`, que s'utilitza per enviar dades al servidor de manera segura i no visible a la URL. S'utilitza quan estàs enviant dades importants o que poden modificar recursos al servidor (com informació de registre, dades d'un formulari de contacte, etc.).


- **`action=""`**:
    
    - Especifica l'**URL** o la destinació a la qual es volen enviar les dades del formulari.
    - En aquest cas, està buit (`""`), cosa que vol dir que quan l'usuari enviï el formulari, les dades es processaran a la **mateixa pàgina** en la qual està el formulari. Si especifiques una URL, el formulari enviarà les dades a aquesta destinació.