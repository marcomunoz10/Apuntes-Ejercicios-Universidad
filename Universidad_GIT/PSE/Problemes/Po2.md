![[Pasted image 20250228104946.png]]


```VHDL
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity COUNTER is
	port(
		clk: IN std_logic;
		reset: IN std_logic; -- senyal asíncron
		load: IN std_logic;
		dataIn: IN std_logic_vector (3 downto 0);
		dataOut: OUT std_logic (3 downto 0);
	);
end entity

architecture behavioral of COUNTER is
SIGNAL counter: std_logic_vector(3 downto 0);
begin 
	process(clk, reset)
		begin
			if reset = '1' then
				counter <= '1111';
			elsif rising_edge(clk) then
				if load = '1' then
					counter <= dataIn;
				else
	end process;

architecture Behavioral of mux4x1 is
begin
 process (D0, D1, D2, D3, S)
 begin
 case S is
 when "00" => Y <= D0;
 when "01" => Y <= D1;
 when "10" => Y <= D2;
 when "11" => Y <= D3;
 when others => Y <= '0';
 end case;
 end process;
end Behavioral;


```