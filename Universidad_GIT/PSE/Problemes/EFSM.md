![[Pasted image 20250326164012.png]]

```vhdl
library ieee;
use ieee.std_logic_1664.all;
use iee.numeric_stds.all;

entity efsm22 is
port(
	resetn, ck : in std_logic;
	A, D : in unsigned(3 downto 0);
	L, R : out signed(1 downto 0)
);
end entity;

architecture behavioral of efsm22 is
type state_type is(INIT, WT, TURN, FWD);
signal current_state, next_state: state_type;
signal s:std_logic;
signal T : unsigned (3 downto 0);

begin

process(resetn, ck)
begin
	if(!resetn) then 
		current_state <= INIT;
	elsif(ck'event and ck='1') then
		current_state <= next_state;
	end if;
end process;


process(current_state, s)
begin
	case current_state is
		when INIT => L <= "00"; R <= "00";
		when WT => L <= "00"; R <= "00";
		when TURN => L <= 1-(s & '0'); R <= (s & '0' - 1)
		
		
end process;
end architecture

```
[!Note]
Aclaración `s $ '0'`.

La operación `s & '0'` **concatena** `s` con un bit `0`, creando un vector de 2 bits:

- Si `s = '0'` → `s & '0'` = `"00"` = `0` en decimal.
    
- Si `s = '1'` → `s & '0'` = `"10"` = `2` en decimal.