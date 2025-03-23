# Design

``` VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity efsmEncrip is
    port( 
        clk, reset: IN std_logic;
        T: IN std_logic_vector(5 downto 0);
        q: IN std_logic;
        z: OUT std_logic;
        g: OUT std_logic
    );
end entity;

architecture moore of efsmEncrip is
type state_type is (INIT, WAIT_S, DEC, GREEN, DELAY, RZ);
signal current_state, next_state: state_type;
signal current_D, next_D: unsigned(5 downto 0);
begin
    updateRegs: process( clk, reset ) -- Clocked process
    begin
        if reset = '0' then
        current_state <= INIT;
        --current_D <= (others => "100011");
        current_D <= to_unsigned(35, current_D'length);
        elsif rising_edge(clk) then
        current_state <= next_state;
        current_D <= next_D;
        end if;
    end process;

    --D <= std_logic_vector(current_D);
    outSIG: process( current_state ) -- Comb. process
    begin
        z <= '0';
        g <= '0';
        if current_state = DEC or current_state = INIT or current_state = RZ then
        z <= '1';
        elsif current_state = GREEN then
	    g <= '1';
        end if;
    end process;

    nextValues: process( q, T, current_state, current_D ) --Comb.
    begin
        -- Default assignments to avoid latches
        next_state <= current_state;
        next_D <= current_D;
        case current_state is
        when INIT => 
            next_state <= WAIT_S;
            next_D <= "100011";
        when WAIT_S => 
            if q then
                next_state <= RZ;
            elsif (not q and ((T < "111100") or not (current_D > "001010"))) then
                next_state <= WAIT_S;
            else 
	            next_state <= DEC;
            end if;
        when DEC => 
            if not q then
                next_state <= WAIT_S;
            else
                next_state <= RZ;
            end if;
            next_D <= current_D - "001010";
        when RZ => 
            next_state <= DELAY;
        when DELAY =>
	        if (T < current_D) then 
		        next_state <= DELAY;
		    else
			    next_state <= GREEN;
			end if;
		when GREEN => 
			--if (T < (current_D + "001111")) then
			--if T < std_logic_vector(current_D + to_unsigned(15, 6)) then
			if T < std_logic_vector(resize(current_D, T'length) + to_unsigned(15, T'length)) then

				next_state <= GREEN;
			else 
				next_state <= INIT;
			end if;
        end case;
    end process;
end architecture moore;

```


# Testbench

```VHDL
library ieee;
use ieee.std_logic_1164.all;

entity efsmEncripTB is
end entity;

architecture estimuli of efsmEncripTB is

    constant cycle      : time := 100 ns;
    constant semicycle  : time := 50 ns;
    constant delay      : time := 10 ns;

    component SM1 is
        port (
            clk    : in std_logic;
            reset : in std_logic;
            T    : in std_logic_vector(5 downto 0);
            q    : in std_logic;
            z : out std_logic;
            g : out std_logic
        );
    end component;

    signal clk_tb     : std_logic;
    signal reset_tb   : std_logic;
    signal T_tb     : std_logic_vector(5 downto 0);
    signal q_tb     : std_logic;
    signal z_tb  : std_logic;
    signal g_tb  : std_logic;
    
    signal stop       : boolean := false;

begin

    -- Component instantiation
    SM1_test : SM1
        port map (
            clk    => clk_tb,
            resetn => reset_tb,
            T    => T_tb,
            q    => q_tb,
            z => z_tb.
            g => g_tb
        );

    -- Clock generation process
    clk_gen : process
    begin
        clk_tb <= '0';
        wait for semicycle;
        clk_tb <= '1';
        wait for semicycle;
        if stop then wait; end if;
    end process;

    -- Testbench process
    test : process
    begin
        -- Initial input values
        T <= '1';  -- 0 ns
        q <= '0';
        
        -- Reset
        reset_tb <= '0'; -- 0 ns
        Output_tb <= '0';
        wait for 100 ns;
        reset_tb <= '1'; -- 100 ns
        wait for 140 ns;
        
        -- State C
        wait until clk_tb'event and clk_tb = '1';
        wait for delay;
        T <= '1';  -- 260 ns
        q <= '0';
        wait for cycle;
        assert (Output_tb = '0')
            report "Sortida estatC incorrecta"
            severity error;
        
        -- Stay in State C
        in1_tb <= '1';  -- 360 ns
        in2_tb <= '0';
        wait for 2 * cycle;
        in1_tb <= '0';  -- 560 ns
        in2_tb <= '0';
        wait for 2 * cycle;
        assert (Output_tb = '0')
            report "Sortida estatC incorrecta"
            severity error;
        
        -- State A
        in1_tb <= '0';  -- 760 ns
        in2_tb <= '1';
        wait for cycle;
        assert (Output_tb = '0')
            report "Sortida estatA incorrecta"
            severity error;
        
        -- State B
        in1_tb <= '0';  -- 860 ns
        in2_tb <= '1';
        wait for cycle;
        assert (Output_tb = '1')
            report "1a Sortida estatB incorrecta"
            severity error;
        
        -- State C
        in1_tb <= '0';  -- 960 ns
        in2_tb <= '0';
        wait for cycle;
        
        -- State A
        in1_tb <= '1';  -- 1060 ns
        in2_tb <= '1';
        wait for cycle;
        
        -- State B
        in1_tb <= '0';  -- 1160 ns
        in2_tb <= '0';
        wait for cycle;
        assert (Output_tb = '1')
            report "2a Sortida estatB incorrecta"
            severity error;
        
        -- Test completion
        report "TEST OK";
        stop <= true;
        wait;
    end process;
end architecture;

```

# Testbench2
```vhdl
library ieee;
use ieee.std_logic_1164.all;

entity tb_efsmEncrip is
end entity;

architecture moore of tb_efsmEncrip is
    -- Constants for timing
    constant cycle : time := 100 ns;
    constant semicycle : time := 50 ns;
    constant delay : time := 10 ns;
    
    -- Component declaration
    component efsmEncrip
        port(
            clk, reset: IN std_logic;
            T: IN std_logic_vector(5 downto 0);
            q: IN std_logic;
            z: OUT std_logic;
            g: OUT std_logic
        );
    end component;
    
    -- Signals for the testbench
    signal clk_tb, reset_tb, q_tb, z_tb, g_tb : std_logic := '0';
    signal T_tb : std_logic_vector(5 downto 0) := (others => '0');
    signal stop : boolean := false;
    
begin
    -- Instantiate the DUT
    efsm_inst: efsmEncrip
        port map(
            clk => clk_tb,
            reset => reset_tb,
            T => T_tb,
            q => q_tb,
            z => z_tb,
            g => g_tb
        );
    
    -- Clock generation process
    clk_gen: process
    begin
        while not stop loop
            clk_tb <= '0';
            wait for semicycle;
            clk_tb <= '1';
            wait for semicycle;
        end loop;
        wait;
    end process;
    
    -- Testbench process
    test: process
    begin
        -- Initial values
        q_tb <= '0';
        reset_tb <= '0';
        T_tb <= (others => '0');
        wait for cycle;
        reset_tb <= '1';
        wait for cycle;
        
        -- Test INIT to WAIT transition
        q_tb <= '1';
        wait for cycle;
        assert (z_tb = '1') report "Error in INIT state" severity error;
        
        -- Test WAIT to DEC transition
        q_tb <= '0';
        wait for 2 * cycle;
        assert (z_tb = '0') report "Error in WAIT state" severity error;
        
        -- Test DEC to RZ transition
        q_tb <= '1';
        wait for cycle;
        assert (z_tb = '1') report "Error in DEC state" severity error;
        
        -- Test RZ to DELAY transition
        wait for cycle;
        assert (z_tb = '1') report "Error in RZ state" severity error;
        
        -- Test DELAY to GREEN transition
        T_tb <= "000101";  -- Simulate elapsed time
        wait for cycle;
        assert (g_tb = '1') report "Error in GREEN state" severity error;
        
        -- Test GREEN to INIT transition
        wait for 15 * cycle;
        assert (g_tb = '0') report "Error in transition to INIT" severity error;
        
        -- End of test
        report "TEST OK";
        stop <= true;
        wait;
    end process;
    
end architecture;

```




# Gemini

```VHDL
library ieee;
use ieee.std_logic_1164.all;
use ieee.numeric_std.all;

entity efsmEncrip_tb is
end entity;

architecture behavioral of efsmEncrip_tb is

    -- Component declaration
    component efsmEncrip is
        port( 
            clk, reset: IN std_logic;
            T: IN std_logic_vector(5 downto 0);
            q: IN std_logic;
            z: OUT std_logic;
            g: OUT std_logic
        );
    end component;

    -- Signals for the component
    signal clk_tb : std_logic := '0';
    signal reset_tb : std_logic := '0';
    signal T_tb : std_logic_vector(5 downto 0) := (others => '0');
    signal q_tb : std_logic := '0';
    signal z_tb : std_logic;
    signal g_tb : std_logic;

    -- Clock period
    constant clk_period : time := 10 ns;

begin

    -- Instantiate the Unit Under Test (UUT)
    uut: efsmEncrip port map (
        clk => clk_tb,
        reset => reset_tb,
        T => T_tb,
        q => q_tb,
        z => z_tb,
        g => g_tb
    );

    -- Clock process
    clk_process :process
    begin
        while true loop
            clk_tb <= '0';
            wait for clk_period/2;
            clk_tb <= '1';
            wait for clk_period/2;
        end loop;
    end process;

    -- Stimulus process
    stim_proc: process
    begin
        -- Reset
        reset_tb <= '1';
        wait for 20 ns;
        reset_tb <= '0';
        wait for 10 ns;

        -- Test case 1:  Press q, observe green after delay
        q_tb <= '1';
        wait for 35 * clk_period; -- Wait for the initial delay (35s)
        q_tb <= '0';
        wait for 15 * clk_period; -- Wait for the green time (15s)
        wait for 10 ns;

        -- Test case 2: Press q again, observe green after delay
        q_tb <= '1';
        wait for 35 * clk_period;
        q_tb <= '0';
        wait for 15 * clk_period;
        wait for 10 ns;

        -- Test case 3:  Wait for 60 seconds, then press q.  Delay should be reduced.
        q_tb <= '0';
        T_tb <= (others => '0');
        wait for 60 * clk_period;
        q_tb <= '1';
        wait for 25 * clk_period; -- Delay should be 25s (35-10)
        q_tb <= '0';
        wait for 15 * clk_period;
        wait for 10 ns;

        -- Test case 4: Wait another 60 seconds, then press q. Delay should be reduced again.
        q_tb <= '0';
        T_tb <= (others => '0');
        wait for 60 * clk_period;
        q_tb <= '1';
        wait for 15 * clk_period; -- Delay should be 15s (25-10)
        q_tb <= '0';
        wait for 15 * clk_period;
        wait for 10 ns;

        -- Test case 5: Wait another 60 seconds, then press q. Delay should be reduced again.
        q_tb <= '0';
        T_tb <= (others => '0');
        wait for 60 * clk_period;
        q_tb <= '1';
        wait for 5 * clk_period; -- Delay should be 5s (15-10)
        q_tb <= '0';
        wait for 15 * clk_period;
        wait for 10 ns;

        -- Test case 6: Wait another 60 seconds, then press q. Delay should remain at 5s.
        q_tb <= '0';
        T_tb <= (others => '0');
        wait for 60 * clk_period;
        q_tb <= '1';
        wait for 5 * clk_period; -- Delay should remain 5s
        q_tb <= '0';
        wait for 15 * clk_period;
        wait for 10 ns;

        wait; -- Stop simulation
    end process;

end architecture;


K
```