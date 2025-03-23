
# Exercici 1
```systemVerilog


module UnitatProces(
    input logic LA, LB, LC, Lff, LACC,
    input logic [2:0] Z1,
    input logic [1:0] ZUAL,
    input logic clk, rst_n,
    output logic [31:0] X, BUSA, BUSB
);

logic [31:0] A, B, C;

always_comb begin
    unique case (Z1)
        3'd0: BUSA = 32'd10;
        3'd1: BUSA = 32'd100;
        3'd2: BUSA = 32'd1000000;
        3'd5: BUSA = A;
        3'd6: BUSA = B;
        3'd7: BUSA = C;
        default: BUSA = 32'd0;
    endcase
end

logic [31:0] W;
always_comb begin
    unique case (ZUAL)
        2'd0: W = BUSA + BUSB;
        2'd1: W = BUSA * BUSB;
        2'd2: W = (BUSA >= BUSB) ? 32'd1 : 32'd0;
        2'd3: W = BUSA;
        default: W = 32'd0;
    endcase
end

always_ff @(posedge clk or negedge rst_n) begin
    if (!rst_n) begin
        A <= 32'd0;
        B <= 32'd0;
        C <= 32'd0;
    end else begin
        if (LA) A <= BUSB;
        if (LB) B <= BUSB;
        if (LC) C <= BUSB;
    end
end

always_ff @(posedge clk or negedge rst_n) begin
    if (!rst_n)
        BUSB <= 32'd0;
    else if (Lff)
        BUSB <= W;
end

always_ff @(posedge clk or negedge rst_n) begin
    if (!rst_n)
        X <= 32'd0;
    else if (LACC)
        X <= W;
end

endmodule

```


# Exercici 2
```systemVerilog


module process_unit(
    input logic clk, rst_n, start, X,
    output logic LA, LB, LC, LACC, Lff,
    output logic [2:0] Z1,
    output logic [1:0] ZUAL
);

  typedef enum {
    S0, S1, S2, S3, S4, S5, S6, S7
  } State;
  
  State state, next_state;
  

  always_ff @(posedge clk or negedge rst_n) begin
    if (!rst_n)
      state <= S0;
    else
      state <= next_state;
  end

  always_comb begin
    LA = 1'b0;
    LB = 1'b0;
    LC = 1'b0;
    LACC = 1'b0;
    Lff = 1'b0;
    Z1 = 3'b000;
    ZUAL = 2'b00;
    
    case (state)
      S0: 
        if start
          next_state = S1;
        else
          next_state = S0;
      
      S1: begin
        LACC = 1'b1;
        Z1 = 3'b111;
        ZUAL = 2'b11;
        next_state = S2;
      end
      
      S2: begin
        Lff = 1'b1;
        Z1 = 3'b010;
        ZUAL = 2'b10;
        next_state = S3;
      end
      
      S3:
        if X == 1'b0
          next_state = S0;
        else
          next_state = S4;
      
      S4: begin
        LACC = 1'b1;
        Z1 = 3'b110; 
        ZUAL = 2'b00;
        next_state = S5;
      end
      
      S5: begin
        LACC = 1'b1;
        Z1 = 3'b000; 
        ZUAL = 2'b00;
        next_state = S6;
      end
      
      S6: begin
        LACC = 1'b1;
        Z1 = 3'b001; 
        ZUAL = 2'b01;
        next_state = S7;
      end
      
      S7: begin
        LC = 1'b1;
        Z1 = 3'b000; 
        next_state = S2;
      end
      
      default: next_state = S0;
    endcase
  end

endmodule

```