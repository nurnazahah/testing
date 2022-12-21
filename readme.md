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
  
  ![day7lab4a](https://user-images.githubusercontent.com/118953917/208875467-f5c0a760-1893-4665-b37e-30d11b7ac78a.jpg)
  
Understanding in default max transition
  
![day7lab4b](https://user-images.githubusercontent.com/118953917/208875522-6492b38c-dc0f-447a-b503-734373bf5d01.jpg)  
  
Understanding in delay model: table look up
  
![day7lab4c](https://user-images.githubusercontent.com/118953917/208875583-9ee8d137-daa6-4305-a6c7-05415a7823e2.jpg)

>> To split gvim side by side
  
> :vsp 
  
![day7lab4d](https://user-images.githubusercontent.com/118953917/208875621-0885c41f-82fe-4850-b49c-b4583fdc94e7.jpg)
  
Internal power delay
  
Note: LHS is and2_2 while RHS is and2_0
  
![day7lab4e](https://user-images.githubusercontent.com/118953917/208875652-d5050b54-ea64-411f-b2af-654412a4271e.jpg)
  
Timing delay
  
![day7lab4f](https://user-images.githubusercontent.com/118953917/208875681-a322de6b-1e7a-4d90-8adf-b8555d78d7d2.jpg)
  
Timing sense and timing type in timing arcs
  
![day7lab4g](https://user-images.githubusercontent.com/118953917/208875709-de6c0d32-61f7-44c4-b0b9-977d62781ea6.jpg)
  
</details> 

<details>
  <summary>Lab 2: Exploring dot libs part 1</summary>
 

### Lab 2: Exploring .lib part 1

Details of pins 
  
![day7lab5a](https://user-images.githubusercontent.com/118953917/208875755-9b9ed68c-cc8e-473b-9b4c-84c87dbae8d9.jpg)

Rising/falling edge of the pins
  
![day7lab5b](https://user-images.githubusercontent.com/118953917/208875786-c5f849db-1204-40e1-93d0-c346f9f901b1.jpg)

Setup time of rising/falling edge of the clock pin

![day7lab5c](https://user-images.githubusercontent.com/118953917/208875819-39b2acfc-16f9-4a69-be3c-90904c99ba03.jpg)

>> Invoking dc_shell to look for DFF and D-latch name

> csh

> dc_shell

> echo $target_library

> get_lib_cells */* -filter "is_sequential==true"

![day7lab5d](https://user-images.githubusercontent.com/118953917/208875857-88aeb1f8-b78b-4b1d-b713-508548f3cc81.jpg)

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

>> To check the direction of the cell
  
> get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/A direction

>> To check the output pin
  
> get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/A is_output
  
>> To check the input pin
  
>get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/A input
 
>> To get the functionality of the cell
  
> get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/X function
  
>> Try another gate
  
>> NAND gate with 4 inputs
  
> get_lib_pins sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__nand4_1/*

>> Repeat the same steps for forach_in_collection for nand4_1
  
>> AND gate with 2 inputs
  
> get_lib_pins sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2b_1/*
  
>> Repeat the same steps for forach_in_collection for and2_b1
  
![day7lab6a](https://user-images.githubusercontent.com/118953917/208875892-ed546572-7921-4af7-9b62-8f7f65d7493b.jpg)
  
> sh gvim my_script.tcl
  
>> Insert the contents inside gvim, save and source the file 
  
> source my_script.tcl
  
>> To get the area value of the cell
  
> get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__nand4b_2 area
  
>> To see the capacitance of B pin
  
> get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__nand4b_2/B capacitance
  
>> To get the connection of the pin with the clock
  
> get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__nand4b_2/B clock
  
![day7lab6b](https://user-images.githubusercontent.com/118953917/208875925-5a719a7c-910c-4d83-a988-25bbb5fcf9fe.jpg)
  
> get_lib_cells */* -filter "is_sequential == true"
  
>> Explore D-latch
  
>> To check whether each component is depending on pins/ports/cell/etc
  
> list_attribute -app
  
![day7lab6c](https://user-images.githubusercontent.com/118953917/208875968-dc77b926-70df-4884-897a-42ba7fb77df8.jpg)
  
</details> 
