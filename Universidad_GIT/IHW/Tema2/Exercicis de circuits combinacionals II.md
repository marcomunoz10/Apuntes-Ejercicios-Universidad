
# Exercici2

![[Pasted image 20250226152936.png]]

**`T`** representa la **temperatura de la assecadora**. Es un valor de 4 bits que refleja las señales generadas por el termostato **A, B, C y D**. Los valores de `A`, `B`, `C` y `D` son las señales de salida del termostato y van cambiando según la temperatura de la secadora. Como se indica en el enunciado, las señales evolucionan de la siguiente manera:

- **Inicialmente**, cuando la temperatura es baja, todas las señales están en 0: `A = B = C = D = 0`, lo que significa que **T = 0000**.
- Cuando la temperatura alcanza el primer umbral **TA**, la señal `A` cambia a 1, y `B`, `C`, y `D` siguen en 0, es decir, **T = 1000**.
- Cuando la temperatura alcanza el segundo umbral **TB**, las señales `A` y `B` son 1, y `C` y `D` siguen en 0, es decir, **T = 1100**.
- Esto sigue evolucionando a lo largo de los umbrales **TC** y **TD**.
## Uso de define

Los `define` simplemente establecen **valores constantes** de comparación en el código antes de la compilación.

```systemverilog
`define TA 4'b1000 
`define TB 4'b1100 
`define TC 4'b1110 
`define TD 4'b1111
```

Esto significa que cuando el compilador ve **`TA`**, lo reemplaza por `4'b1000`, y lo mismo con los demás..


```systemverilog
module assecadora(A,B,C,D,W1,W2,W3);

	input logic A,B,C,D;
	output logic W1,W2,W3;

	logic [3:0] T; //vector de 4 bits [3] [2] [1] [0]
	assign T={A,B,C,D} //T es la concatenación de los 4 bits A,B,C,D
```

## Diseño de entradas y salidas
Hoy en día las entradas y salidas sí que se pueden definir como vectores, pero en hardware antiguo no se hacía. Es por eso que el profesor, después de haber declarado `output logic W1, W2, W3`, declara un vector W con estos valores:

```systemverilog
logic [2:0] W;//Vector de 3 bits [2] [1] [0]
assign W1 = W[2];
assign W2 = W[1];
assign W3 = W[0];
```


## Descripción circuito combinacional.

