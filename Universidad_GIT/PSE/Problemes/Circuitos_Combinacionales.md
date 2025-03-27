
[Haced una descripción sintetizable de un conversor de BCD a 7 segmentos. El bit A de entrada es el bit de más peso del valor BCD y el bit D el bit de menos peso. Para activar un segmento, la señal de salida correspondiente tiene que ser 1.]
![[Pasted image 20250325191801.png]]



```VHDL
library IEEE;
use IEE.STD_LOGIC_1664.ALL;

entity sevensegment is
	port(Datain in : std_logic_vector(3 downto 0);
	Dataout out : std_logic_vector(6 downto 0) 
	);
end entity;

architecture behavioral of sevensegment is
begin
process(Datain) is
	case Datain is
	when "0000" => Dataout <= "1111110";
	when "0001" => Dataout <= "1110000";
	when "0010" => Dataout <= "0110000";
	...
	end case;
end process;
end architecture;
```


---


[Haz una descripción sintetizable en VHDL de un contador de 16 bits de tipo One Shot, reversible y con señal de habilitación. Las entradas y salidas son las siguientes]

− **resetn**: señal de reset asíncrono y activa a baja. Cuando se activa el reset, el contador se inicia en
cero.
− **ck**: señal de reloj. El flanco de subida es el flanco activo.
− **enable**: Señal síncrono de habilitación. Cuando enable es 1, el contador pasa a un nuevo valor con cada flanco de reloj (cuenta habilitada) y mantiene el valor de lo contrario.
− **up**: señal de un bit que indica la dirección de la cuenta. Cuando es 1, la cuenta es ascendente. En caso contrario es descendente. En ambos casos funciona de forma cíclica.
− **DataOut**: Salida paralela de 16 bits que muestra el valor que tiene el contador en todo momento.
Un contador de tipo One Shot de 4 bits sigue la siguiente secuencia cuando la cuenta es ascendente: 0000 → 0001 → 0010 → 0100 → 1000 → 0000 → 0001 ...
Cuando la cuenta es descendente se sigue el orden inverso:
0000 → 1000 → 0100 → 0010 → 0001 → 0000 → 1000 ..

```vhdl
library IEEE;
use IEE.STD_LOGIC_1664.ALL;

entity exercici1 is
port( resetn, ck, enable, up : in std_logic;
	DataOut : in std_logic_vector (15 downto 0)
);
end entity;

architecture behavioral of exercici1 is
signal reg: std_logic_vector (16 downto 0);
begin
process(ck, resetn)
if reset = '0' then
	reg(16) = '1';
	reg(15 downto 0) <= (OTHERS => '0');
elsif rising_edge(ck) then
	if enable = '1' then
		if up = '1' then
			reg <= reg(15 downto 0) & reg(16);
		else
			reg <= reg(0) & reg(16 downto 1);
		end if;
	end if;
end if;

end process;
DataOut <= reg(15 downto 0);
end architecture;
```


---
[2022]

.[!NOTE].
Los que aparece entre # es que no lo habia puesto y es necesario para la solución correcta

```vhdl

library ieee;
use iee.std_logic_1664.all


entity exercici2 is
	port (
		Da, Dp, V : in std_logic_vector(7 downto 0);
		f, a : out std_logic;
		P : out std_logic_vector(7 downto 0)

	);
end entity;

architecture behavioral of exercici2 is
begin
	process(Da, Dp, V)
		if (((Da+Dp)/2) < "00001111") then
			
			if (Dp < Da) then
				a <= 1;
				f <= 0;
			else
				a <= 0;
				f <= 1,
			end if;
			p <= V;
		else
			P <= 0;
		end if;
	
	end process;
end architecture;
```



---
[2022_2]
[Fes una descripció sintetitzable en VHDL d’un comptador de 16 bits, reversible i amb càrrega paral·lela.]


.[!NOTE].
Este ejercicio es mi primer intento, abajo está la corrección.
```vhdl
library iee;
use iee.std_logic_1664.all
use iee.numeric_std.all

entity exercici22_2 is
	port(
		resetn, ck, dir, load : in std_logic;
		Datain : in std_logic_vector(15 downto 0);
		Dataout : out std_logic_vector (15 downto 0)
	);
end entity

architecture behavioral of exercici22_2 is
signal comptador: unsigned(15 downto 0);
begin
	process(resetn, ck, dir, load)
	begin
		if (!resetn) then
			comptador <= (OTHERS => '0');
		end if;
		elsif (load = '1') then
			comptador <= Datain;
		end if;
		elsif (dir = '1') then
			if(rising_edge_ck = '1') then
				comptador <= comptador + '1';
			end if;
		end if;
		elsif (dir = '0') then
			if(rising_edge_ck = '1') then
				comptador <= comptador - '1';
			end if;
	end process;
	Dataout <= comptador;
end architecture;
```

```vhdl
library iee;
use iee.std_logic_1664.all

entity exercici22_2 is
	port(
		resetn, ck, dir, load : in std_logic;
		Datain : in std_logic_vector(15 downto 0);
		Dataout : out std_logic_vector (15 downto 0)
	);
end entity

architecture behavioral of exercici22_2 is
signal comptador: std_logic_vector(15 downto 0);
begin
	process(resetn, ck, load, Datain)
	begin
		if (!resetn) then
			comptador <= (OTHERS => '0');
		end if;
		elsif(ck'event and ck = '1') then
			if (load = '1') then 
				comptador <= Datain;
			else
				if (dir = '1') then
					comptador <= comptador + '1';
				else
					comptador <= comtpador - '1';
				end if;
			end if;
		end if;
	end process;
	Dataout <= comptador;
end architecture;
```