# **FPGA--abric-Design-and-Architecture**
This repository contains all the information studied and created during the FPGA - Fabric, Design and Architecture workshop. It is primarily focused on a complete FPGA flow using the maximum open-source tools.
# Table of Contents
[FPGA--abric-Design-and-Architecture](#FPGA--abric-Design-and-Architecture)




# **Day 1 - Exploring FPGA Basics and Vivado**
## FPGA Architecture




## Counter Example in Vivado
A 4-bit up counter is being used for exploring the Vivado tool and OpenFPGA. 
Below mentioned the RTL for the counter modules that is being used.
### VERILOG Codes

```verilog
`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 23.03.2022 00:47:56
// Design Name: 
// Module Name: counter
// Project Name: 
// Target Devices: 
// Tool Versions: 
// Description: 
// 
// Dependencies: 
// 
// Revision:
// Revision 0.01 - File Created
// Additional Comments:
// 
//////////////////////////////////////////////////////////////////////////////////


module counter(clk,reset,count);
input clk,reset;
output reg [3:0] count = 4'b0000;
reg [25:0] count_reg;
reg clk_div = 1'b0;

always @ (posedge clk)
begin
if (reset)
    begin
        clk_div <= 1'b0;
        count_reg <= 26'd0;
     end
else
    begin
        count_reg <= count_reg + 1;
        if (count_reg == 26'h33) // for synthesis
    //   if (count_reg == 26'd12) // for simulation
        begin
            clk_div <= ~ clk_div;
            count_reg <= 26'd0;
        end
    end
 end
  
 always @ (posedge clk_div)
 begin
 if (reset)
 begin
    count <= 4'b0000;
 end
 else
 begin
    count <= count + 1;
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

## SIMULATION
![Screenshot (2041)](https://user-images.githubusercontent.com/120498080/207792232-9b68120f-8b85-4007-a252-f2f653b11717.png)
## RTL ANALYSIS
### Elaborate Design
It is going to bind few module and the modulate hierarchi and it will also enstablish certain net connectivities.
It is usually done before Synthesis

#### Scheamatic
- To see how the designes are actually getting mapped.

![Screenshot (2043)](https://user-images.githubusercontent.com/120498080/207792466-697fca77-ced7-4c0f-b1da-4283731ff720.png)

### I/O Planning
- To obsreve the differnet pins of FPGA
![Screenshot (2044)](https://user-images.githubusercontent.com/120498080/207792629-8a565a89-b586-48dc-9be4-0dd4756585db.png)

### Mapping I/O Ports to the pins of the FPGA
- If we do not have access to FPGA board then we can use the scheamitic of basis 3 and map the pins accordingle
#### Sceheamitic of basis 3
![Screenshot (2046)](https://user-images.githubusercontent.com/120498080/207820032-b6efb987-a4ae-45eb-af8a-756cf998eaf4.png)
#### Mapping in constarints.edx file 
- We set Vcc as 3.3V (I/O Std - LVCMOS33) for all input and output pins. 
- Then by setting the inouts and outouts we generate the constarints.edx file.
![Screenshot (2047)](https://user-images.githubusercontent.com/120498080/207792790-0b3e7b29-8ea9-4cdb-8593-18b314ab9b75.png)
#### Theoty Slack 
Do if from lec 4
## SYNTHESIS & IMPLEMANTATION
- Clock frequency is set as 100MHz (from Constraints Wizard)

#### Report Timing Summary
![Screenshot (2068)](https://user-images.githubusercontent.com/120498080/207813932-5b9ffb95-2197-4598-a2ae-e8f5f9a9e220.png)
#### Set of Paths of Timing Report 
![Screenshot (2073)](https://user-images.githubusercontent.com/120498080/207833329-2a05d4fc-fcd4-4984-b865-ecf12f37cce0.png)
#### Path report if Path1 in Paths Timing Report 
![Screenshot (2074)](https://user-images.githubusercontent.com/120498080/207833693-368619f4-b532-4f29-a548-1e18c5a0f546.png)
![Screenshot (2075)](https://user-images.githubusercontent.com/120498080/207834358-a9078258-db6d-4ff8-9b60-e7fadf99afa4.png)
![Screenshot (2076)](https://user-images.githubusercontent.com/120498080/207834391-bacc2dda-c8e7-478a-a2e8-cc4ddb944bcc.png)
 
#### Schematic
- It shows the synthesized netlist of the schematic
![Screenshot (2069)](https://user-images.githubusercontent.com/120498080/207824737-cfb319d1-aaba-44fe-9699-2e830f28b627.png)
#### Power Report
![Screenshot (2070)](https://user-images.githubusercontent.com/120498080/207825343-2fa69e41-a876-4a59-8982-9898858ec658.png)

#### Report Utilazation
- It basic aly tells about the area report 
- e.g - it shows us how many resources has been utilized in the FPGA
![Screenshot (2071)](https://user-images.githubusercontent.com/120498080/207827717-1e2d4d97-c7d2-49d3-8d4b-9eeea1413900.png)



## PROGRAM AND DEBUG
### Generate Bitstream
- A series of bytes is known as a bytestream. Since a byte is often an 8-bit quantity, the term "octet stream" is occasionally used interchangeably. There is no specific and direct translation between bytestreams and bitstreams because one octet can be encoded as a series of 8 bits in a variety of different ways (see bit numbering).
- Here we locally connect the FPGA Basis3 board into out pc, program the board and test the output by adjusting reset switch  
## VIO - Virtual Input/Output 






# **Day 2 - Exploring OpenFPGA, VPR and VTR**
## Introduction To OpenFPGA
from  1 to 3

## VPR - Versatile Place and Route

## VTR -Verilog to Routing 

## Example 1 : VPR on a Pre-Synthesized Circuit

### Observe the result files

### Visualize(GUI) Circuit Implemantation

## Example 2 :Run the entire VTR flow automatically 
Implement our own circuit (blink.v and counter.v) on a pre-existing
FPGA architecture Earch.xml (VTR_ROOT/vtr_flow/arch - Use an automated approach (Odin II and ABC are automatically run)
Perform timing simulation on the generated fabric

[VTR Reference](https://docs.verilogtorouting.org/en/latest/vpr/)
### Input to this VPS is:
-  Technology mapped netlist of a Design (in form of **blif** file)
-  FPGA Architecture discription file (in form of **Earch.xml** format)
### VPR tool will go through following steps. 
#### VPR Packing 
- It is going to combine all the primitive netlist blocks(e.g. - all the LUTs,FF,etc which are the part of the design) 
- These are going to be packen in CLB(Complex Logic Blocks).
- The output of this is a .net file
#### VPR Placement 
- The CLBs will get placed in the FPGA Grid.
- The output of this is a .place file, which is going to contain the locations of where these CLBs are placed. 
#### VPR Route
- It is going connect all of these CLBs into ths FPGA grid.
- The output of this is a .route file, which is going to contain all the route information.
#### VPR Aalysis
- It dose the analysis in terms of Area, Timing and Power.
- It is also going to output a Post-Implemantation Netlist( it will give information about resource usage, number of block pipes and wires used, timimg in terms of critical path delay and timimg path and also poer usage be each of these blocks)

#### Command to run VPR
```
$VTR_ROOT/vpr/vpr\
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml\
$VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blf\-- route_chan_width 100
```
# Day 3 - RISCV Core Programming Using Vivado
# Day 4 - Introduction To SOFA FPGA Fabric
# Day 5 - RISCV Core on Custom SOFA Fabric



# References
- [VLSI System Design](https://www.vlsisystemdesign.com/ip/)
- https://docs.verilogtorouting.org/en/latest/vtr/cad_flow/

# Acknowledgement
- [Kunal Ghosh](https://github.com/kunalg123) Co-founder of VLSI System Design (VSD) Corp. Pvt. Ltd.
- [Nanditha Rao](https://github.com/nandithaec) Assistant Professor at International Institute of Information Technology – Bangalore
