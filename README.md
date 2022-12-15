# FPGA--abric-Design-and-Architecture
This repository contains all the information studied and created during the FPGA - Fabric, Design and Architecture workshop. It is primarily focused on a complete FPGA flow using the maximum open-source tools.
# Table of Contents
[Testbench](#VERILOG testbench)










## Counter Example in Vivado
A 4-bit up counter is being used for exploring the Vivado tool and OpenFPGA. Below mentioned the RTL for the counter modules that is being used
### VERILOG Codes

```
`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////

// Description: 4 bit counter with source clock (100MHz) division.

/*
////////////4 bit counter block///////////////////
always @(posedge clk)
begin
if(rst)
begin
counter_out<=4'b0000;
//div_clk <= 1'b0;
end
else
begin
counter_out<= counter_out+1;
end
end
endmodule 
*/

//////////////////////////////////////////////////////////////////////////////////
module counter_clk_div(clk,rst,counter_out);
input clk,rst;
reg div_clk;
reg [25:0] delay_count;
output reg [3:0] counter_out;

//////////clock division block////////////////////


always @(posedge clk)
begin

if(rst)
begin
delay_count<=26'd0;
//counter_out<=4'b0000;
div_clk <= 1'b0; //initialise div_clk


//uncomment this line while running just the div clock counter for simulation purpose
counter_out<=4'b0000;
end
else

//uncomment this line while running just the div clock counter for simulation purpose
if(delay_count==26'd212)

//comment this line while running just the div clock counter for simulation purpose
//if(delay_count==26'd32112212)
begin
delay_count<=26'd0; //reset upon reaching the max value
div_clk <= ~div_clk;  //generating a slow clock
end
else
begin
delay_count<=delay_count+1;
end
end


/////////////4 bit counter block///////////////////
always @(posedge div_clk)
begin

if(rst)
begin
counter_out<=4'b0000;
end
else
begin
counter_out<= counter_out+1;
end
end

endmodule 

```
### VERILOG testbench
```
`timescale 1ns / 1ps

module test_counter();
reg clk, reset;
wire [3:0] out;

//create an instance of the design
counter_clk_div dut(clk, reset, out);  

initial begin

//note that these statements are sequential.. execute one after the other 

//$dumpfile ("count.vcd"); 
//$dumpvars(0,upcounter_testbench);

clk=0;  //at time=0

reset=1;//at time=0

#20; //delay 20 units
reset=0; //after 20 units of time, reset becomes 0


end


always 
#5 clk=~clk;  // toggle or negate the clk input every 5 units of time


endmodule 
```


![Screenshot (2041)](https://user-images.githubusercontent.com/120498080/207792232-9b68120f-8b85-4007-a252-f2f653b11717.png)


![Screenshot (2043)](https://user-images.githubusercontent.com/120498080/207792466-697fca77-ced7-4c0f-b1da-4283731ff720.png)


![Screenshot (2044)](https://user-images.githubusercontent.com/120498080/207792629-8a565a89-b586-48dc-9be4-0dd4756585db.png)
![Screenshot (2047)](https://user-images.githubusercontent.com/120498080/207792790-0b3e7b29-8ea9-4cdb-8593-18b314ab9b75.png)
![Screenshot (2048)](https://user-images.githubusercontent.com/120498080/207792808-03244b4b-04aa-47d1-884c-3388fd819f25.png)




