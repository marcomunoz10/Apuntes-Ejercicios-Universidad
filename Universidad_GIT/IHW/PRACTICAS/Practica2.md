
Página 11
```systemverilog


module vga_controller (

	// Declaración de las entradas y salidas del módulo.
    input  logic pixel_clk,
    input  logic reset_n,
    output logic h_sync,
    output logic v_sync,
    output logic disp_ena,
    output logic [10:0] row,
    output logic [11:0] column,
    output logic n_blank,
    output logic n_sync
);


//Definición de los parámetros locales h_period y v_period
//h_period ha de valer la duración total del periodo horizontal expresado en cíclos del píxel clock

    parameter int h_pixels = 1024;
    parameter int h_fp     = 24;
    parameter int h_pulse  = 136;
    parameter int h_bp     = 160;
    
    parameter int v_pixels = 768;
    parameter int v_fp     = 3;
    parameter int v_pulse  = 6;
    parameter int v_bp     = 29;
    
    parameter int h_pol    = 0;
    parameter int v_pol    = 0;
    
    parameter int h_period = h_pixels + h_fp + h_pulse + h_bp;
    parameter int v_period = v_pixels + v_fp + v_pulse + v_bp;
    
// Descripción del contador horizontal conformado por
// un sumador, un comparador, un multiplexor y un registro de 12 bits
always_ff @(posedge pixel_clk or negedge reset_n) begin
        if (!reset_n)
            h_count <= 0;
        else if (!h_eoc)
            h_count <= 0;
        else
            h_count <= h_count + 1;
    end
    assign h_eoc = (h_count == h_period - 1);

// Descripción del contador vertical conformado por
// un sumador, un comparador, un multiplexor y un registro con enable de 11 bits
always_ff @(posedge pixel_clk or negedge reset_n) begin
        if (!reset_n)
            v_count <= 0;
        else if (h_eoc) begin
            if (v_count == v_period - 1)
                v_count <= 0;
            else
                v_count <= v_count + 1;
        end
    end

// Descripción del subcircuito que genera la señal h_sync conformado por
// dos comparadores, una OR, un multiplexor y un flip-flop
always_ff @(posedge pixel_clk or negedge reset_n) begin
        if (!reset_n)
            h_sync <= h_pol;
        else if ((h_count >= (h_pixels + h_fp + h_pulse)) || (h_count < (h_pixels + h_fp)))
            h_sync <= ~h_pol;
        else
            h_sync <= h_pol;
    end

// Descripción del subcircuito que genera la señal v_sync conformado por
// dos comparadores, una OR, un multiplexor y un flip-flop
always_ff @(posedge pixel_clk or negedge reset_n) begin
	if(!reset_n)
		v_sync <= v_pol;
	else if((v_count >= (v_pixels + v_fp + v_pulse)) || (v_count < (v_pixels + v_fp)))
		v_sync <= ~v_pol;
	else
		v_sync <= v_pol;


// Descripción del subcircuito que genera la señal row conformado por
// un comparador y un registro de 11 bits
always_ff @(posedge pixel_clk or negedge reset_n) begin
        if (!reset_n)
            row <= 0;
        else if (v_count < v_pixels)
            row <= v_count;
    end

// Descripción del subcircuito que genera la señal column conformado por
// un comparador y un registro de 12 bits
always_ff @(posedge pixel_clk or negedge reset_n) begin
        if (!reset_n)
            column <= 0;
        else if (h_count < h_pixels)
            column <= h_count;
    end

// Descripcion del subcircuito que genera la senal disp_ena conformado por
// dos comparadores, una AND y un flip-flop
always_ff @(posedge pixel_clk or negedge reset_n) begin
        if (!reset_n)
            disp_ena <= 0;
        else
            disp_ena <= (h_count < h_pixels) && (v_count < v_pixels);
    end

// Descripción de las señales de salida constantes n_blank (=1) y n_sync (=0)

assign n_blank = 1;
assign n_sync = 0;
    

endmodule
```




Exercici2:
```systemverilog
module image_source
#(
    parameter int PIXEL_X = 400,  // Anchura en píxeles del rectángulo azul
    parameter int PIXEL_Y = 250   // Altura en píxeles del rectángulo azul
)
(
    input logic [11:0] column,  // Entrada de columna (12 bits)
    input logic [10:0] row,     // Entrada de fila (11 bits)
    input logic disp_ena,       // Señal de habilitación de pantalla
    output logic [9:0] red,     // Salida del color rojo (10 bits)
    output logic [9:0] green,   // Salida del color verde (10 bits)
    output logic [9:0] blue     // Salida del color azul (10 bits)
);

    // Descripción del subcircuito que genera la señal
    // que controla los tres primeros multiplexores
    logic in_rect;
    assign in_rect = (column < PIXEL_X) && (row < PIXEL_Y);

    // Descripción del subcircuito que genera la salida red[9:0]
    // conformado por dos multiplexores
    always_comb begin
        if (disp_ena && in_rect) begin
            red = 10'd0;
        end else begin
            red = 10'd1023;
        end
    end

    // Descripción del subcircuito que genera la salida green [9:0]
    // conformado por dos multiplexores
    always_comb begin
        if (disp_ena && in_rect) begin
            green = 10'd0;
        end else begin
            green = 10'd1023;
        end
    end

    // Descripción del subcircuito que genera la salida blue [9:0]
    // conformado por dos multiplexores
    always_comb begin
        if (disp_ena && in_rect) begin
            blue = 10'd1023;
        end else begin
            blue = 10'd0;
        end
    end

endmodule

```