# code31 
library IEEE;
use IEEE.std_logic_1164.all;-------------------------------------------------------------------------------------------------- 
  entity VHDL_LEDWATER1 is
        port (

                     Clk    : in  STD_LOGIC;             --创建时钟端口，连接开发板PIN23

                     Rst     : in  STD_LOGIC;            --创建复位端口，连接开发板PIN116

                     Output : out BIT_VECTOR(7 downto 0) --创建输出端口，对应8个LED。分别

        --为PIN142-PIN133，要使用移位操作符

             );                                         --其左侧必须为BIT_VECTOR类型

 end VHDL_LEDWATER1;

 --------------------------------------------------------------------------------------------------

 architecture behave of VHDL_LEDWATER1 is

       signal Clk1 : STD_LOGIC;             --建立中间时钟信号
 
 begin

 P1:process(Clk)
 
 variable count : INTEGER range 0 to 25 := 0; --变量初始值不可综合，在仿真中使用，并

variable count1: STD_LOGIC := '1';           --且为便于仿真，这里取到25，当烧写到开
 
        --发板时候，改写为25000000即可           
 
        begin
 
               if(Rst = '0') then
 
                      count := 0;
 
               elsif(Clk'event and Clk = '1') then
 
                      count := count + 1;
 
                    if(count = 25) then --这里使用=,而不是>=,可以防止产生比较器,节省硬件资源

                         count := 0;

                            count1 := not count1;

                     end if;

              end if;
 
              Clk1 <= count1;

end process P1;

 P2:process(Clk1)
 
        variable temp : BIT_VECTOR(7 downto 0) := "11111110";--注意左操作数类型
 
        begin
 
               if(Clk1'event and Clk1 = '1') then
 
                      temp := (temp rol 1);                  
 
               end if;

              Output <= temp;
       end process P2;
 
end architecture;
