T es una señal que necesita actualizarse automáticamente cada vez que cambian A,B,C,D. A esto se le llama asignación contínua. Es por ello que se necesita `asign`.
```systemverilog
module ex2(A,B,C,D,W1,W2,W3);
`define TA 4'b1000;
`define TB 4'b1100;
`define TC 4'b1110;
`define TD 4'b1111;

input logic A,B,C,D;
output logic W1,W2,W3;
logic [3:0] T;
assign T = {A,B,C,D};
```

[+] La última asignación quiere decir:
`T[3] = A, T[2] = B, T[1] = C, T[0] = D`

Con `W1, W2, W3` pasa lo mismo, pero al revés, porque estas son una salida.

```systemverilog
logic [2:0] W;
assign W1 = W[2];
assign W2 = W[1];
assign W3 = W[0];
```


```systemverilog

`define TempA 4'b1000
`define TempB 4'b1100
`define TempC 4'b1110
`define TempD 4'b1111

module ex2(A,B,C,D,W1,W2,W3);

logic [3:0] T;
assign T = {A,B,C,D};
//lo mismo que assign T[3] = A; assign T[2] = B...

logic [2:0] W;
assign W1 = W[2];
assign W2 = W[1];
assign W3 = W[0];

always_comb begin
	if (T<`TempA) // equivale a if(T<4'b1000)
		W = 3'b110;
	else if (`TempA <= T < `TempB)
		W = 3'b100; 
end

//el mateix q possar:
always comb begin
case (T)
	4'b0000: W=3'b110; //No se podria poner TempA
	4'b1000: W=3'b100;
endmodule
```
