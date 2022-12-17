# **FPGA--abric-Design-and-Architecture**
This repository contains all the information studied and created during the FPGA - Fabric, Design and Architecture workshop. It is primarily focused on a complete FPGA flow using the maximum open-source tools.
# Table of Contents
[FPGA--abric-Design-and-Architecture](#FPGA--abric-Design-and-Architecture)




# **Day 1 - Exploring FPGA Basics and Vivado**
## FPGA Architecture




## Counter Example in Vivado
A 4-bit up counter is being used for exploring the Vivado tool and OpenFPGA. 
Below mentioned the RTL for the counter modules that is being used.

- Linux codes to download GitHub file from link `git clone https://github.com/nandithaec/fpga_workshop_collaterals.git`

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
```verilog
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
[VPR Reference](https://docs.verilogtorouting.org/en/latest/vpr/)

## VTR -Verilog to Routing 
[VTR Reference](https://docs.verilogtorouting.org/en/latest/quickstart/)

## Example 1 : VPR on a Pre-Synthesized Circuit

### Observe the result files

### Visualize(GUI) Circuit Implemantation

Implement our own circuit (blink.v and counter.v) on a pre-existing
FPGA architecture Earch.xml (VTR_ROOT/vtr_flow/arch - Use an automated approach (Odin II and ABC are automatically run)
Perform timing simulation on the generated fabric


### Input to this VPS is:
-  Technology mapped netlist of a Design (in form of **blif** file)
-  FPGA Architecture discription file (in form of **Earch.xml** format)
### VPR tool will go through following steps. 
#### 1. VPR Packing 
- It is going to combine all the primitive netlist blocks(e.g. - all the LUTs,FF,etc which are the part of the design) 
- These are going to be packen in CLB(Complex Logic Blocks).
- The output of this is a .net file
#### 2. VPR Placement 
- The CLBs will get placed in the FPGA Grid.
- The output of this is a .place file, which is going to contain the locations of where these CLBs are placed. 
#### 3. VPR Route
- It is going connect all of these CLBs into ths FPGA grid.
- The output of this is a .route file, which is going to contain all the route information.
#### 4. VPR Aalysis
- It dose the analysis in terms of Area, Timing and Power.
- It is also going to output a Post-Implemantation Netlist( it will give information about resource usage, number of block pipes and wires used, timimg in terms of critical path delay and timimg path and also poer usage be each of these blocks)





- Location of blif file : 
> /home/kunalg123/Desktop/vtr-verilog-to-routing/vtr_flow/benchmarks/blif/tseng.blif
  
- Location of Earch.xml file :
>/home/kunalg123/Desktop/vtr-verilog-to-routing/vtr_flow/arch/timing/Earch.xml

#### Example xml file explanation :
![Screenshot (2080)](https://user-images.githubusercontent.com/120498080/207940470-314389f2-05d8-4886-84b3-074b9638554b.png)

- Command to open the above blif and Earch.xml location `cd $VTR_ROOT` |

#### Move to our home directory
> cd ~

#### Make a working directory
> mkdir -p vtr_work/quickstart/vpr_tseng

#### Move into the working directory
> cd ~/vtr_work/quickstart/vpr_tseng

#### Working Location :
>is22mtech14002@fpga-workshop-02:~/vtr_work/quickstart/vpr_tseng$

#### Command to run VPR
- Use this command in the working location
```
$VTR_ROOT/vpr/vpr $VTR_ROOT/vtr_flow/arch/timing/EArch.xml $VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blif --route_chan_width 100 --disp on
```
$VTR_ROOT/vpr/vpr\                    // invoking vpr which is at VTR_ROOT (where vtr has been installed in the cloud)                                       
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml\     // First input is FPGA Architecture Discription File (EArch.xml)
$VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blf\
-- route_chan_width 100 \   // use a benchmark file that is already beign converter into blf format
-- disp on    // to open GUI

#### GUI
![Screenshot (2081)](https://user-images.githubusercontent.com/120498080/207940492-b3c8ddb5-da7d-4026-86e0-b2f7bba9bce7.png)
#### Structure of FPGA Architecture
![Screenshot (2088)](https://user-images.githubusercontent.com/120498080/208081790-279b1699-d010-4a70-a94f-295040f1519a.png)
#### Congestion Architecture
- It shows which area is how mech Congusted.
![Screenshot (2089)](https://user-images.githubusercontent.com/120498080/208081834-0f92663a-bc7e-4757-86eb-24f96f55c9a5.png)

- There are also differnet views available of this FPGA Architecture


- Finally .net .place .route and .log files are generate in same working directory `/home/kunalg123/Desktop/vtr-verilog-to-routing/`
- It will also generate report_timing.setup.rpt , report_timing.hold.rpt, packing_pin_util.rpt, etc files. in the same working directory.


- Command to search .rpt file in working directory `ls *.rpt`



#### tseng.sdc file
- To get the correct timing report we need to  set the clock and for that we need to set the contrains file (.sdc file)
```
create_clock -period 10 -name pclk     
set_input_delay -clock pclk -max 0 [get_ports {*}]  
set_output_delay -clock pclk -max 0 [get_ports {*}]  
```
create_clock -period 10 -name pclk     //Created a clock with time period of 10nsec with name plck (we can find it from timing report)
set_input_delay -clock pclk -max 0 [get_ports {*}]   //Set input delay to zero
set_output_delay -clock pclk -max 0 [get_ports ("}]   //Set output delay to zero

### Now to add this clock 
- First go back to the same working directory and append the sdc option.
```
$VTR_ROOT/vpr/vpr $VTR_ROOT/vtr_flow/arch/timing/EArch.xml $VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blif --route_chan_width 100 --sdc_file /home/is22mtech14002/LAB1/tseng.sdc  
```
[Reference for VPR Command Line Options](https://docs.verilogtorouting.org/en/latest/vpr/command_line_usage/)

#### Setup Slag Path
![Screenshot (2087)](https://user-images.githubusercontent.com/120498080/208078972-dc84eab8-f2e4-45bd-b51f-cafacb8b42fc.png)
- Finally we got a Setup Slag Path as 8.507nsec


## Example 2 :Run the entire VTR flow automatically 
- In VTR flow we will start with HDL going through Odin II then ABC then finally the VPR flow (as done is example 1)
- There are two ways of running VTR
1. Manually Running the VTR Flow
- We have to invoke Odin II manually
- Then we have to do the tecjnology mapping with ABC
- Then mannualy implement the circuit using VPR
2  . Automatically Running the VTR Flow

- Here we will Automatically Run the VTR Flow
We will be invocing python script presnet at 
>$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py

#### Codes to run VTR tool
```

$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py     /home/is22mtech14002/Desktop/fpga_workshop_collaterals/Day2/counter_files/counter.v   $VTR_ROOT/vtr_flow/arch/timing/EArch.xml   -temp_dir .  --route_chan_width 100
```

- Then run the commands in working direcory to generate .blif file 

 $VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \         //Invocing Python script .py
    $VTR_ROOT/doc/src/quickstart/counter.v \            //Inputs the counter.v file
    $VTR_ROOT/vtr_flow/arch/timing/EArch.xml \        //Architecture onto which we wnt to map the counter.v file
    -temp_dir . \                                     //Local working Directory
    --route_chan_width 100                             // Rounting Channel width for the Architecture


#### Codes of counter.v file
```verilog
/*Important: Once you run ./a.out, it will keep running infinitely, because it is in an always block. You need to hit Ctrl +Z to stop it, else, the vcd will become a large file and will never end.

*/

module up_counter    (
out     ,  // Output of the counter
enable  ,  // enable for counter
clk     ,  // clock Input
reset      // reset Input
);

output [3:0] out;
//you can alternately write this as output reg [7:0] out;
input enable, clk, reset;
//------------Internal Variables--------
reg [3:0] out; 



always @(posedge clk)
if (reset) begin //reset ==1
  out = 4'b0 ;
end 
else if (enable) begin //reset =0
  out = out + 1;
end

endmodule 
```
- Commands to invoke GUI analysis step
```
$VTR_ROOT/vpr/vpr $VTR_ROOT/vtr_flow/arch/timing/EArch.xml  /home/is22mtech14002/vtr_work/quickstart/vpr_tseng/counter.pre-vpr.blif  --route_chan_width 100 --analysis --disp on
```

--analysis      // to run GUI from analysis step onwards

- Commands to invoke GUI with entire VPR flow
```
$VTR_ROOT/vpr/vpr $VTR_ROOT/vtr_flow/arch/timing/EArch.xml  /home/is22mtech14002/vtr_work/quickstart/vpr_tseng/counter.pre-vpr.blif  --route_chan_width 100 --disp on
```

### GUI
![Screenshot (2090)](https://user-images.githubusercontent.com/120498080/208226847-1cfb86ed-e9fa-42fd-9afc-f4fdce2b5814.png)

- So now it will complete the entire VPR flow




## Generation of the Post-Implementation Netlist
- Now we need to generete a Post-Implementation Netlist from VPR
[Generation of the Post-Implementation Netlist](https://docs.verilogtorouting.org/en/latest/tutorials/timing_simulation/)

```
$VTR_ROOT/vpr/vpr $VTR_ROOT/vtr_flow/arch/timing/EArch.xml  /home/is22mtech14002/vtr_work/quickstart/vpr_tseng/counter.pre-vpr.blif  --route_chan_width 100 --gen_post_synthesis_netlist on
```
```
$VTR_ROOT/vpr/vpr $VTR_ROOT/vtr_flow/arch/timing/EArch.xml                   //Run VPR with the Architecture file (.xml file)
/home/is22mtech14002/vtr_work/quickstart/vpr_tseng/counter.pre-vpr.blif      //Run the .blif file that has been created from the VPR
--route_chan_width 100 
--gen_post_synthesis_netlist on                                              //to genrete post synthesis netlist
```
- So now VPR will generate the  Post-Implementation Netlist (up_counter_post_synthesis.v file) and also the delay file (up_counter_post_synthesis.sdf file)


- Linux Command to check up_counter_post_synthesis.sdf and up_counter_post_synthesis.v file has been generated `ls *.v *.sdf`

/home/kunalg123/Desktop/vtr-verilog-to-routing/vtr_flow/primitives.v
/home/is22mtech14002/vtr_work/quickstart/vpr_tseng/up_counter_post_synthesis.blif
/home/is22mtech14002/vtr_work/quickstart/vpr_tseng/up_counter_post_synthesis.sdf
/home/is22mtech14002/vtr_work/quickstart/vpr_tseng/up_counter_post_synthesis.v

## Now to make this run in VIVADO
- Now create a project in VIVADO and add primitives.v and up_counter_post_synthesis.v as design sources  and upcounter_testbench.v as simulation sources


#### Codes of upcounter_testbench.v file
```verilog

...

```
- **NOTE** It does not matter what FPGA we choose inintially in VIVADI tool because we are noe going to run an FPGA simulation, the up_counter_post_synthesis.v file is specifific to a open FPGA Architecture and to Xilinx particular architecture, but we are going to use this Xilinx tools only particularly for simulation purpose and for synthesis and simulation and so on. 








# Day 3 - RISCV Core Programming Using Vivado
# Day 4 - Introduction To SOFA FPGA Fabric
# Day 5 - RISCV Core on Custom SOFA Fabric















# References
- [VLSI System Design](https://www.vlsisystemdesign.com/ip/)
- https://docs.verilogtorouting.org/en/latest/vtr/cad_flow/
- https://docs.verilogtorouting.org/en/latest/arch/reference/#arch-grid-layout
- [Workshop GitHub Material]( https://github.com/nandithaec/fpga_workshop_collaterals)


# Acknowledgement
- [Kunal Ghosh](https://github.com/kunalg123) Co-founder of VLSI System Design (VSD) Corp. Pvt. Ltd.
- [Nanditha Rao](https://github.com/nandithaec) Assistant Professor at International Institute of Information Technology – Bangalore
