![[Pasted image 20250401105618.png]]


```systemverilog
module exercici1(a,b,c,m,n,r);

input logic a,b,c,m,n;
output logic r;
logic N, P, L, M;

assign N = (a-b);
assign P = (c + 1'b1);
assign L = c + (a + b);

assign M = (m>n ? N:P;
assign r = (m == n ? L:M);
endmodule
```

[+] Simplificando:
```systemverilog
module exercici1(a,b,c,m,n,r);

input logic a,b,c,m,n;
output logic r;
logic N, P, L, M;

r = (m == n ? (c + (a+b)):((m>n) ? (a-b):(c + 1'b1) ));
endmodule
```

[+] Si se utilizase `if-else` en vez de `?`:
1)
```systemverilog
if (m>n) M = N;
else M = P;

if (m == n) r = L;
else r = M;
```

2)
```systemverilog
always_comb
if(m==n) r = L;
else if(m>n) r = N;
else r = P;
```

3)
```systemverilog
always_comb
if(m==n) r = c + (a+b);
else if(m>n) r = a - b;
else r = c + 1'b1;
```