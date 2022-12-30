## Day-9

### Topic: Optimizations

<details>
  <summary>Optimizations Combinational Opt</summary>
 

### Optimizations Combinational Opt

**Advanced synthesis and STA using Design Compiler**
	
**Logic optimizations**
	
**What are the optimization goals?**
* Cost function based optimizations
	+ Optimization till the cost is met
	+ Over optimization of one goal will harm other goals 
	+ Goals for synthesis: ensure to meet timing, area, and power (Those are 3 metrics of netlist which will be contradictory)
	
**Combinational Logic Optimisation**
* To squeeze the logic to get the most optimised design in terms of Area and Power saving
* Constant propagation technique used: Direct optimisation
* Boolean logic optimisation technique: K-map and Quine McKluskey
	
gambar 2-6
	
</details>
	
<details>
  <summary>Sequential Logic Optimizations</summary>
 

### Sequential Logic Optimizations
	
* Basic
	+ Sequential constant propagation
	+ Retiming
	+ Unused flop removal
	+ Clock gating
	
* Advanced 
	+ State optimisation
	+ Sequential logic cloning (Floor plan aware synthesis)
	
**Sequential constant**
	
gambar 7-11
	
**Controlling sequential optimizations in DC**
```
compile_seqmap_propagate_constants 
compile_delete_unloaded_sequential_cells
compile_register_replication						(For cloning the registers)
```
</details>
	
<details>
  <summary>Lab 1 part 1</summary>
 

### Lab 1: Combinational Optimizations part 1

> Setup environment for dc_shell
```
/nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/sky130RTLDesignAndSynthesisWorkshop
csh
dc_shell
```
	
> To view the scripting of all opt_checks
```
sh gvim DC_WORKSHOP/verilog_files/opt_check*.v -o
```

gambar 12-16

> opt_check
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/opt_check.v
report_timing
link 
compile
report_timing
get_cells *
report_timing -to y2
report_timing -to y1
write -f ddc -out DC_WORKSHOP/verilog_files/opt_check.ddc
```
	
```
design_vision
read_ddc DC_WORKSHOP/verilog_files/opt_check.ddc
```

gambar 17
gambar 18 
	
> opt_check2
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/opt_check2.v
link 
compile
write -f ddc -out DC_WORKSHOP/verilog_files/opt_check2.ddc
	
read_ddc DC_WORKSHOP/verilog_files/opt_check2.ddc				(In design_vision)
```
	
gambar 19
	
> opt_check3
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/opt_check3.v
link
compile
write -f ddc -out DC_WORKSHOP/verilog_files/opt_check3.ddc
	
read_ddc DC_WORKSHOP/verilog_files/opt_check3.ddc
```
	
gambar 20
	
> opt_check4
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/opt_check4.v
link
compile
write -f ddc -out DC_WORKSHOP/verilog_files/opt_check4.ddc
	
read_ddc DC_WORKSHOP/verilog_files/opt_check4.ddc
```
	
gambar 21
	
```
report_timing -to y
set_max_delay .06 -from [all_inputs] -to [get_ports y]
report_timing
report_timing -sig 4
compile_ultra
report_timing
get_lib_cells */sky130_fd_sc_hd__xnor2*
size_cell U3 sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__xnor2_4
report_timing
compile_ultra
```
	
gambar 22

</details>
	
<details>
  <summary>Lab 1 part 2</summary>
 

### Lab 1: Combinational Optimizations part 2
```
sh gvim DC_WORKSHOP/verilog_files/resource_sharing_mult_check.v
read_verilog DC_WORKSHOP/verilog_files/resource_sharing_mult_check.v
link
compile_ultra
write -f ddc -out DC_WORKSHOP/verilog_files/resource_sharing_mult_check.ddc
	
reset_design
read_ddc DC_WORKSHOP/verilog_files/resource_sharing_mult_check.ddc
```
	
gambar 23

> In design_vision
```
report_area
report_timing
set_max_delay -from [all_inputs] -to [all_outputs] 2.5
report_timing
compile_ultra
report_timing
```	
	
gambar 24
gambar 25
	
	
```
report_area
set_max_delay -from sel -to [all_outputs] 0.1
report_timing
compile_ultra
report_area
report_timing
report_timing -sig 4
```
	
gambar 26
gambar 27
	
```
set_max_area 800
compile_ultra
report_timing
report_area
```
	
gambar 28
gambar 29
	
</details>
	
<details>
  <summary>Lab 2</summary>
 

### Lab 2: Sequential Optimizations

```
sh gvim DC_WORKSHOP/verilog_files/dff_cons* -o
```
	
gambar 30-35
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/dff_const1.v
link
compile
get_cells

foreach_in_collection my_cell [get_cells *] {
set cell_name [get_object_name $my_cell];
echo $cell_name; 
}
	
foreach_in_collection my_cell [get_cells *] {
set cell_name [get_object_name $my_cell];
set rn [get_attribute [get_cells $cell_name] ref_name];  
echo $cell_name $rn; 
}
```
	
gambar 36 
	
```
design_vision
read_verilog DC_WORKSHOP/verilog_files/dff_const1.v
link
compile
```

gambar 37
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/dff_const2.v
link
compile
```
	
gambar 38
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/dff_const3.v
link
compile
```
	
gambar 39
	
```
reset_design 
read_verilog DC_WORKSHOP/verilog_files/dff_const2.v
set compile_seqmap_propagate_constants false
link
compile
start_gui
```
	
gambar 40
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/dff_const4.v
link
set compile_seqmap_propagate_constants true
compile_ultra
start_gui
```
	
gambar 41
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/dff_const5.v
link
compile
```
	
gambar 42
	
</details>
	
<details>
  <summary>Special Optimizations</summary>
 

### Special Optimizations

**Register retiming**
	
**Why flops were added?**
* To reduce the number of delay 
	+ From the figure, each flop has 47 ns slack. So, the combinational logic is divided to some partitions between the flops resulting those slacks of the critical path to be decreased as well as making the freaquency increases.
	+ This is what we called Register Retiming
	
gambar 43
	
**Boundary Optimization**
	
**How to control boundary optimization?**
```
set_boundary_optimization <design><true|false>
set_boundary_optimization module_sub false
```
	
gambar 44
	
**Multi-cycle path**
	
gambar 45
	
**False path**
	
> Paths that are not valid for STA
```
set false_path -from <> -to <>
set false_path -through <>
```

gambar 46
	
**External load vs internal load**
	
gambar 47
	
</details>
	
<details>
  <summary>How Paths are timed MCP?</summary>
 

### How Paths are timed Multi-Path Cycle?
	
**How DC/STA tool checks timing?**
	
**Single cycle path**
	
gambar 48
	
**Half cycle path**
	
gambar 49
	
**Multi cycle path**
	
gambar 50
	
</details>
	
<details>
  <summary>Lab 3</summary>
 

### Lab 3: Boundary Optimization
	
```
sh gvim DC_WORKSHOP/verilog_files/check_boundary.v
```
	
gambar 51 
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/check_boundary.v
link
compile_ultra
write -f ddc -out boundary.ddc
get_cells
```
	
```
design_vision
read_ddc DC_WORKSHOP/verilog_files/boundary.ddc
start_gui
```
	
gambar 52

> In design_vision
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/check_boundary.v
link
get_cells							(If command in dc_shell, the tool cannot find the matching objects because the boundary optimization has happened, that's why the hierarchy got dissoved)
set_boundary_optimization u_im false
compile_ultra
start_gui
```
	
gambar 53
</details>
	
<details>
  <summary>Lab 4</summary>
 

### Lab 4: Register Retiming

> In design_vision
```
reset_design
sh gvim DC_WORKSHOP/verilog_files/check_reg_retime.v
```
	
gambar 54
	
```
read_verilog DC_WORKSHOP/verilog_files/check_reg_retime.v
link
compile
start_gui
```

gambar 55
	
```
report_timing
sh gvim DC_WORKSHOP/verilog_files/reg_retime_cons.tcl			(Setting the constraints)
source DC_WORKSHOP/verilog_files/reg_retime_cons.tcl
report_clocks
report_timing
compile_ultra -retime
start_gui
```
	
gambar 56
gambar 57
	
```
report_timing
compile_ultra
report_timing -from [all_inputs]
report_timing -from [all_inputs] -trans -cap -nosplit -sig 4
```
	
slide 58
	
</details>
	
<details>
  <summary>Lab 5</summary>
 

### Lab 5: Isolating Output Ports
	
slide 59
	
```
sh gvim DC_WORKSHOP/verilog_files/check_boundary.v
```
	
slide 60
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/check_boundary.v
link
compile_ultra
start_gui
```
	
gambar 61
	
```
set_isolate_ports -type buffer [all_outputs]
compile_ultra
start_gui
```
	
gambar 62
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/check_boundary.v
link
compile_ultra
create_clock -per 5 -name myclk [get_ports clk]
set_input_delay -max 2 [all_inputs] -clock myclk
set_output_delay -max 2 [all_outputs] -clock myclk
set_load -max 0.3 [all_outputs]
report_timing -nosplit -inp -cap -trans -sig 4
report_timing -to val_out_reg[0]/D -inp -trans -nosplit -cap -sig 4		(Viewing report timing before isolating portr -> reg to reg path)
```
	
> Isolating ports of both paths
```
set_isolate_ports -type buffer [all_outputs]
compile_ultra
report_timing -nosplit -inp -cap -trans -sig 4
report_timing -from val_out_reg[0]/CLK -to val_out_reg[0]/D -nosplit -inp -cap -trans -sig 4
```
	
gambar 64
	
</details>
	
<details>
  <summary>Lab 6</summary>
 

### Lab 6: Multi Cycle Path
	
```
sh gvim DC_WORKSHOP/verilog_files/mcp_check.v
```
	
gambar 65
	
```

	
