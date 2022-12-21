## Day-7

### Topic: Basic SDC constraints

<details>
  <summary>Introduction to STA</summary>
 

### Introduction to Static Timing Analysis (STA)

**Advanced Synthesis and STA using Design Compiler**

**STA Basics**

**Maximum and Minimum Delay Constraints**

![day7lab1a](https://user-images.githubusercontent.com/118953917/208686000-7da7df59-66a1-4852-acc1-82e3861dcb40.jpg)

**Why delay?**

By using water bucket analogy

Scenario 1: $\textcolor{red}{\text{Different}}$ amount of water inflow with the $\textcolor{red}{\text{same}}$ size of bucket.

Notes: Example figure is taken from instructor's video

![day7lab1b](https://user-images.githubusercontent.com/118953917/208686086-048909a3-e0e7-4cbb-8b19-9bc6e7ad78a1.jpg)
  
Scenario 1: $\textcolor{red}{\text{Same}}$ amount of water inflow with $\textcolor{red}{\text{different}}$ size of bucket.

Notes: Example figure is taken from instructor's video

  ![day7lab1c](https://user-images.githubusercontent.com/118953917/208686124-9c07015e-83ea-4a0b-a1b3-562532e1b507.jpg)
  
**Is delay of a cell constant?**
* Delay of a cell will be a function of input Transition which is the water inflow. 
* Delay of a cell will be a function of output load which is the size of the bucket.
  
  ![day7lab1d](https://user-images.githubusercontent.com/118953917/208686187-fbb5da82-515c-4658-b124-29a47fd8fec1.jpg)

**Timing arcs**
* Combinational cell -> Delay information from every input pin to every output pin which it can control.
  
  ![day7lab1e](https://user-images.githubusercontent.com/118953917/208686221-a37ade29-097b-44a7-b3df-158ac8a6b66b.jpg)

* Sequential cell -> can be DFF/D-latch
  
  -> Delay from clock to Q for D-flip flop
  
  -> Delay from clock to Q, delay from D to Q for D latch
  
  -> Setup and hold time 
  
![day7lab1f](https://user-images.githubusercontent.com/118953917/208686254-dcc45d95-460d-48bd-8b88-e4e39df82387.jpg)
  
 </details> 

<details>
  <summary>Constraints</summary>
 

### What are constraints?
  
**Timing paths**
  
![day7lab2a](https://user-images.githubusercontent.com/118953917/208686303-80ff2d78-65b0-4d05-b50d-565adfd34e57.jpg)

  
**Constraining the design**
  
Why constraints?
  
![day7lab2b](https://user-images.githubusercontent.com/118953917/208686332-9eaaf8bc-e010-4dc6-a0fa-26399c4ca734.jpg)

  
**Timing paths**
  
* Start point
  
  -> Input ports
  
  -> Clk pins of register
  
* End point
  
  -> Output ports
  
  -> D pins of DFF/D-latch
  
* Always the timing paths start at one of the start points and ends at one of the end points.
  
  -> Clk to D (reg-to-reg timing path)
  
  -> Clk to output (IO timing path)
  
  -> Input to D (IO timing path)
  
  -> Input to output (IO timing path that ideally shouldn't be present)
  
![day7lab2c](https://user-images.githubusercontent.com/118953917/208686404-63146af9-cffe-4acf-85db-a03ef948f9dc.jpg)

  
* Reg-to-reg: constrained by clock -> Tclk >= Tcq + Tcombi + Tsetup
  
* Reg-to-output: constrained by output external delay, output load and clock period 
  
* Input-to-reg: constrained by input external delay, input transition and clock period 
  
* Collectively the reg-to-output and input-to-reg are called IO paths and the delay modelling referred as IO delay modelling -> using standard interface specifications and budgeting based in interactions with other modules
  
</details> 

<details>
  <summary>IO Constraints</summary>
 

### Input transfer output load
  
Is IO modelling sufficient? 
  
![day7lab3a](https://user-images.githubusercontent.com/118953917/208686438-a656e8f2-9a32-4f31-975d-731993703bee.jpg)

  
Are IO delay and input transition modelling sufficient for IO path?
  
Note: IO path should be constrained for both maximum delay (setup) and minimum delay (hold)
  
![day7lab3b](https://user-images.githubusercontent.com/118953917/208686468-072ed17d-5645-448d-a3e8-ebc4af5fbf04.jpg)
</details> 

<details>
  <summary>Lab 1: Timing dot libs</summary>
 

### Lab 1: Timing .lib
  
>> Open .lib file
  
> gvim DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
  
> :syn off
  
gambar 113
  
Understanding in default max transition
  
gambar 114
  
Understanding in delay model: table look up
  
gambar 115

>> To split gvim side by side
> :vsp 
  
gambar 116
  
Internal power delay
  
Note: LHS is and2_2 while RHS is and2_0
  
gambar 117
  
Timing delay
  
gambar 118
  
Timing sense and timing type in timing arcs
  
gambar 119
  
</details> 

<details>
  <summary>Lab 2: Exploring dot libs part 1</summary>
 

### Lab 2: Exploring .lib part 1

Details of pins 
  
gambar 120 

Rising/falling edge of the pins
  
gambar 121 

Setup time of rising/falling edge of the clock pin

gambar 122

>> Invoking dc_shell to look for DFF and D-latch name

> csh

> dc_shell

> echo $target_library

> get_lib_cells */* -filter "is_sequential==true"

gambar 123

</details> 

<details>
  <summary>Lab 3: Exploring dot libs part 2</summary>
 

### Lab 3: Exploring .lib part 2 

> list_lib
  
> get_lib_cells */*and*
  
> foreach_in_collection my_lib_cell [get_lib_cells */*and*] {
  
  set my_lib_cell_name [get_object_name $my_lib_cell]; 
  
  echo $my_lib_cell_name;  
  
  }
  
> get_lib_pins sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/*
  
> foreach_in_collection my_pins [get_lib_pins sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/*] {
  
  set my_pin_name [get_object_name $my_pins];
  
  set pin_dir [get_lib_attribute $my_pin_name direction];
  
  echo $my_pin_name $pin_dir;
  
  }
  
> get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/A direction
  
> get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/A is_output
  
>get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/A input
  
> get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/X function
  
> 
