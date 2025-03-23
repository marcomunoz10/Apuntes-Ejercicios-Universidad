Els **biaixos** són paràmetres addicionals en una xarxa neuronal que permeten ajustar la sortida d'una neurona independentment dels valors d'entrada. Són com un "desplaçament" que s'afegeix a la suma ponderada de les entrades abans d'aplicar la funció d'activació.

- **Matemàticament:**  
    La sortida d'una neurona es calcula com:
    ![[Pasted image 20250315164055.png]]
- On:
    
    - wi​: Pesos de les connexions.
        
    - xi​: Entrades a la neurona.
        
    - b: Biaix.

- **Per què són importants?**  
    Els biaixos permeten a la xarxa aprendre patrons més complexos, ja que ajusten el punt en què la neurona s'activa (per exemple, quan es passa de valors negatius a positius en una funció d'activació com ReLU).