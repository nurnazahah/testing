## Day-10

### Topic: Quality Checks - Quality of Results (QOR)

<details>
  <summary>Report timing</summary>
 

### Lecture Report timing

**Generating timing reports**
```
report_timing -from DDF_A/clk
report_timing -from DDF_A/clk -to DDF_A/d
report_timing -fall_from DDF_A/clk
report_timing -rise_from DDF_b/clk
report_timing -delay_type min -to DDF_C/d
report_timing -delay_type min -through INV/a
report_timing -delay_type max -through AND/b
report_timing -rise_from DDF_b/clk -delay_type max -nets -cap -trans -sig 4             (delay_type max -> default if the delay type was not specified)
```
  
**Quick look at propagation delay**
  
gambar 2
  
**Timing path: Further details**
  
gambar 3
  
**Max path and Nworst**
  
gambar 4
</details>
  
<details>
  <summary>Lab 1</summary>
 

### Lab 1: Report timing
  
```
sh gvim DC_WORKSHOP/verilog_files/lab8_circuit_modified.v
:vsp DC_WORKSHOP/verilog_files/lab8_cons_modified.tcl
```
  
gambar 5

> Setup check
```
read_verilog DC_WORKSHOP/verilog_files/lab8_circuit_modified.v
link
source DC_WORKSHOP/verilog_files/lab8_cons_modified.tcl
report_timing -sig 4 -nosplit -trans -cap -input_pins -from IN_A > DC_WORKSHOP/verilog_files/t1.rpt
sh gvim DC_WORKSHOP/verilog_files/t1.rpt
```
  
gambar 6

> To check for the worst delay
```
report_timing -rise_from IN_A -sig 4 -transition_time -capacitance -input_pins > DC_WORKSHOP/verilog_files/t2.rpt
sh gvim DC_WORKSHOP/verilog_files/t2.rpt
:vsp DC_WORKSHOP/verilog_files/t1.rpt   
```
  
gambar 7
  
```
report_timing -rise_from IN_A -sig 4 -transition_time -capacitance -input_pins -to REGA_reg/D > DC_WORKSHOP/verilog_files/t3.rpt
sh gvim DC_WORKSHOP/verilog_files/t3.rpt
:vsp DC_WORKSHOP/verilog_files/t1.rpt  
```
  
gambar 8
  
> Hold check
```
report_timing -delay min -from  IN_A
```
  
gambar 9
  
> Report timing through U15/Y
```
report_timing -through U15/Y
report_timing -delay min -through U15/Y
```
  
gambar 10
  
</details>
  
<details>
  <summary>Lab 2</summary>
 

### Lab 2: Lab Check_timing, Check_design, Set_max_capacitance, HFN
  
```
read_verilog DC_WORKSHOP/verilog_files/lab8_circuit_modified.v
link
check_design
compile_ultra
check_timing                (Checking the design is properly constrained or not)
report_constraints          (Default constraints loaded in the tool memory)
source DC_WORKSHOP/verilog_files/lab8_cons_modified.tcl
check_timing
report_timing
```
  
gambar 11
  
```
report_constraints
```
  
gambar 12
  
```
reset_design
sh gvim DC_WORKSHOP/verilog_files/mux_generate.v
:vsp DC_WORKSHOP/verilog_files/mux_generate_128_1.v  
```
  
gambar 13
  
```
sh gvim DC_WORKSHOP/verilog_files/mux_generate.v 
:vsp DC_WORKSHOP/verilog_files/mux_generate_128_1.v
read_verilog DC_WORKSHOP/verilog_files/mux_generate_128_1.v 
link
compile_ultra
write -f verilog -out DC_WORKSHOP/verilog_files/mux_generate_128_1_net.v
sh gvim DC_WORKSHOP/verilog_files/mux_generate_128_1_net.v
```

gambar 14
  
```
get_cells * -hier -filter "is_sequential == true"           (No output since there is no sequential cells)
get_cells * -hier -filter "is_sequential == false"
report_timing -net -cap -sig 4
check_timing
```
 
> Setting the constraints
```
set_max_delay -from [all_inputs] -to [all_outputs] 3.5
report_timing
```
  
gambar 15
  
> Setting max capacitance
```
set_max_capacitance 0.025 [current_design]
report_constraint -all_violators
compile_ultra
check_timing 
report_constraints
report_timing
report_timing -net -cap -sig 4
```
  
gambar 16
  
> New design
```
sh gvim DC_WORKSHOP/verilog_files/en_128.v
reset_design 
read_verilog DC_WORKSHOP/verilog_files/en_128.v
link
compile_ultra
report_timing -from en -inp -nets -cap
set_max_capacitance 0.03 [current_design]                       (Breaking/buffering HFN)
report_constraints
compile_ultra 
report_timing -from en -inp -nets -cap -sig 4
write -f ddc -out  DC_WORKSHOP/verilog_files/en_128.ddc
```
  
> Invoking design vision
```
design_vision
read_ddc DC_WORKSHOP/verilog_files/en_128.ddc
start_gui
```
  
gambar 18
  
> Setting the max transition 
```
report_timing -from en -nets -cap -sig 4 -trans
set_max_transition 0.150 [current_design]
report_constraints
report_constraint -all_violators
```
  
gambar 19
  
```
compile_ultra
report_constraint -all_violators
report_timing -inp -nets -cap -trans -sig 4 -nosplit
report_timing -inp -nets -cap -trans -sig 4 -nosplit -from en -to y[116]
```
  
gambar 20
  
**Summary**

* check_design 
  + To ensure the quality of the design is proper, to check if the design is proper/not, or if there are any missing things in the design.
* check_timing 
  + To ensure all the proper constraints are in place.
* report_constraints 
  + To check what are the constraints we are checking. If there is any violation, it will report it.
* set_max_capacitance & set_max_transition 
  + To check if there is any high fanout net (HFN) or broken or is the design is buffered properly, so that there are no transition issues.
  
**Summarizing**
  
**Important things should be there in synthesis script -> synth.tcl**

* Constraints
  + clock, genclk (if any)
  + vclk (if any)
  + Practicalities of the clock -> uncertainty including skew + jitter for pre CTS latency and jitter only for post CTS & latency
  + Input delay
  + Output delay
  + Input trans/set_driving _cell
  + Output load
  + set_max_area
  + set_max_capacitance
  + set_max_transition

* Synthesis Knobs
  + Boundary Optimization
  + Retiming 
  + Constant propagation 
  + Unused flop removal 
  + Isolate ports 

  
Note: All of those things must be needed in synthesis or script.tcl

  
**After sourcing all those contents, the program needs to:**

* read_verilog
* read_db
* check_design
* source constraints
* check_timing
* compile_ultra
* report_constraint (for all_violations)
* report_area
</details>
  
<details>
  <summary>Timing Model</summary>
 

### Timing Model: ETM and QTM
  
**QTM and ETM in VLSI**
  
**Introduction**
In design, once a block has met its timing constraints at gate level, the detailed internal timing of the block is not needed for chip-level timing analysis. For that, we can create/write a timing model for the block.
  
*Source: http://www.vlsi-expert.com/2011/02/etm-extracted-timing-models-basics.html#:~:text=Extracted%20Timing%20Models%20(ETM)%20in,QTM)%20for%20top%2Ddown%20design 
  
**What is timing models?**
  
* It is used to perform STA on a chip using Primetime since every leaf cell must have a timing model.
* For STA purposes, a leaf cell can be a simple macro cell i.e. NAND, NOR, or flip flop, or a complex block i.e. RAM/microprocessor.
* Hierarchical STA flow allows us to partition different blocks using timing models which should completely model the full input/output timing characteristics without requiring a complete netlist of the block and not modelling every path in the block.
* Hierarchical STA reduces runtime and memory usage as compared to flat STA. The actual runtime savings depending on the design complexity.
  
  
**Types of timing models**
* Extracted Timing Model (ETM)
* Quick Timing Model (QTM)
  
*Source: http://mantravlsi.blogspot.com/2014/06/timing-models-etm-qtm-ilm.html 

  
**Extracted Timing Model (ETM)**

* Block-based model (.lib)
* Contents of the block are hidden
* Original netlist is replaced by model containing timing arcs for the block interfaces
* NLDM lookup tables are extracted for each timing arcs
* These arcs whose delay a function of input transition and output load. This makes ETM usable with different input transition times and different output loads.
* Multiple modes per model
* Single PVT per model
* Used for implementation (not sign-off) of IP models. The contents are protected since the model contains abstracted timing information, without any netlist information
  
**ETM model illustration**

*Source: http://www.vlsi-expert.com/2011/02/etm-extracted-timing-models-basics.html#:~:text=Extracted%20Timing%20Models%20(ETM)%20in,QTM)%20for%20top%2Ddown%20design 
  
![image](https://user-images.githubusercontent.com/118953917/210180743-50a8facd-28c1-4bdf-ab26-7122d454c532.png)

**Quick Timing Model (QTM)**
In the early stages of the design cycle, if a block does not have a netlist yet, we can use a QTM to decsribe its initial timing. Later in the cycle, we can replace each QTM with a netlist block to obtain more accurate timing.
  
*Source: http://mantravlsi.blogspot.com/2014/06/timing-models-etm-qtm-ilm.html 
  
* QTM is generated by the command create_qtm_model, followed by save_qtm_model
  
*Source: http://tech.tdzire.com/what-are-etms-qtms-and-ilms/
