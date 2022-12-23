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

gambar 129

**Clock generation**

* Oscillator
* Phase-Locked Loop (PLL) 
* External clock source
* All of the above clock sources have inherent variations in the clock period due to stochastic effects

gambar 130
  
**Clock distribution**
  
Jitter: Stochastic variations of clock generation
  
gambar 131
  
**Clock skew**
  
* Clock Tree built during CTS -> Practical Clock Tree
  
* Logic optimization happens in synthesis -> Ideal Clock Tree
  
* Timing Clean path in synthesis may fail after STA
  
gambar 132
  
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
  
gambar 133
  </details>
<details>
  <summary>SDC Part2</summary>
 

### SDC Part1: IO Delays
  
**Advanced Synthesis and STA using Design Compiler**

**Advanced constraints, specifying constraints through Synopsys Design Constraints (SDC)**

**How to constraint the design in DC?**
  
* DC takes constraints in the form of SDC 
  
gambar 134
  
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
  
gambar 135 

**Clock distribution**
  
gambar 136
  
Bringing the practicalities (latency, uncertainty) of clock network
  
gambar 137
  
**Clocks: Waveform**
  
gambar 138
  
**Constraining IO paths**
  
gambar 139
  
gambar 140
  
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
  
gambar 141 
  
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
gambar 142

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

gambar 143

> Get cells
```
get_cells *
get_attribute [get_cells U9] is_hierarchical                        (Checking the desired cell is hierarchical/not)
get_cells * -hier -filter "is_hierarchical == false"                (Listing the false hierarchical cell)
get_cells * -hier -filter "is_hierarchical == true"                 (Listing the true hierarchical cell)
```
  
gambar 144
  
  
> Get reference name of the cells
```
get_attribute [get_cells REGA_reg] ref_name                         (Getting the reference name of the cell)
  
foreach_in_collection my_cell [get_cells * -hier] {                 (Listing cell name and its reference name)
set my_cell_name [get_object_name $my_cell];
set rname [get_attribute [get_cells $my_cell_name] ref_name];
echo $my_cell_name $rname;
} 
```

gambar 145

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
gambar 146 
gambar 147

Note: In digital design, net can only have one driver
  
Example: 
  
gambar 148
  
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
  
gambar 149
  
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

  gambar 150
  
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
  
gambar 151 
