# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)

![image](https://github.com/user-attachments/assets/bf644a40-6924-4798-80fd-7d5d5e2f15d7)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)

Testbench :

`timescale 1ns / 1ps

module up_down_counter_tb();

reg clk;
reg reset;
reg up_down;
reg enable;
wire [3:0] count;


up_down_counter uut(
    .clk(clk),
    .reset(reset),
    .up_down(up_down),
    .enable(enable),
    .count(count)
);


initial begin
    clk = 0;
    forever #5 clk = ~clk;  
end


initial begin

    reset = 1;
    up_down = 1;
    enable = 0;
    

    #20 reset = 0;
    

    #10 enable = 1;
    #200;  
    

    up_down = 0;
    #200;  
    

    enable = 0;
    #50;
    

    reset = 1;
    #20 reset = 0;
    

    #50;
    

    $finish;
end


initial begin

    $monitor("Time: %t, Reset: %b, Up/Down: %b, Enable: %b, Count: %b", 
             $time, reset, up_down, enable, count);
end
endmodule

DESIGN :

imescale 1ns / 1ps module up_down_counter( input wire clk,

input wire reset,

input wire up_down,

input wire enable,

output reg [3:0] count );

always @(posedge clk or posedge reset) begin
   
    if (reset) 
    
    begin
    
        count <= 4'b0000;
    
    end

    else if (enable)
    
    begin
    
        if (up_down)
        
        begin
        
            count <= count + 1'b1;
        
        end
        
        else begin
        
            count <= count - 1'b1;
        
        end
    
    end

end

endmodule

◦ SDC (Synopsis Design Constraint) File (.sdc)


![Screenshot 2025-05-09 115510](https://github.com/user-attachments/assets/71961ce6-0dde-4e3f-917f-4c50eecd76e2)


 ### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.


![Screenshot 2025-05-09 120146](https://github.com/user-attachments/assets/6001393e-199f-4bec-a0fe-5182dbdee670)


•	The SDC File must contain the following commands;

create_clock -name clk -period 2 -waveform {0 1} [get_ports "clk"]

set_clock_transition -rise 0.1 [get_clocks "clk"]

set_clock_transition -fall 0.1 [get_clocks "clk"]

set_clock_uncertainty 0.01 [get_ports "clk"]

set_input_delay -max 0.8 [get_ports "rst"] -clock [get_clocks "clk"]

set_output_delay -max 0.8 [get_ports "count"] -clock [get_clocks "clk"]

i→ Creates a Clock named “clk” with Time Period 2ns and On Time from t=0 to t=1.

ii, iii → Sets Clock Rise and Fall time to 100ps.

iv → Sets Clock Uncertainty to 10ps.

v, vi → Sets the maximum limit for I/O port delay to 1ps.

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being
used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.


![Screenshot 2025-05-09 120412](https://github.com/user-attachments/assets/0292e227-5cfb-4957-ba11-bd61c33cac64)


• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :


![image](https://github.com/user-attachments/assets/d3e5547c-26b6-4d09-8bdf-a6f628586956)


#### Area report:


![Screenshot 2025-05-09 120528](https://github.com/user-attachments/assets/e7626099-ffa0-43f3-9aa5-c8acfcfff041)


#### Power Report:


![Screenshot 2025-05-09 120556](https://github.com/user-attachments/assets/060911f1-662d-4e6f-9162-8c51f6f4645c)


#### Timing Report: 


![image](https://github.com/user-attachments/assets/5966998b-bd33-4c0f-afad-af4f72711bb3)


#### Result: 

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.





