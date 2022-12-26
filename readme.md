## Day-7

### Topic: Advanced SDC Constraints

<details>
  <summary>SDC Part1</summary>
 

### SDC Part1: Clock - Clock Tree Modelling - Uncertainty

**Advanced Synthesis and STA using Design Compiler**

**Advanced constraints, specifying constraints through SDC**

+ Timing path
  * Reg-to-reg: constrained by clock -> clock period 
  * Reg-to-output: constrained by output external delay, output load and clock period 
  * Input-to-reg: constrained by input external delay, input transition and clock period 
  
**What needs to be constrained for clocks?**

![image](https://user-images.githubusercontent.com/118953917/209547160-3b2465ee-2f28-46e5-a85e-a2682216a05b.png)

**Clock generation**

* Oscillator
* Phase-Locked Loop (PLL) 
* External clock source
* All of the above clock sources have inherent variations in the clock period due to stochastic effects

![image](https://user-images.githubusercontent.com/118953917/209547202-64208f1f-39be-4fe0-8874-c95eb0153137.png)
  
**Clock distribution**
  
Jitter: Stochastic variations of clock generation
  
![image](https://user-images.githubusercontent.com/118953917/209547219-516926a5-bc50-4791-a161-144b45220748.png)
  
**Clock skew**
  
* Clock Tree built during CTS -> Practical Clock Tree
  
* Logic optimization happens in synthesis -> Ideal Clock Tree
  
* Timing Clean path in synthesis may fail after STA
  
![image](https://user-images.githubusercontent.com/118953917/209547244-c618379a-2dab-4259-8188-878a6828ea09.png)
  
**Clock modelling**
  
* Period
* Source latency: time taken by the clock source to generate clock
* Clock Network Latency: time taken by clock distribution network
* Clock skew: clock path delay mismatches which causes difference in the arrival of the clock
  + CTS will balance the clocks, but still the skew cannot be reduced to 0
* Jitter: stochastic variations in the arrival of clock edge
  + Duty cycle jitter
  + Period jitter 
* Collectively clock skew and jitter are called Clock Uncertainty
  
![image](https://user-images.githubusercontent.com/118953917/209547266-2ba5968a-ceb8-4478-b6c4-13be3a956085.png)
  </details>
<details>
  <summary>SDC Part2</summary>
 

### SDC Part1: IO Delays
  
**Advanced Synthesis and STA using Design Compiler**

**Advanced constraints, specifying constraints through Synopsys Design Constraints (SDC)**

**How to constraint the design in DC?**
  
* DC takes constraints in the form of SDC 
  
![image](https://user-images.githubusercontent.com/118953917/209547347-511c0ac1-87e4-4b06-a646-8b9fe164651d.png)
  
**Commands of getting ports in DC**
  
> To get the ports
```
get_ports clk;                           (All ports containing clock)
get_ports *clk*;                         (Collection of ports containing clk)
get_ports *;                             (All ports of the design)
get_ports * -filter "direction == in";     (All input ports)
get_ports * -filter "direction == out";    (All output ports)
```
  
-> get_ports -> Query the ports in the design
+ For example; 
  * port
  * pin
  * clock

*Note: all of the names are case sensitive 

**Commands of getting clocks in DC**
  
> To get the clocks
```
get_clocks *                                        (All clocks in the design)
get_clocks *clk*                                    (All clocks containg clk)
get_clocks * -filter "period>10"                    (Filtering the clocks)
get_attribute [get_clocks_ my_clk] period           (Querying period of the mentioned clock)
get_attribute [get_clocks my_clk] is_generated      (Checking whether clock mentioned is generated or not)
report_clocks my_clk                                (Reporting the details of the clock)
```

**Querying the cells in the design**
  
![image](https://user-images.githubusercontent.com/118953917/209547377-5a64bd16-da43-4e90-9a7c-0759ef6fcab7.png) 

**Clock distribution**
  
![image](https://user-images.githubusercontent.com/118953917/209547415-7c81ece4-d884-4094-b3a9-40de1493552e.png)
  
Bringing the practicalities (latency, uncertainty) of clock network
  
![image](https://user-images.githubusercontent.com/118953917/209547435-6d8c7e48-d746-4d4d-a0d3-053d269570cb.png)
  
**Clocks: Waveform**
  
![image](https://user-images.githubusercontent.com/118953917/209547461-739d2845-a057-4043-bc2f-6179ca3a8546.png)
  
**Constraining IO paths**
  
![image](https://user-images.githubusercontent.com/118953917/209547507-9b73d080-41c5-47a9-b758-07852a0b66f8.png)
  
![image](https://user-images.githubusercontent.com/118953917/209547538-bb3e046f-efba-4ef3-b092-2e883f283fc5.png)
	
**Summary**
  
* create_clock
* set_clock_latency
* set_clock_uncertainty 
  + skew + jitter -> Pre-CTS
  + only jitter -> Post-CTS
* set_input_delay
* set_input_transition
* set_output_delay
* set_load
* get_ports, get_clocks, get_cells
  
</details>
<details>
  <summary>Lab 1: Loading design</summary>
 

### Lab 1: Loading design get_cells, get_ports, get_nets
  
> gvim lab8_circuit.v
  
![image](https://user-images.githubusercontent.com/118953917/209547591-73c16358-105d-4af2-82de-e596fe948645.png)
  
> Invoke dc_shell
```
csh                                                        
dc_shell                                
echo $target_library 
echo $link_library 
read_verilog DC_WORKSHOP/verilog_files/lab8_circuit.v 
link
compile_ultra
```
![image](https://user-images.githubusercontent.com/118953917/209547614-41c9d71a-1fc7-44da-b126-e97d029e81e9.png)

> Get ports
```
get_ports
  
foreach_in_collection my_port [get_ports *] {                      (Listing the collection of ports name)
set my_port_name [get_object_name $my_port];
echo $my_port_name;
}
  
get_ports rst
get_attribute [get_ports rst] direction                           (Display reset port direction)

foreach_in_collection my_port [get_ports *] {                     (Listing the collection of port name and its direction)
set my_port_name [get_object_name $my_port];
set dir [get_attribute [get_ports $my_port_name] direction];     
echo $my_port_name $dir;
}
```

![image](https://user-images.githubusercontent.com/118953917/209547642-d79c7ea6-a7d4-4825-85ea-62a03853d159.png)

> Get cells
```
get_cells *
get_attribute [get_cells U9] is_hierarchical                        (Checking the desired cell is hierarchical/not)
get_cells * -hier -filter "is_hierarchical == false"                (Listing the false hierarchical cell)
get_cells * -hier -filter "is_hierarchical == true"                 (Listing the true hierarchical cell)
```
  
![image](https://user-images.githubusercontent.com/118953917/209547666-fe7bb361-e71b-4c7f-a480-b85ed7cf036f.png)
  
  
> Get reference name of the cells
```
get_attribute [get_cells REGA_reg] ref_name                         (Getting the reference name of the cell)
  
foreach_in_collection my_cell [get_cells * -hier] {                 (Listing cell name and its reference name)
set my_cell_name [get_object_name $my_cell];
set rname [get_attribute [get_cells $my_cell_name] ref_name];
echo $my_cell_name $rname;
} 
```

![image](https://user-images.githubusercontent.com/118953917/209547686-7edde42e-7a39-4bc7-8298-0fe6434ad306.png)

> Writing DDC 
```
write -f ddc -out lab8_circuit.ddc
csh
design_vision
read_ddc DC_WORKSHOP/verilog_files/lab8_circuit.ddc
get_nets *
all_connected N1
all_connected n5

foreach_in_collection my_pin [all_connected n5] {                   (Listing the driver of the net)
set pin_name [get_object_name $my_pin];
set dir [get_attribute [get_pins $pin_name] direction];
echo $pin_name $dir;
}
```
![image](https://user-images.githubusercontent.com/118953917/209547714-293a0e43-0d98-452a-a16d-73cc140199f5.png)
	
![image](https://user-images.githubusercontent.com/118953917/209547737-d67a4e53-41e9-4858-85bb-21d26ce72a4e.png)

Note: In digital design, net can only have one driver
  
Example: 
  
![image](https://user-images.githubusercontent.com/118953917/209547771-e4aff53d-73bb-4abc-afdb-ddb691ba76c1.png)
  
</details>
<details>
  <summary>Lab 2: Loading design</summary>
 

### Lab 2: Loading design get_pins, get_clocks, querying_clocks
  
> Get pins 
```
get_pins *

foreach_in_collection my_pin [get_pins *] {                            (Listing out the pins)
set pin_name [get_object_name $my_pin];
echo $pin_name;
}

get_attribute [get_pins REGC_reg/RESET_B] direction
get_attribute [get_pins REGC_reg/RESET_B] clock
get_attribute [get_pins REGC_reg/CLK] clock
  
foreach_in_collection my_pin [get_pins *] {
set pin_name [get_object_name $my_pin];
set dir [get_attribute [get_pins $pin_name] direction];
echo $pin_name $dir;
}
```
  
![image](https://user-images.githubusercontent.com/118953917/209547817-e39169be-520f-4d0c-b646-29c37fe1b9b6.png)
  
> Make use of regexp
>> 0 if it is not found, 1 if it is matched
```
regexp abcd efgh
regexp abcd abcd
set a in
regexp $a in
regexp $a out

foreach_in_collection my_pin [get_pins *] {                             (Listing input pin name with clock)
set pin_name [get_object_name $my_pin];                                                                                      
set dir [get_attribute [get_pins $pin_name] direction];                                                      
if { [regexp $dir in] } {                                                                                    
if { [get_attribute [get_pins $pin_name] clock] } {                                          
echo $pin_name;                                                                                              
}                                                                                                            
}                                                                                                            
}
```

![image](https://user-images.githubusercontent.com/118953917/209547853-dfec6797-ed26-4631-a9b8-65ee0dc8ac11.png)
  
> Scripting in gvim 
```
sh gvim query_clock_pin.tcl

foreach_in_collection my_pin [get_pins *] {                              (Listing input pin name with clock - The pin is meant to be a clock pin/not)
	set my_pin_name [get_object_name $my_pin];
        set dir [get_attribute [get_pins $my_pin_name] direction];                                                                                              
	if { [regexp $dir in] } {
		if { [get_attribute [get_pins $my_pin_name] clock] } {
		       echo $my_pin_name $clk_name;

		}
	}
}	
```
   
> Checking if there is any clock coming in
```
get_attribute [get_pins REGA_reg/CLK] clock
get_attribute [get_pins REGA_reg/CLK] clocks
```

> Get clock
```
get_clocks *
```
  
![image](https://user-images.githubusercontent.com/118953917/209547885-0ea80cfa-1929-4918-bf93-229334f35a54.png)
</details>
<details>
  <summary>Lab 3: Clock waveform</summary>
 

### Lab 3: Creating clock waveform

> Get the name of top module that is currently working, get ports, create clock and attribute
```
current_design										(Display current module)
get_ports *
create_clock -name MYCLK -per 10 [get_ports clk]
get_clocks *
get_attribute [get_clocks MYCLK] period
get_attribute [get_clocks MYCLK] is_generated						(If false, it is the master clock)
get_attribute [get_pins REGA_reg/CLK] clocks
report_clocks *										(Reviewing clocks report)
get_attribute [get_pins REGA_reg/CLK] clocks
get_attribute [get_ports out_clk] clocks
sh gvim query_clock_pin.tcl
	
foreach_in_collection my_pin [get_pins *] {						(Contents inside gvim)
	set my_pin_name [get_object_name $my_pin];
        set dir [get_attribute [get_pins $my_pin_name] direction];                                                                                              
	if { [regexp $dir in] } {
		if { [get_attribute [get_pins $my_pin_name] clock ] } { 
			set clk [get_attribute [get_pins $my_pin_name] clocks]; 
	# set clk_name [get_object_name [get_attribute [get_pins $my_pin_name] clocks]];
			set clk_name [get_object_name $clk];
 			echo $my_pin_name $clk_name;

		}
	}
}
	
source query_clock_pin.tcl								(Source the gvim)
```
![image](https://user-images.githubusercontent.com/118953917/209548104-723892a1-797e-4f1b-8ed3-201a1d050774.png)
	
> Creating clock and reviewing clock report
```
get_cells U13
get_attribute [get_cells U13] ref_name
get_attribute [get_cells U14] ref_name
create_clock -name BAD_CLK -per 10 [get_pins U14/Y]
get_clocks *
report_clocks *								(From the report, it's not reaching any clock pin/data path of any flop)
all_connected U14/Y
all_connected n3
```
	
![image](https://user-images.githubusercontent.com/118953917/209548139-c21c44c1-be63-4740-8572-0ec0a9217b13.png)

> Removing clock and adjusting multiple clock waveforms
```
remove_clock BAD_CLK
get_clocks *
report_clocks *
remove_clock MYCLK
create_clock -name MYCLK -per 10 [get_ports clk] -wave {5 10}				(Rising edge = 5; falling edge = 10)
report_clocks *
remove_clock MYCLK
create_clock -name MYCLK -per 10 [get_ports clk] -wave {0 2.5}				(Order is not important)
report_clocks *
remove_clock MYCLK
create_clock -name MYCLK -per 10 [get_ports clk] -wave {15 20}
report_clocks *
```

![image](https://user-images.githubusercontent.com/118953917/209548165-2c4f4c42-a82d-4536-85f6-1cccd533bdca.png)
	
</details>
<details>
  <summary>Lab 4: Clock Network Modelling</summary>
 

### Lab 4: Clock Network Modelling: Uncertainty & report_timing
  
* Clock skew -> clock network due to imbalance delay between the flops
* Clock jitter -> Random variations in clock edge, coming from the source
* Clock Tree Synthesis (CTS) -> the tool to minimize the imbalance but it cannot make it zero ("0")
* Clock_uncertaint
	+ Skew + jitter (in pre-CTS)
	+ Jitter alone since the clock network is actualy built (in post-CTS)
	
![image](https://user-images.githubusercontent.com/118953917/209548189-207a7007-d804-4f4e-9542-815b9198d5f9.png)

> Modelling the clock tree attribute
```
report_clocks *
set_clock_latency -source 1 [get_clocks MYCLK]					(Modelling the source latency)
set_clock_latency 1 [get_clocks MYCLK]						(Modelling the network latency)
set_clock_uncertainty 0.5 [get_clocks MYCLK]					(Max delay for setup time by default)
set_clock_uncertainty -hold 0.1 [get_clocks MYCLK]				(Min delay for hold time) 
```
	
![image](https://user-images.githubusercontent.com/118953917/209548207-0a3e6d53-ce51-4edd-acce-6fec2347b3c7.png)
	
> Clock timing
```
remove_clock *
get_clocks *
report_timing
report_timing -to REGC_reg/D							(Got nothing to constraint)
create_clock -name MYCLK -per 10 [get_ports clk]
report_timing -to REGC_reg/D
```
	
![image](https://user-images.githubusercontent.com/118953917/209548231-1606e86b-c8a2-4298-b382-5f6fc83e445e.png)
	
> Model clock behavior and report timing
```
set_clock_latency -source 2 [get_clocks MYCLK]
set_clock_latency 1 [get_clocks MYCLK]
set_clock_uncertainty -setup  0.5 [get_clocks MYCLK]
set_clock_uncertainty -hold  0.1 [get_clocks MYCLK]
report_timing -to REGC_reg/D
```

![image](https://user-images.githubusercontent.com/118953917/209548245-36cc6d8f-7c3b-4bec-a77c-2e7ad7f874ff.png)
	
> Hold time
```
report_timing -to REGC_reg/D -delay min
```
	
![image](https://user-images.githubusercontent.com/118953917/209548278-1f83bd69-d551-4b67-975c-1829ea55a905.png)
	
> Report of all ports
```
report_port -verbose
```
	
![image](https://user-images.githubusercontent.com/118953917/209548295-44b174ce-93c1-4c39-a497-d893337ca2d1.png)
	
	
</details>
<details>
  <summary>Lab 5: IO Delays</summary>
 

### Lab 5: IO Delays
  
![image](https://user-images.githubusercontent.com/118953917/209548311-f40b3814-2e08-4f5c-a535-26295823dae4.png)
	
> IO Constraints
```
report_timing -from IN_A
report_timing
report_timing -to OUT_Y
```
	
![image](https://user-images.githubusercontent.com/118953917/209548340-884ddc87-b125-4ec9-8727-5d9f0f801533.png)
	
> Listing all the ports
```
report_port -verbose
```
	
![image](https://user-images.githubusercontent.com/118953917/209548360-7b4f3c30-0bdf-4022-a5e3-84244a1ca94f.png)
	
> Modelling the delay
```
set_input_delay -max 5 -clock [get_clocks MYCLK] [get_ports IN_A]				(Setting input delay)
set_input_delay -max 5 -clock [get_clocks MYCLK] [get_ports IN_B]
report_port -verbose
report_timing -from IN_A
report_timing -from IN_A -trans -net -cap
report_timing -from IN_A -trans -net -cap -nosplit > a
sh gvim a
```

![image](https://user-images.githubusercontent.com/118953917/209548381-41af4143-b650-481e-b370-51f81a2bd7f0.png)
	
![image](https://user-images.githubusercontent.com/118953917/209548404-5074afa1-bcab-4430-a26f-192f48fc5525.png)
	
> Hold timing
```
report_timing -from IN_A -trans -net -cap -nosplit -delay_type min
```
	
![image](https://user-images.githubusercontent.com/118953917/209548431-74172e5c-2cdc-4219-9f26-79819165909a.png)
	
> Modelling the hold time 
```
set_input_delay -min 1 -clock [get_clocks MYCLK] [get_ports IN_A]
set_input_delay -min 1 -clock [get_clocks MYCLK] [get_ports IN_B]
report_timing -from IN_A -trans -net -cap -nosplit -delay_type min
```
	
![image](https://user-images.githubusercontent.com/118953917/209548454-4fe83460-3285-473d-a79b-c0fabcb05041.png)
	
> Setting new maximum value
```
set_input_delay -max 5 -clock [get_clocks MYCLK] [get_ports IN_A]
sh gvim a
```
	
![image](https://user-images.githubusercontent.com/118953917/209548480-9ee82772-2ebc-4000-b595-65cbacfcdfb2.png)
	
> Setting input transition
```
set_input_transition -max 0.3 [get_ports IN_A]
set_input_transition -max 0.3 [get_ports IN_B]
set_input_transition -min 0.1 [get_ports IN_B]
set_input_transition -min 0.1 [get_ports IN_A]
report_timing -from IN_A -trans  -cap -nosplit > a_trans
```
	
![image](https://user-images.githubusercontent.com/118953917/209548506-1026575b-c0eb-4a04-ba6a-c46501056e37.png)
	
> Setting output transition
```
set_output_delay -max 5 -clock [get_clocks MYCLK] [get_ports OUT_Y]
set_output_delay -min 1 -clock [get_clocks MYCLK] [get_ports OUT_Y]
report_timing -to OUT_Y
report_timing -to OUT_Y -cap -trans
```
	
![image](https://user-images.githubusercontent.com/118953917/209548527-d3feaac2-0f9f-4a68-b55a-87f58884e1d8.png)
	
> Setting for load
```
set_load -max 0.4 [get_ports OUT_Y]
report_timing -to OUT_Y -cap -trans
```
	
![image](https://user-images.githubusercontent.com/118953917/209548546-1a570a8c-af03-44ab-b060-2c19c1d0f7a0.png)
	
> Setting minimum delay
```
report_timing -to OUT_Y -cap -trans -delay min
```
	
> Setting another load for minimum delay
```
report_timing -to OUT_Y -cap -trans -delay min
```
	
![image](https://user-images.githubusercontent.com/118953917/209548567-8982e988-5709-454f-98c9-ae39b498246c.png)
	
</details>
<details>
  <summary>SDC Part3</summary>
 

### SDC Part3: Generated clock

**Notes**
![image](https://user-images.githubusercontent.com/118953917/209548593-e87f2035-ef23-4ab2-af7b-cdd647b7aa2c.png)
	
**Generated clocks**

* Generated clocks: always created with respect to master clocks (clocks at clock source i.e. PLL/oscillator or primary IO port)

```
create_generated_clock -name MY_GEN_CLK -master [get clocks MY_CLK] -source [get_ports CLK] -div 1 [get_ports OUT_CLK]
```

where;
* [get clocks MY_CLK] -source [get_ports CLK] 
	+ wrt what the generated clock is created
* -div
	+ Refers to the division value of generated clock (useful for clock dividers)
* [get_ports OUT_CLK]
	+ Generated definition point where the generated clock is created 
* Out_Y need to be constrained wrt the generated clock
	
**Constraining the design**
	
![image](https://user-images.githubusercontent.com/118953917/209548621-38adcc67-bb3e-4d40-b9ea-0dd5ffb097f3.png)

![image](https://user-images.githubusercontent.com/118953917/209548643-4bf30f41-2483-4114-9117-9341fffe2714.png)

</details>
<details>
  <summary>Lab 6: Generated clock</summary>
 

### Lab6: Generated clock

```
report_timing -to OUT_Y
```
	
![image](https://user-images.githubusercontent.com/118953917/209548658-671239fa-0b0b-422c-a707-1356d6099079.png)
	
> Creating generated clock
```
create_generated_clock -name MYGEN_CLK -master MYCLK -source [get_ports clk] -div 1 [get_ports out_clk]
report_clocks
get_attribute [get_clocks MYGEN_CLK] is_generated								(True if it is generated clk)
get_attribute [get_clocks MYCLK] is_generated									(False if it is generated clk - master clk)
```
	
> Modelling MYGEN_CLK
```
set_clock_latency -max 1 [get_clocks MYGEN_CLK]
set_output_delay -max 5 [get_ports OUT_Y] -clock [get_clocks MYGEN_CLK]
set_output_delay -min 1 [get_ports OUT_Y] -clock [get_clocks MYGEN_CLK]
report_timing -to OUT_Y
```
	
![image](https://user-images.githubusercontent.com/118953917/209548669-bc6ad376-aadc-4dd6-ac1e-ecd9b3046796.png)
	
> Modifying lab8_circuit.v
```
sh gvim DC_WORKSHOP/verilog_files/lab8_circuit.v
:vsp lab8_circuit_modified.v 
```
	
![image](https://user-images.githubusercontent.com/118953917/209548680-add03dc2-c565-47a5-8e03-d43640f10a2f.png)
	
> Resetting the design
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/lab8_circuit_modified.v
sh gvim DC_WORKSHOP/verilog_files/lab8_cons.tcl
link
source DC_WORKSHOP/verilog_files/lab8_cons.tcl
report_clocks
get_generated_clocks
```
	
![image](https://user-images.githubusercontent.com/118953917/209548705-eed00ea4-2abd-42cc-8711-e9a5d1207837.png)
	
> Reporting the ports
```
report_port -verbose
```

The design is completely constrained

![image](https://user-images.githubusercontent.com/118953917/209548729-afb555d7-e69d-49a7-97d7-5e6617abf752.png)	
	
</details>
<details>
  <summary>SDC Part4</summary>
 

### SDC Part4: vclk, max_latency, rise_fall IO Delays

**Input delay**
	
* Positive delay 
	+ data comes after clk edge
	+ for max is tightening
* Negative delay 
	+ data comes before clk edge
	+ for max is relaxing
	
* min delay 
	+ Positive is tightening the path
	+ Negative is relaxing the path
	
```
set_input_delay –max 3 –clock myclk [get_ports IN_A]
```
	
* create_clock_name myclk –per 10 [get_ports clk ]
* To get available time -> available time = [clock period – uncertainty – input delay] 
* Positive for max is tightening
	
```
set_input_delay –max -3 –clock myclk [get_ports IN_A]
```
	
* Negative i.e. -3 is the delay where before the rise edge, the data was stable, however, the clock got more delayed compared to the data
* i.e. 10 -[-3] = 13 ns
* Negative delay for max is relaxing 
	
```
set_input_delay –min 1  –clock myclk [get_ports IN_A]
```
	
* Positive delay –> the data comes after clock edge
* 1 ns is the hold time and it helps to meet the hold time
* Positive delay becomes relax
	
```
set_input_delay –min -1 –clock myclk [get_ports IN_A]
```
	
* Negative i.e. -1 is delay where the data comes after clock edge
* Note that we need to avoid the hold failures, hence, we need to delay the data
* Negative for min delay is tightening 
	
![image](https://user-images.githubusercontent.com/118953917/209548761-fcc143e7-1433-487f-9197-56b1a2b09142.png)
	
**Output delay**
	
```
set_output_delay –max 3 -clock myclk [get_ports IN_A]
```
	
* set clock period to 10 ns 
* To get avalaibale time -> available time = clock period – external delay 

```
set_output_delay –max -3 -clock myclk [get_ports IN_A]
```
	
```
set_output_delay –min 1 -clock myclk [get_ports IN_A]
```
	
* Relaxing the hold time
* Independent of the clock period 
	
```
set_output_delay –min -1 -clock myclk [get_ports IN_A]
```
	
* Tightening the hold time
* Independent of the clock period
* -min delay -> clock relatively getting pushed wrt the data
	
**IO constraints re-visited**
	
![image](https://user-images.githubusercontent.com/118953917/209548788-ac62ef3b-3461-4252-9b67-92097a7d0d20.png)
![image](https://user-images.githubusercontent.com/118953917/209548805-7d50e9ff-d06c-4887-b2e5-b46b8264bcf2.png)
![image](https://user-images.githubusercontent.com/118953917/209548924-2146da70-dea6-4930-9fcb-7272f39cb89b.png)
![image](https://user-images.githubusercontent.com/118953917/209548952-5ebe1835-cb2f-486b-b877-b3d5fd4f061b.png)
![image](https://user-images.githubusercontent.com/118953917/209548981-c09d0a9e-38d1-4a19-bd72-bbad8fee77f6.png)
	
</details>
<details>
  <summary>Lab 7: Set_Max_delay</summary>
 

### Lab 7: Set_Max_delay

![image](https://user-images.githubusercontent.com/118953917/209549018-a529d492-b5a2-49ef-8650-19939e5d3c99.png)
	
> Resetting the design to lab14_circuit.v
```
reset_design
sh gvim DC_WORKSHOP/verilog_files/lab14_circuit.v
read_verilog DC_WORKSHOP/verilog_files/lab14_circuit.v
source DC_WORKSHOP/verilog_files/lab8_cons.tcl 
report_timing								(Report timing in terms of gtech cells)
```
	
![image](https://user-images.githubusercontent.com/118953917/209549039-8787ac5c-ef25-40cf-88e4-eb038238b0e6.png)
	
> Linking the design
```
link
compile_ultra
report_timing
```
	
![image](https://user-images.githubusercontent.com/118953917/209549055-e0f23d8c-93e5-4df4-a799-4b91f2c5962c.png)
	
> Checking the path whether it is constrained/not
```
get_ports *
report_timing -to OUT_Z
report_timing -from IN_C
```
	
![image](https://user-images.githubusercontent.com/118953917/209549141-08dc3ff6-ceee-4a44-9c6a-2084c9fa69d5.png)
	
> Another helpful commands to try on
```
all_inputs							(Listing all inputs)
all_outputs							(Listing all outputs)
all_clocks 							(Listing all clocks)
all_registers 							(Listing all registers)
all_registers -clock MYCLK 					(Listing registers clocked by MYCLK)
all_fanout -from IN_A 						(Listing all fanouts of desired input)
all_fanout -flat -endpoints_only -from IN_A
all_fanin -to REGA_reg/D
all_fanin -flat -startpoints_only -to REGA_reg/D
```
	
Note: ENDPOINTS of a timing path => D/output pin

> Constraining path to Z
Note that no path to OUT_Z yet
```
set_max_delay 0.1 -from [all_inputs] -to [get_port OUT_Z]
report_timing -to OUT_Z -sig 4
```
	
Path is not constraint properly, DC do not optimized to meet the path
	
![image](https://user-images.githubusercontent.com/118953917/209549166-1979fbc3-9ef8-4b1a-b949-a0dd05be00e7.png)
	
> Optimizing the delay
	
```
compile_ultra
report_timing -to OUT_Z -sig 4
```
	
The tool is picking up other cells
	
![image](https://user-images.githubusercontent.com/118953917/209549204-d26fe307-a6b3-4c1e-a159-7064fcc4d7f5.png)
	
> Invoking design_vision
```
write -f ddc -out DC_WORKSHOP/verilog_files/lab14.ddc
reset_design
read_ddc DC_WORKSHOP/verilog_files/lab14.ddc
start_gui
```
	
![image](https://user-images.githubusercontent.com/118953917/209549228-c4c2e809-0499-4bb6-9545-8fc7825e0858.png)
	
</details>
<details>
  <summary>Lab 8: VCLK</summary>
 

### Lab 8: VCLK

**Virtual clock**
* Other ways to constraint path from IN_C or IN_D to Z
* Input delay and output delay with respect to virtual clock
	
**While using VCLK**
* First method: set_max_delay
* Second method: use virtual clock where a clock is created without a definition point
	
> Using VCLK
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/lab14_circuit.v
link
source DC_WORKSHOP/verilog_files/lab8_cons.tcl
compile_ultra
report_clocks
report_timing -to OUT_Z
```
	
![image](https://user-images.githubusercontent.com/118953917/209549269-37b2fb49-8308-41b9-801b-fc73199f82f8.png)	
```
create_clock -name MYVCLK -per 10
report_clocks
```
	
The highlighted one is virtual clock since it has no source
	
![image](https://user-images.githubusercontent.com/118953917/209549295-c3d2a9ed-3a6e-408b-9340-815a008168f3.png)
	
> Annotating IO Delays
```
set_input_delay -max 5 [get_ports IN_C] -clock [get_clocks MYVCLK]
set_input_delay -max 5 [get_ports IN_D] -clock [get_clocks MYVCLK]
set_output_delay -max 4.9 [get_ports OUT_Z] -clock [get_clocks MYVCLK]
report_timing
```
	
![image](https://user-images.githubusercontent.com/118953917/209549331-a1547370-569c-42d1-8be8-abc0ffc368ab.png)
	
> Optimizing 
```
compile_ultra
report_timing -to OUT_Z -sig 4
report_port -verbose
```

![image](https://user-images.githubusercontent.com/118953917/209549363-fa392726-ada0-455d-8aef-100f65a3c50a.png)
	
</details>
