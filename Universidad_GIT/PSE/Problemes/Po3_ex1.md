
## Design

```VHDL
library ieee;
use IEEE.std_logic_1164.all;

entity Counter is
    port ( clk        : in std_logic;
           AsynResetn : in std_logic;
           Load       : in std_logic;
           Datain     : in std_logic_vector(2 downto 0);
           Count      : out std_logic_vector(2 downto 0)
         );
end Counter;

architecture behavioral of Counter is
    signal Count_reg : std_logic_vector(2 downto 0);  
begin

    process (clk, AsynResetn)
    begin
        if (AsynResetn = '0') then
            Count_reg <= "000";
        elsif rising_edge(clk) then
            if (Load = '1') then
                Count_reg <= Datain;
            else
                Count_reg <= Count_reg(1 downto 0) & not Count_reg(2); 
            end if;
        end if;
    end process;

    Count <= Count_reg;

end behavioral;

```

# Testbench
```VHDL
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity tb_Counter is
end tb_Counter;

architecture test of tb_Counter is

    signal tb_clk, tb_AsynResetn, tb_Load : std_logic;
    signal tb_Datain : std_logic_vector (2 downto 0);
    signal tb_Count : std_logic_vector (2 downto 0);

begin

    uut: entity work.Counter
    port map (clk => tb_clk, AsynResetn => tb_AsynResetn, Load => tb_Load, Datain => tb_Datain, Count => tb_Count);

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
        
        tb_Datain <= "111";
 		tb_Load <= '0';
        tb_AsynResetn <= '0';
        
        
        wait for 10 ns; tb_AsynResetn <= '1';
        tb_Load <= '1';
        wait for 10 ns;
        
        tb_Load <= '0';
        tb_Datain <= "001";
        wait for 20 ns;
       
        tb_Load <= '1'; wait for 10 ns;
        tb_Load <= '0'; 
        
        wait for 20 ns;
        tb_AsynResetn <= '0';
        wait for 10 ns; tb_AsynResetn <= '1';

        wait;
    end process;

end test;



```

