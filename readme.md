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

![image](https://user-images.githubusercontent.com/118953917/210128758-35553b5a-2243-4037-9d86-f8b2faea5bb7.png)

![image](https://user-images.githubusercontent.com/118953917/210128764-1951205b-1981-4de9-bb02-9a5b572b848c.png)

![image](https://user-images.githubusercontent.com/118953917/210128770-64515713-e924-4f06-b958-3f109930e33c.png)

![image](https://user-images.githubusercontent.com/118953917/210128777-5bae9223-396a-4970-8517-c759a5a57a44.png)

![image](https://user-images.githubusercontent.com/118953917/210128795-953d1979-6e07-414a-a21f-33524fcf9b87.png)
	
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
	
![image](https://user-images.githubusercontent.com/118953917/210128815-1bca84f4-4a26-427d-81d4-764828eaf89a.png)

![image](https://user-images.githubusercontent.com/118953917/210128821-759b7612-292b-494e-a093-e2915692edde.png)

![image](https://user-images.githubusercontent.com/118953917/210128828-515c10f8-a263-4d55-a6b8-b2f40fd4f8db.png)

![image](https://user-images.githubusercontent.com/118953917/210128832-16e6a103-a4d4-4193-8100-574cb746be72.png)

![image](https://user-images.githubusercontent.com/118953917/210128855-b14d51c7-5f77-4554-a0e0-b55ddd245ed6.png)


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

![image](https://user-images.githubusercontent.com/118953917/210128871-3ea71ff9-a112-4733-a0a8-4649b0f7bab1.png)

![image](https://user-images.githubusercontent.com/118953917/210128876-23527a11-f377-4f4e-9ee8-d2ab6cee034e.png)

![image](https://user-images.githubusercontent.com/118953917/210128882-6894dc44-8234-47cc-a999-88c5d83ee1d1.png)

![image](https://user-images.githubusercontent.com/118953917/210128889-ed74e739-e06a-493c-9d5a-bfb7b3f43fbe.png)

![image](https://user-images.githubusercontent.com/118953917/210128896-339c6be9-0b3f-4124-8a07-d43ffe0ff5be.png)

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

![image](https://user-images.githubusercontent.com/118953917/210128910-d17033bf-5c43-41d0-874d-77557f1cb14b.png)

![image](https://user-images.githubusercontent.com/118953917/210128922-b3b5c561-2dcc-414b-a0c7-fa18d16fe87d.png)
	
> opt_check2
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/opt_check2.v
link 
compile
write -f ddc -out DC_WORKSHOP/verilog_files/opt_check2.ddc
	
read_ddc DC_WORKSHOP/verilog_files/opt_check2.ddc				(In design_vision)
```
	
![image](https://user-images.githubusercontent.com/118953917/210128939-b1e06619-f654-4358-a949-bcd49469be18.png)
	
> opt_check3
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/opt_check3.v
link
compile
write -f ddc -out DC_WORKSHOP/verilog_files/opt_check3.ddc
	
read_ddc DC_WORKSHOP/verilog_files/opt_check3.ddc
```
	
![image](https://user-images.githubusercontent.com/118953917/210128945-398a1927-f7c0-4677-aea4-78e96e7760ff.png)
	
> opt_check4
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/opt_check4.v
link
compile
write -f ddc -out DC_WORKSHOP/verilog_files/opt_check4.ddc
	
read_ddc DC_WORKSHOP/verilog_files/opt_check4.ddc
```
	
![image](https://user-images.githubusercontent.com/118953917/210128956-87cafbcf-13ed-43d3-9644-50e9c5dff9bc.png)
	
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
	
![image](https://user-images.githubusercontent.com/118953917/210128969-28586244-5dd1-4662-b4e0-471da87b2636.png)

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
	
![image](https://user-images.githubusercontent.com/118953917/210128975-b4d48749-8ce0-44f9-adc1-eaacab5a7fdc.png)

> In design_vision
```
report_area
report_timing
set_max_delay -from [all_inputs] -to [all_outputs] 2.5
report_timing
compile_ultra
report_timing
```	
	
![image](https://user-images.githubusercontent.com/118953917/210128981-40dd8c6c-02b4-4893-b8c6-cf875a5c90ab.png)
	
![image](https://user-images.githubusercontent.com/118953917/210128989-699fd768-98fe-4319-9d27-4f7828cf332a.png)
	
	
```
report_area
set_max_delay -from sel -to [all_outputs] 0.1
report_timing
compile_ultra
report_area
report_timing
report_timing -sig 4
```
	
![image](https://user-images.githubusercontent.com/118953917/210128995-dd719518-7b99-4628-a23c-34dda6237bb9.png)
	
![image](https://user-images.githubusercontent.com/118953917/210128998-5509f3be-875a-484c-8de8-3a9e3c39c2aa.png)
	
```
set_max_area 800
compile_ultra
report_timing
report_area
```
	
![image](https://user-images.githubusercontent.com/118953917/210129008-eddc3233-961e-4cf0-876c-28fc10dcc449.png)
	
![image](https://user-images.githubusercontent.com/118953917/210129015-309304f1-5046-4cee-875d-d108c5dda74f.png)
	
</details>
	
<details>
  <summary>Lab 2</summary>
 

### Lab 2: Sequential Optimizations

```
sh gvim DC_WORKSHOP/verilog_files/dff_cons* -o
```

![image](https://user-images.githubusercontent.com/118953917/210129023-fe573204-17ea-44df-8bd3-5d282c6248cc.png)

![image](https://user-images.githubusercontent.com/118953917/210129031-1ee147c2-b401-4a76-88e7-9e66ae5d41d7.png)

![image](https://user-images.githubusercontent.com/118953917/210129040-a0ccfd74-8332-47f3-bd99-fc71311983dc.png)

![image](https://user-images.githubusercontent.com/118953917/210129045-06351669-d469-4fe0-a912-2386327cfee1.png)

![image](https://user-images.githubusercontent.com/118953917/210129051-bad7474a-e2f7-481e-bf68-7bde38fc2644.png)

![image](https://user-images.githubusercontent.com/118953917/210129054-8a75683b-c0d0-4f2b-8152-049c37eb12c7.png)

	
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
	
![image](https://user-images.githubusercontent.com/118953917/210129056-fc0596f3-4410-49fd-9cb1-e9cecb5f0843.png)
	
```
design_vision
read_verilog DC_WORKSHOP/verilog_files/dff_const1.v
link
compile
```

![image](https://user-images.githubusercontent.com/118953917/210129067-a967fa54-9528-4b2a-8f3b-77b2b4ff7d30.png)
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/dff_const2.v
link
compile
```
	
![image](https://user-images.githubusercontent.com/118953917/210129072-180059aa-3718-4d01-b7a1-df96e859c96c.png)
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/dff_const3.v
link
compile
```
	
![image](https://user-images.githubusercontent.com/118953917/210129088-aaf3f023-1ed4-485d-b9a1-4c23eabe6a06.png)
	
```
reset_design 
read_verilog DC_WORKSHOP/verilog_files/dff_const2.v
set compile_seqmap_propagate_constants false
link
compile
start_gui
```
	
![image](https://user-images.githubusercontent.com/118953917/210129098-a09c0313-2c77-451d-b9c7-ca99ead9e6cb.png)
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/dff_const4.v
link
set compile_seqmap_propagate_constants true
compile_ultra
start_gui
```
![image](https://user-images.githubusercontent.com/118953917/210129122-5e28bc24-7a91-4731-bcdd-b77b07e70ec6.png)	
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/dff_const5.v
link
compile
```
	
![image](https://user-images.githubusercontent.com/118953917/210129118-2595f2f6-254e-4b28-a59e-de91cb89104a.png)
	
</details>
	
<details>
  <summary>Special Optimizations</summary>
 

### Special Optimizations

**Register retiming**
	
**Why flops were added?**
* To reduce the number of delay 
	+ From the figure, each flop has 47 ns slack. So, the combinational logic is divided to some partitions between the flops resulting those slacks of the critical path to be decreased as well as making the freaquency increases.
	+ This is what we called Register Retiming
	
![image](https://user-images.githubusercontent.com/118953917/210129133-2c7c49be-7238-4275-8fcb-b0e75d12f81d.png)
	
**Boundary Optimization**
	
**How to control boundary optimization?**
```
set_boundary_optimization <design><true|false>
set_boundary_optimization module_sub false
```
	
![image](https://user-images.githubusercontent.com/118953917/210129139-17283ab1-ac0b-4a49-8444-c204ce7c5178.png)
	
**Multi-cycle path**
	
![image](https://user-images.githubusercontent.com/118953917/210129152-c56fe10f-01b2-4632-89b1-16f84fdad647.png)
	
**False path**
	
> Paths that are not valid for STA
```
set false_path -from <> -to <>
set false_path -through <>
```

![image](https://user-images.githubusercontent.com/118953917/210129162-9270bdb6-f3ab-4a02-a3f3-6e4224b00cc0.png)
	
**External load vs internal load**
	
![image](https://user-images.githubusercontent.com/118953917/210129172-98810ca9-7ba1-4ba3-bdec-71c06e0ab1e9.png)
	
</details>
	
<details>
  <summary>How Paths are timed MCP?</summary>
 

### How Paths are timed Multi-Path Cycle?
	
**How DC/STA tool checks timing?**
	
**Single cycle path**
	
![image](https://user-images.githubusercontent.com/118953917/210129180-1d16a1d7-6062-4f6a-a048-0c5910aa2d84.png)
	
**Half cycle path**
	
![image](https://user-images.githubusercontent.com/118953917/210129190-e88b82dc-4918-4be9-a5e8-071a250d808a.png)
	
**Multi cycle path**
	
![image](https://user-images.githubusercontent.com/118953917/210129203-cfb5b40c-d864-4328-99fa-c7fa30157661.png)
	
</details>
	
<details>
  <summary>Lab 3</summary>
 

### Lab 3: Boundary Optimization
	
```
sh gvim DC_WORKSHOP/verilog_files/check_boundary.v
```
	
![image](https://user-images.githubusercontent.com/118953917/210129207-36caf3a2-3403-47ea-a4d6-d315fd9c31c7.png)
	
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
	
![image](https://user-images.githubusercontent.com/118953917/210129215-8dac93db-be0e-42aa-968c-97f725fe388b.png)

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
	
![image](https://user-images.githubusercontent.com/118953917/210129224-d3fc3603-2128-4a3e-a2cc-5c4569fe6da4.png)
	
</details>
	
<details>
  <summary>Lab 4</summary>
 

### Lab 4: Register Retiming

> In design_vision
```
reset_design
sh gvim DC_WORKSHOP/verilog_files/check_reg_retime.v
```
	
![image](https://user-images.githubusercontent.com/118953917/210129235-3bf86683-725f-4489-8f62-8c0f3bb22265.png)
	
```
read_verilog DC_WORKSHOP/verilog_files/check_reg_retime.v
link
compile
start_gui
```

![image](https://user-images.githubusercontent.com/118953917/210129248-c6cbeef4-dd0c-4b9c-982a-dbf72ffd6918.png)
	
```
report_timing
sh gvim DC_WORKSHOP/verilog_files/reg_retime_cons.tcl			(Setting the constraints)
source DC_WORKSHOP/verilog_files/reg_retime_cons.tcl
report_clocks
report_timing
compile_ultra -retime
start_gui
```
	
![image](https://user-images.githubusercontent.com/118953917/210129252-5c47dbc5-919d-4f9c-82f7-d6e6a00f3071.png)
	
![image](https://user-images.githubusercontent.com/118953917/210129264-003c2885-7411-4205-806d-c2b48f2310dd.png)
	
```
report_timing
compile_ultra
report_timing -from [all_inputs]
report_timing -from [all_inputs] -trans -cap -nosplit -sig 4
```
	
![image](https://user-images.githubusercontent.com/118953917/210129279-a1702c7a-c93d-4161-9d6c-e31511294e55.png)
	
</details>
	
<details>
  <summary>Lab 5</summary>
 

### Lab 5: Isolating Output Ports
	
![image](https://user-images.githubusercontent.com/118953917/210129288-4ca80cf2-e625-473c-bc20-cc796da8c3b0.png)
	
```
sh gvim DC_WORKSHOP/verilog_files/check_boundary.v
```
	
![image](https://user-images.githubusercontent.com/118953917/210129293-21564a49-bcb3-4ca5-9af0-0de446102049.png)
	
```
reset_design
read_verilog DC_WORKSHOP/verilog_files/check_boundary.v
link
compile_ultra
start_gui
```
	
![image](https://user-images.githubusercontent.com/118953917/210129298-f60dfe27-c494-43da-a705-33332670f6a8.png)
	
```
set_isolate_ports -type buffer [all_outputs]
compile_ultra
start_gui
```
	
![image](https://user-images.githubusercontent.com/118953917/210129305-8efa8dbf-2af4-42e1-a7ab-d447b9bbdfc8.png)
	
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
read_verilog DC_WORKSHOP/verilog_files/mcp_check.v
link
compile_ultra
sh gvim DC_WORKSHOP/verilog_files/mcp_check_cons.tcl
source DC_WORKSHOP/verilog_files/mcp_check_cons.tcl
report_timing
compile_ultra
report_timing
set_multicycle_path -setup 2 -to prod_reg[*]/D -from [all_inputs] 	(Must put all_inputs -> valid to prod_reg is a single cycle path. If [all_inputs] didn't there, it will mcp the valid to prod_reg path too. This is a CRIMINAL MISTAKE!)
report_timing -to prod_reg[*]/D -from valid_reg/CLK
report_clock *
```

> Applying MCP
```
report_timing -to prod_reg[*]/D -from [all_inputs]
```

gambar 66
gambar 67
	
> Hold time
```
report_timing -delay min
```
	
> Applying MCP for hold time
```
set_multicycle_path -hold 1 -from [all_inputs] -to prod_reg[*]/D
report_timing -delay min -to prod_reg[*]/D -from [all_inputs]
```
	
> Isolating ports in report timing
```
report_timing -nosplit -inp -cap -trans -sig 4
```

gambar 68
	
</details>


	
