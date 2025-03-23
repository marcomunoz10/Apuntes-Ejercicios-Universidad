
# Design

```VHDL
library ieee;
use IEEE.std_logic_1164.all;

entity SM2 is
 port ( clk : in std_logic;
 reset : in std_logic;
 input : in std_logic;
 output : out std_logic_vector (2 downto 0)
 );
end SM2;

architecture moore of SM2 is
	 type state_type is (E0,E1,E2,E3);
	 signal current_s,next_s: state_type;
	 
	 begin
	 process (clk,reset)
	 begin
		 if (reset='1') then
			 current_s <= E0;
		 elsif (rising_edge(clk)) then
			 current_s <= next_s;
		 end if;
	 end process;
	 
	 process (current_s,input)
	 begin
		 case current_s is
			 when E0 =>
			 if(input ='0') then
				 next_s <= E2;
			 elsif (input = '1') then
				 next_s <= E1;
			 end if;
		
			when E1 =>
			 if(input ='0') then
				 next_s <= E1;
			 elsif (input = '1') then
				 next_s <= E2;
			 end if;
			 
			when E2 =>
			 if(input ='0') then
				 next_s <= E3;
			 elsif (input = '1') then
				 next_s <= E0;
			 end if;
		
			when E3 =>
			 if(input ='0') then
				 next_s <= E1;
			 elsif (input = '1') then
				 next_s <= E1;
			 end if;
		end case;
	end process;
	
	process(current_s)
	begin
		case current_s is
			when E0 => output <= "010";
			when E1 => output <= "101";
			when E2 => output <= "001";
			when E3 => output <= "110";
		end case;
	end process;

end architecture;
	
```

# Testbench

```VHDl
library ieee;
use IEEE.std_logic_1164.all;

entity tb_SM2 is
end tb_SM2;

architecture test of tb_SM2 is

    signal tb_clk, tb_reset, tb_input : std_logic;
	signal tb_output : std_logic_vector(2 downto 0);
	begin

	    uut: entity work.SM2
	    port map (clk => tb_clk, reset => tb_reset, input => tb_input, output => tb_output);

	process
    begin
        while now < 200 ns loop
            tb_clk <= '0'; wait for 5 ns;
            tb_clk <= '1'; wait for 5 ns;
        end loop;
        wait;
    end process;

	process
	begin
    	tb_reset <= '1'; wait for 10 ns; tb_reset <= '0'; 
        
        tb_input <= '0'; wait for 10 ns;
		tb_input <= '1'; wait for 10 ns;
		tb_input <= '0'; wait for 10 ns;
		tb_input <= '1'; wait for 10 ns;
		tb_input <= '1'; wait for 10 ns;
        
        tb_reset <= '1'; wait for 10 ns; tb_reset <= '0';
        
		tb_input <= '0'; wait for 10 ns;
		tb_input <= '1'; wait for 10 ns;
        tb_input <= '0'; wait for 10 ns;
        tb_input <= '1'; wait for 10 ns;
        tb_input <= '1'; wait for 10 ns;
		tb_input <= '0'; wait for 10 ns;
		tb_input <= '0'; wait for 10 ns;
		tb_input <= '1'; wait for 10 ns;
		tb_input <= '1'; wait for 10 ns;
        
        
        tb_reset <= '1'; wait for 10 ns; tb_reset <= '0';
        tb_input <= '0'; wait for 10 ns;
		tb_input <= '1'; wait for 10 ns;
		tb_input <= '0'; wait for 10 ns;
        
	wait;
	end process;

end test;

```