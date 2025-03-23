
Circuit mostrejador, el senyal d'entrada va canviant. Quan arriba el flang de pujada d'un rellotge em captura el valor. Es calcula el senyal D quan hi ha el senyal de rellotge

Tenen el temps de setup(temps mínim q la entrada D ha de ser estable abans del flang de rellotge pq aquest valor es pugui emmagatzemar corr dins del flipflop
sino entra en metaestabilitat --> captura un valor aleatori de la senyal analógica. És un circuit biestable i acabará caient en un 0 o un 1 aleatori.

Temps de hall(se toma un caramelo fuerte)


Quan arribi flang actiu de pujada la sortida ha de valer l'entrada. Es pot aturar el bucle always amb l'operador @.

per inferir un flip flop possar always_ff:
always_ff @(posedge CLK)


L'assignació és non-blocking.

**RST_asíncron**: quan s'aplica s'aplica en aquell moment.

Si no s'està fent el RST assignem el valor de sortida el valor de l'entrada.

Si arriba flang i senyal de rellotge és 1 la sortida és l'entrada.

```verilog
if (LOAD) Q <= D;
else Q <= Q //manté el valor i no ho possem
```

Tinc un multiplexor. Quan hi ha molts `if-else` amb vàries variables pot ser q ens oblidem d'uns dels casos... Quan pasa el simulador manté el valor `r` --> inferim un senyal de memòria **latch**.
```verilog
always @(m,n,a,b)
if(m>n) r = a+b;
else r = a-b; 
```

Els senyals de `RST` són crítics.
* Asíncron: Prohibits en ambients amb molta radiació (ex: espai).
* Síncron: baixa probabilitat de pols de tensió.

# Latch

Mentre es trobi en senyal alt la sortida segueix l'entrada. Quan passa de dalt a baix captura el nivell en aquest moment. Tenen un temps de setup y temps de hall.

# Sequencial circuits

