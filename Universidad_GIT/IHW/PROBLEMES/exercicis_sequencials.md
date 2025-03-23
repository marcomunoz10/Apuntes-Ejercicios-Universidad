## Exercici 1

```systemVerilog

module exercici1(
    input logic [2:0] Z1,
    input logic [1:0] ZUAL,
    input logic LA, LB, LC, Lff, LACC,
    input logic clk, rst_n,
    output logic [31:0] X, BusA, BusB
);

logic [31:0] A, B, C, ff, ACC;

always_comb
begin
	case(Z1)
		3'd0: BusA = 32'd10;
		3'd1: BusA = 32'd100;
		3'd2: BusA = 32'd1000000;
		// d3 i d4 no definits (imatge)
		3'd5: BusA = A;
		3'd6: BusA = B;
		3'd7: BusA = C;
		default: BusA = 32'd0;
	endcase
end

//UAL
logic [31:0] w;
always_comb
begin
	case(ZUAL)
		2'd0: w = BusA + BusB;
		2'd1: w = BusA * BusB;
		2'd2: w = (BusA >= BusB) ? 32'd1 : 32'd0;
		2'd3: w = BusA;
		default: w = 32'd0;
	endcase
end

//Registre ACC
logic [31:0] ACC;
always_ff @(posedge clk or negedge rst_n) begin
    if (!rst_n)
        ACC <= 32'd0;
    else if (LACC)
        ACC <= w;
end

//Registre FF
logic [31:0] ff;
always_ff @(posedge clk or negedge rst_n) begin
    if (!rst_n)
        ff <= 32'd0;
    else if (Lff)
        ff <= w;
end


always_ff @(posedge clk, negedge rst_n) 
	if (!rst_n) 
		begin 
			A <= 32’d0;
			B <= 32’d0; 
			C <= 32’d0;  
		end 
	else 
		begin 
			if (LA) A <= BusB; 
			if (LB) B <= BusB;
			if (LC) C <= BusB; 
		end 
end

// Assignació de sortida 
assign X = ff; 
assign BusB = ACC; 

endmodule
```


## Exercici 2

```systemVerilog
module exercici2(
    input logic clk, rst_n, inici, X,
    output logic [2:0] Z1,
    output logic [1:0] ZUAL,
    output logic LA, LB, LC, LACC, Lff
);

  typedef enum logic [3:0] {
    S0, S1, S2, S3, S4, S5, S6, S7
  } state_t;
  
  state_t estat, seguent_estat;

  always_ff @(posedge clk or negedge rst_n) begin
    if (!rst_n)
      estat <= S0;
    else
      estat <= seguent_estat;
  end

  always_comb begin
    Z1 = 3'b000;
    ZUAL = 2'b00;
    LA = 1'b0;
    LB = 1'b0;
    LC = 1'b0;
    LACC = 1'b0;
    Lff = 1'b0;
    
    case (estat)
      S0: 
        if (inici)
          seguent_estat = S1;
        else
          seguent_estat = S0;
      
      S1: begin
        Z1 = 3'b111; 
        ZUAL = 2'b11;
        LACC = 1'b1;
        seguent_estat = S2;
      end
      
      S2: begin
        Z1 = 3'b010; 
        ZUAL = 2'b10;
        Lff = 1'b1;
        seguent_estat = S3;
      end
      
      S3:
        if (X == 1'b0)
          seguent_estat = S0;
        else
          seguent_estat = S4;
      
      S4: begin
        Z1 = 3'b110; 
        ZUAL = 2'b00;
        LACC = 1'b1;
        seguent_estat = S5;
      end
      
      S5: begin
        Z1 = 3'b000;
        ZUAL = 2'b00;
        LACC = 1'b1;
        seguent_estat = S6;
      end
      
      S6: begin
        Z1 = 3'b001; 
        ZUAL = 2'b01;
        LACC = 1'b1;
        seguent_estat = S7;
      end
      
      S7: begin
        Z1 = 3'b000;
        LC = 1'b1;
        seguent_estat = S2;
      end
      
      default: seguent_estat = S0;
    endcase
  end

endmodule
```

