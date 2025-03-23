### Introducció
`VHDL` --> Pensat per equips més grans.

[[Netlist]] es una representación intermedia clave en el diseño y configuración de una [[FPGA]].
No es programa una FPGA, es programa un processador.

Un cop tinc clara l'FPGA ja puc fer una simulació física.

VHDL no és un programa, és una explicació de com és un circuit lògic. És una descripció de síntesi d'un circuit.

```vhdl
a + b = y ; -- a y se li assigna el resultat després de x ex 5 segons amb un wait
```

# Maneres de descriure 

He d'explicar la entity, com és la caixa (ha dibuixat un requadre al circuit amb entrades i sortides entrant i sortint)
1. **Structural** Descriu el que hi ha dins: vull q l'entrada a amb l'entrada b vagin a una porta AND, la sortida d'aquesta és l'entrada a un multiplexor... Descripció estructural: dir exactament quin és el senyal q es connecta amb cadascuna de les sortides.
2. **Dataflow**: l'única cosa que conservo són les connexió. En comptes d'afegir portes lògiqes s'afegeix la operació lògica equivalent a AND = a · b. Fa com una caixa on hi ha l'AND i possa les entrades petites i l'operació en mig en gran. Només es pot passar info si l'objecte és un senyal.
3. **Behaviooral** Explico comportament sortides i entrades únicament, lo de dins és una caixa negra. És una descripció comportamental. 

# Entity declaration
Puc llegir una entrada però **no puc llegir una sortida**. Si la necessito llegir treballo amb un senyal intern.
El búffer soluciona el problema. És una sortida especial que es pot llegir. S'ha d'evitar l'ús pq són molt escassos. 


![[Pasted image 20250221110320.png]]
En un circuit combinacional formen part totes les entrades primàries d'un circuit.
Les assignacions es fan a futur (en el següent nanosegon). Primer es farà l'event 1 i després l'event 2. (ex. cas de la `y` i les assignacions).
Les assignacions de senyals no són inmediates, són a futur pq aquest canvi triga un temps.
Si necessito fer canvis inmediats hi ha una estructura dins d'un process on puc afegir assignació de variables. Quan s'acaba l'execució d'un process desapareixen. La manera d'exportar informació a través de dins de un process és a través de senyals.

Possar sempre parèntesis!!


# Sentències seqüencials

Hi ha dos:
1. if-else
2. case

possar `when-else` fora d'un process


# Simulació
Fent servir un software com per exemple Quartus II
Fent servir una descripció `VHDL`
