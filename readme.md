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
  
-	To get the ports
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
  
-	To get the clocks
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

gambar 152
	
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
	
gambar 153

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

gambar 154
	
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
	
gambar 155 

> Modelling the clock tree attribute
```
report_clocks *
set_clock_latency -source 1 [get_clocks MYCLK]					(Modelling the source latency)
set_clock_latency 1 [get_clocks MYCLK]						(Modelling the network latency)
set_clock_uncertainty 0.5 [get_clocks MYCLK]					(Max delay for setup time by default)
set_clock_uncertainty -hold 0.1 [get_clocks MYCLK]				(Min delay for hold time) 
```
	
gambar 156
	
> Clock timing
```
remove_clock *
get_clocks *
report_timing
report_timing -to REGC_reg/D							(Got nothing to constraint)
create_clock -name MYCLK -per 10 [get_ports clk]
report_timing -to REGC_reg/D
```
	
gambar 157
	
> Model clock behavior and report timing
```
set_clock_latency -source 2 [get_clocks MYCLK]
set_clock_latency 1 [get_clocks MYCLK]
set_clock_uncertainty -setup  0.5 [get_clocks MYCLK]
set_clock_uncertainty -hold  0.1 [get_clocks MYCLK]
report_timing -to REGC_reg/D
```

gambar 158
	
> Hold time
```
report_timing -to REGC_reg/D -delay min
```
	
gambar 159
	
> Report of all ports
```
report_port -verbose
```
	
gambar 160
	
	
</details>
<details>
  <summary>Lab 5: IO Delays</summary>
 

### Lab 5: IO Delays
  
gambar 161
	
> IO Constraints
```
report_timing -from IN_A
report_timing
report_timing -to OUT_Y
```
	
gambar 162
	
> Listing all the ports
```
report_port -verbose
```
	
gambar 163
	
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

gambar 164
gambar 165 

> Hold timing
```
report_timing -from IN_A -trans -net -cap -nosplit -delay_type min
```
	
gambar 166
	
> Modelling the hold time 
```
set_input_delay -min 1 -clock [get_clocks MYCLK] [get_ports IN_A]
set_input_delay -min 1 -clock [get_clocks MYCLK] [get_ports IN_B]
report_timing -from IN_A -trans -net -cap -nosplit -delay_type min
```
	
gambar 167
	
> Setting new maximum value
```
set_input_delay -max 5 -clock [get_clocks MYCLK] [get_ports IN_A]
sh gvim a
```
	
gambar 168
	
> Setting input transition
```
set_input_transition -max 0.3 [get_ports IN_A]
set_input_transition -max 0.3 [get_ports IN_B]
set_input_transition -min 0.1 [get_ports IN_B]
set_input_transition -min 0.1 [get_ports IN_A]
report_timing -from IN_A -trans  -cap -nosplit > a_trans
```
	
gambar 169
	
> Setting output transition
```
set_output_delay -max 5 -clock [get_clocks MYCLK] [get_ports OUT_Y]
set_output_delay -min 1 -clock [get_clocks MYCLK] [get_ports OUT_Y]
report_timing -to OUT_Y
report_timing -to OUT_Y -cap -trans
```
	
gambar 170
	
> Setting for load
```
set_load -max 0.4 [get_ports OUT_Y]
report_timing -to OUT_Y -cap -trans
```
	
gambar 171
	
> Setting minimum delay
```
report_timing -to OUT_Y -cap -trans -delay min
```
	
> Setting another load for minimum delay
```
report_timing -to OUT_Y -cap -trans -delay min
```
	
gambar 172
	
</details>
<details>
  <summary>SDC Part3</summary>
 

### SDC Part3: Generated clock

**Notes**
gambar 173
	
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
	
gambar 174
gambar 175
	
</details>
<details>
  <summary>Lab 6: Generated clock</summary>
 

### Lab6: Generated clock

```
report_timing -to OUT_Y
```
	
gambar 176
	
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
	
gambar 177
	
> Modifying lab8_circuit.v
```
sh gvim DC_WORKSHOP/verilog_files/lab8_circuit.v
:vsp lab8_circuit_modified.v 
```
	
gambar 178
	
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
	
gambar 179
	
> Reporting the ports
```
report_port -verbose
```

The design is completely constrained

gambar 180
	
	
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
	
gambar 181
	
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
	
gambar 182
gambar 183
gambar 184
gambar 185
gambar 186
gambar 187
	
</details>
<details>
  <summary>Lab 7: Set_Max_delay</summary>
 

### Lab 7: Set_Max_delay

gambar 188
	
> Resetting the design to lab14_circuit.v
```
reset_design
sh gvim DC_WORKSHOP/verilog_files/lab14_circuit.v
read_verilog DC_WORKSHOP/verilog_files/lab14_circuit.v
source DC_WORKSHOP/verilog_files/lab8_cons.tcl 
report_timing								(Report timing in terms of gtech cells)
```
	
gambar 189
	
> Linking the design
```
link
compile_ultra
report_timing
```
	
gambar 190
	
> Checking the path whether it is constrained/not
```
get_ports *
report_timing -to OUT_Z
report_timing -from IN_C
```
	
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
	
gambar 192
	
> Optimizing the delay
	
```
compile_ultra
report_timing -to OUT_Z -sig 4
```
	
The tool is picking up other cells
	
gambar 193 

> Invoking design_vision
```
write -f ddc -out DC_WORKSHOP/verilog_files/lab14.ddc
reset_design
read_ddc DC_WORKSHOP/verilog_files/lab14.ddc
start_gui
```
	
gambar 194
	
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
	
gambar 195
	
```
create_clock -name MYVCLK -per 10
report_clocks
```
	
The highlighted one is virtual clock since it has no source
	
gambar 196
	
> Annotating IO Delays
```
set_input_delay -max 5 [get_ports IN_C] -clock [get_clocks MYVCLK]
set_input_delay -max 5 [get_ports IN_D] -clock [get_clocks MYVCLK]
set_output_delay -max 4.9 [get_ports OUT_Z] -clock [get_clocks MYVCLK]
report_timing
```
	
gambar 197
	
> Optimizing 
```
compile_ultra
report_timing -to OUT_Z -sig 4
report_port -verbose
```

gambar 198
	
</details>
