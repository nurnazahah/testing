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
	
**Sequential Logic Optimizations**
	
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
 

### Lab 1: Combinational Optimizations

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
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/opt_check3.v
link
compile
write -f ddc -out DC_WORKSHOP/verilog_files/opt_check3.ddc
	
read_ddc DC_WORKSHOP/verilog_files/opt_check3.ddc
```
	
gambar 20
	
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
	
```
report_area
