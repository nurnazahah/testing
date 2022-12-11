# Day 4

### Topic: GLS, blocking vs non-blocking and Synthesis-Simulation mismatch

#### Introduction to GLS, blocking vs non-blocking and Synthesis-Simulation mismatch

What is Gate Level Simulation (GLS)?

* Running the test bench with netlist as Design Under Test (DUT).
* Netlist is logically same as RTL code - same test bench will allign with the design.

Why GLS?

* Verify the logical correctness of design after synthesis.
* Ensuring the timing of the design is met - for this GLS needs to be run with delay annotation.

GLS using iverilog

*Notes: Mindmap is taken from instructor's video.

![day4labintroa](https://user-images.githubusercontent.com/118953917/206889766-5e90e4dc-1a2a-48ba-82c0-440d9c2a13cf.JPG)

*Notes: Example is taken from instructor's video.

![day4labintroB](https://user-images.githubusercontent.com/118953917/206892298-58e8e0c8-fa62-4660-9bf0-0f271a3f96d6.JPG)

Synthesis simulation mismatch

* Missing sensitivity list.
* Blocking vs non-blocking assignments.
* Non standard verilog coding.

Simulator works based on activity where output will change based upon the changes in input. If there is no activity, output will not change at all. 

*Notes: Example is taken from instructor's video.

![day4labintroc](https://user-images.githubusercontent.com/118953917/206893907-72c992b3-22a9-4916-b429-c9877f3d4aa1.JPG)

Blocking and non-blocking statements in verilog

**Inside always block**

= : Bocking

* Executes the statements in the order it is written.
* So, the first statement is evaluated before the second statement.

<= : Non-blocking

* Executes all the Right Hand Side (RHS) when always block is entered and assigns to Left Hand Side (LHS).
* Parallel evaluation. 

Caveats with blocking statements

*Notes: Example is taken from instructor's video.

![day4labintrod](https://user-images.githubusercontent.com/118953917/206893381-678189a6-58fc-46ec-883c-68ef1c17a8c1.JPG)

![day4labintroe](https://user-images.githubusercontent.com/118953917/206893870-a8421413-8d09-4017-9738-c667fd4a17c4.JPG)


###############################################################################################

#### Lab Result

#### Lab 1: GLS Synth Sim Mismatch

>> gvim ternary_operator_mux.v -o bad_mux.v -o good_mux.v 

**Ternary operator**

Example: assign y = sel?i1:i0

lab1a gambar 
  
>> iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
  
>> ./a.out

>> gtkwave tb_ternary_operator_mux.vcd

When sel = 0, the output Y follows input i0. Whereas, when sel = 1, Y follows input i0.

lab1b gambar

>> yosys

>> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

>> read_verilog ternary_operator_mux.v

>> synth -top ternary_operator_mux

>> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

>> write_verilog -noattr ternary_operator_mux_net.v 

>> show 

gambar result

>> iverilog ../my_lib/verilog_model/primitives.v  ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v

>> ./a.out

>> gtkwave tb_ternary_operator_mux.vcd 

gambar ternary operator mux

**Bad mux**

>> iverilog bad_mux.v tb_bad_mux.v 

>> ./a.out

>> gtkwave tb_bad_mux.vcd

**Synthesis simulation mismatch**

>> iverilog ../my_lib/verilog_model/primitives.v  ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v

>> ./a.out

>> gtkwave tb_bad_mux.vcd 

**Bad mux**

When there is activity on select, the inputs and output is toggling because it is depending on select.
* When select is low, i0 should be high based on the actual behavior of multiplexer. But, in this case, i0 is low because select is low (there is no activity in select).
* Whereas, when select is high, there is activity on select. Theoretically, i1 should be high and i0 should be low. But in bad mux, both inputs i0 and i1 is high since there is activity on select.
* Only when select is changing, the output Y is also changing based upon the select.

**Synthesis simulation mismatch**

The output Y is following both inputs i0 and i1 depending on select.
* When select is low, the output is following i0.
* Whereas when select is high, the output is following i1.







>> read_verilog bad_mux.v

>> synth -top bad_mux

>> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

>> write_verilog -noattr bad_mux_net.v 
