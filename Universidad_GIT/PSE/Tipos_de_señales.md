###  **1. Señales que requieren `ck'event`**

Estas señales **se actualizan solo en el flanco del reloj**, es decir, son **registradas**. Por ejemplo, son las salidas de un esquema EFSM que tiene un símbolo como [+].
En el código VHDL, esto se maneja con un **proceso secuencial** como este:

```vhdl
process(resetn, ck)
begin
  if resetn = '0' then
    T <= (others => '0');
  elsif ck'event and ck = '1' then
    case current_state is
      when INIT =>   T <= (others => '0');
      when WT   =>   T <= '0' & A(2 downto 0) + D;
      when TURN =>   T <= T - ("" & c);
      when FWD  =>   T <= T - ("" & c);
    end case;
  end if;
end process;
```

 **¿Por qué necesitan `ck'event`?**

- **T y s son registros (valores almacenados)**.
    
- Solo pueden cambiar su valor en el **flanco de subida del reloj (`ck'event and ck = '1'`)**.
    
- Esto significa que su valor no cambia de inmediato, sino en el siguiente ciclo de reloj.
    

---

###  **2. Señales que NO requieren `ck'event`**

Estas señales **se actualizan inmediatamente** en función de otras señales, sin depender del reloj. Son **combinacionales** y se manejan en un **proceso sin `ck'event`**, como este:

```vhdl
process(current_state, s)
begin
  case current_state is
    when INIT  =>  L <= "00";  R <= "00";
    when WT    =>  L <= "00";  R <= "00";
    when TURN  =>  L <= 1 - (s & '0');  
                   R <= (s & '0') - 1;
    when FWD   =>  L <= "01";  R <= "01";
  end case;
end process;
```

 **¿Por qué NO necesitan `ck'event`?**

- **L y R son señales de salida que afectan directamente a los motores.**
    
- **Son combinacionales**, es decir, su valor se determina **inmediatamente** cuando el estado cambia, sin esperar un ciclo de reloj.
    
- En cada ciclo de reloj, cuando la máquina de estados cambia de estado, **L y R toman su nuevo valor de inmediato**.
    

---
