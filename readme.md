## Day-18

### Topic: Pre-layout timing analysis and importance of good clock tree

### Timing modelling using delay tables
<details>
  <summary>Lab 1:  Lab steps to convert grid info to track info</summary>
 
### Lab steps to convert grid info to track info
  
**Library Exchange Format (LEF)**
  
* A specification in which representing the physical layout of an integrated circuit in an ASCII format
* It includes design rules and abstract information about the standard cells
* LEF only has basic information required at that level to serve the purpose of the concerned CAD tool
* Containing information on input, output, power and group port, does not consists logic path information
  
* Objective: extract LEF file from .mag file and then plug the file into the picorv32a flow (previous is standard cell library)
* Main guidelines:
  + The input and output ports must lie on the intersection of the vertical and horizontal tracks
  + The width of standard cell should be on the track pitch, and the height should be on the track vertical pitch
  
```
~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd
vim tracks.info
```
  
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
magic -T sky130A.tech sky130_inv.mag
```
  
* Track information (using during routing stage) routes can go over the track/layer (metal traces)
  
![image](https://user-images.githubusercontent.com/118953917/214740911-da449828-590d-499e-8e84-5dbe2c0ac156.png)
  
* Set the grid based on the tracks.info (grid) converted the track into grid
  
![image](https://user-images.githubusercontent.com/118953917/214740963-2d493469-4489-4ba8-9ef9-7ee97563f49b.png)

* Ports are on the intersection of the horizontal and vertical tracks. It ensures that the routes can reach the ports from x and y direction.
* Verified that both input and output ports have fulfilled the guideline where input and output ports lies at the intersection of horizontal and vertical tracks

*Note: press "g" to enable grid (zoom in to see the grid)*
  
![image](https://user-images.githubusercontent.com/118953917/214741494-d1a5f5e6-5295-45ff-ba2e-ad8eac2fe210.png)

</details>

<details>
  <summary>Lab 2:  Lab steps to convert magic layout to std cell LEF</summary>
 
### Lab steps to convert magic layout to std cell LEF
  
* We need to only define layers, not ports in layout
* Ports definitions are required when we want to extract LEF file
  + Ports will be defined as pins of a macro
  
**How to define ports?**

* Select port --> edit --> Text --> fill those required information 
  
*Note: For A; and Y in locali while for VPWR and VGND in metal1*

![image](https://user-images.githubusercontent.com/118953917/214749913-cb884945-e9a5-4c44-b10c-8cb3983560ed.png)
  
* Then, define classes of ports 
  
![image](https://user-images.githubusercontent.com/118953917/214756209-3b67c804-59ed-42a8-83f3-eb585339b834.png)
  
* Extract the LEF file 
  
> In tkcon 
```
save sky130A_vsdinv.mag
```
  
> Checking the saved file
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
ls
```
  
![image](https://user-images.githubusercontent.com/118953917/214787867-9cf85de0-99f3-4db2-89fd-eec1c064ed29.png)
  
```
magic -T sky130A.tech sky130_vsdinv.mag
```

> In tkcon
```
lef write
```
  
![image](https://user-images.githubusercontent.com/118953917/214751397-07e1287d-a91d-49b5-b624-25878e3759c0.png)

```
vim sky130_vsdinv.lef
```
  
![image](https://user-images.githubusercontent.com/118953917/214756411-087a7a3c-a8f0-4f55-86f4-9453d1feb89f.png)

</details>

<details>
  <summary>Lab 3:  Introduction to timing libs and steps to include new cell in synthesis</summary>
 
### Introduction to timing libs and steps to include new cell in synthesis
  
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
cp sky130A_vsdinv.lef /home/nur.nazahah.mohd.amri/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src
cd ~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/libs
cp sky130_fd_sc_hd__* /home/nur.nazahah.mohd.amri/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/
vim config.tcl
```
  
**Modifying config.tcl file**
  
![image](https://user-images.githubusercontent.com/118953917/214758920-88489361-6f99-4a0f-8e41-ffa089a16269.png)

```
cd ~/Desktop/work/tools/openlane_working_dir/openlane
make mount
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a -tag 13-01_14-09 -overwrite      (Check run date at ls ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs)
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis
```
  
Successfully invoke openlane and doing synthesis
  
![image](https://user-images.githubusercontent.com/118953917/214760516-fc95cafa-9f17-4f92-8803-9819361bf9a0.png)

</details>

<details>
  <summary>Theory 1:  Introduction to delay tables</summary>
 
### Introduction to delay tables

* With the use of gates for clock nets, such as shown in the below figure, which is the method known as clock gating, we can ensure no dynamic switching and short circuit power consumed by the clock tree, during the time the clock gets gated.

![image](https://user-images.githubusercontent.com/118953917/214761577-5e776c52-2f13-4e4d-ba3a-b35145638a68.png)

* We need to look into the timing characteristics of the buffer, in the case where we want to swap out the buffer for a gate. 
* For each level of buffering, we should have an identical buffer being used, and each node should be driving the same node. 
* Keep in mind that the load at the output will be varying, and since the load of one buffer is varying, the input transition of the following buffer will also vary. 
* This means that we will have a variety of delays.
  
![image](https://user-images.githubusercontent.com/118953917/214762059-d8079e4f-ab52-48e5-9250-98041e883c2d.png)
  
* The delay table is characterized based on varying the input transition and output load of a cell, against the delay of that cell. 
* Each cell will have its own delay table for different sizes and threshold tables.

![image](https://user-images.githubusercontent.com/118953917/214762237-10ec6a51-a825-4ebf-baf4-52bb7ff85339.png)

</details>

<details>
  <summary>Theory 2:  Delay table usage</summary>
 
### Delay table usage
  
* Each type of cell will be having its own individual delay table, as the internal pmos and nmos width/length ratio gets varied, the resistance changes, then RC constant gets varied as well, meaning the delay of each cell gets varied. 
* The values of delay which are not available in the table are extrapolated based on the given data.
  
![image](https://user-images.githubusercontent.com/118953917/214764494-ef258ff7-6660-4281-85ff-5f8a2652140d.png)

* Similarly, the ways on how we have a delay table, we will also have a characterization table for input transition.
* The latency at the endpoints will be the sum of the delays of each individual cell in that path. 
* The total skew value between two endpoints will be non-zero if the output load driven for a cell is varied, meaning different delay numbers are seen between endpoints, this is why it is preferred to have the nodes at each level driving the same load. 
* Another case in which we can retain the skew to be zero in the presence of varied load, is by using a different buffer size at the same level that can achieve the same level of delay as the other buffer in same level based on its delay table.
* These are factors which should be looked into in the early stages of the clock tree design stage. 
* Now we must look into power aware CTS, where we have to consider endpoints which are only active under certain conditions. 
* In this case, we do not need to propagate the clock into those cells during the period of inactivity.

![image](https://user-images.githubusercontent.com/118953917/214765256-2979c5c0-b2a9-41ab-b311-9e3cfb33a076.png)

</details>

<details>
  <summary>Lab 4: Lab steps to configure synthesis settings to fix slack and include vsdinv</summary>
 
### Lab steps to configure synthesis settings to fix slack and include vsdinv
  
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/configuration
vim README.md
```
  
* SYNTH_STRATEGY: control the area and timing
* SYNTH_BUFFERING: control if we want to buffer high fanout net 
* SYNTH_SIZING: control in cell sizing instead of buffering
* SYNTH_DRIVING_CELL: ensure more drive strength cell to drive input
  
![image](https://user-images.githubusercontent.com/118953917/214776677-df33d5a9-66c2-4778-ab7e-b3ea83a6fad6.png)

> In openlane terminal
```
echo $::env(SYNTH_STRATEGY)
set ::env(SYNTH_STRATEGY) "DELAY 0"
echo $::env(SYNTH_STRATEGY)
echo $::env(SYNTH_BUFFERING)
echo $::env(SYNTH_SIZING)
set ::env(SYNTH_SIZING) 1
echo $::env(SYNTH_SIZING)
echo $::env(SYNTH_DRIVING_CELL)
```
  
*Note: refer https://github.com/AngeloJacobo/OpenLANE-Sky130-Physical-Design-Workshop#lab-part-3-day-4---fix-negative-slack for the details*
  
* With SYNTH_STRATEGY of Delay 0, the tool will focus more on optimizing/minimizing the delay, index can be 0 to 3 where 3 is the most optimized for timing (sacrificing more area). 
* SYNTH_BUFFERING of 1 ensures cell buffer will be used on high fanout cells to reduce delay due to high capacitance load. 
* SYNTH_SIZING of 1 will enable cell sizing where cell will be upsize or downsized as needed to meet timing. 
* SYNTH_DRIVING_CELL is the cell used to drive the input ports and is vital for cells with a lot of fan-outs since it needs higher drive strength (larger driving cell needed).
  
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-01_14-09/results/synthesis
rm -rf picorv32a.synthesis.v
```
 
> In openlane terminal
```
run_synthesis
```
  
![image](https://user-images.githubusercontent.com/118953917/214780525-3aa279d5-169d-444a-aefa-c5ff52cad77f.png)

> In openlane 
```
run_floorplan
```

![image](https://user-images.githubusercontent.com/118953917/214782752-dc620279-42e7-4d0c-ae61-7c9572e0e6c0.png)
  
* The solution for this error is found. 
* basic_macro_placement command is failing since EXTRA_LEFS variable inside config.tcl is assumed as a macro which is not. 
* The temporary solution is to comment call on basic_macro_placement inside the OpenLane/scripts/tcl_commands/floorplan.tcl (this is okay since we are not adding any macro to the design).

* After that run_placement, another error will occur relating to remove_buffers, the solution is to comment the call to remove_buffers_from_nets in OpenLane/scripts/tcl_commands/placement.tcl. 
* After successfully running placement, runs/[date]/results/placement/picorv32.def will be created.
  
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/scripts/openroad
vim or_basic_mp.tcl
cd ~/Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands
vim floorplan.tcl
```
  
**Modification in gvim**
  
![image](https://user-images.githubusercontent.com/118953917/214784124-bd01cb53-8308-4b02-8c5e-469da27d04b5.png)

> In openlane
```
run_floorplan
run_placement
```
  
> In terminal
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-01_14-09/results/placement
magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def
```

![image](https://user-images.githubusercontent.com/118953917/214790612-456f38a4-6cca-4353-a68a-c5135632a6a9.png)

</details>

### Timing analysis with ideal clocks using openSTA
<details>
  <summary>Theory 1: Setup timing analysis and introduction to flip-flop setup time</summary>
 
### Setup timing analysis and introduction to flip-flop setup time
  
* At the zero time stamp, there is one clock edge that reaches the launch flop. 
* At T time stamp, the second rising edge reaches the capture flop. Any analysis that needs to be done is between 0 and T. For the combinational circuit to work, the combinational delay needs to be less than the period, T.

![image](https://user-images.githubusercontent.com/118953917/214792086-fb20df82-8122-48da-8694-a780b6d8d977.png)

* Looking at more practical scenarios and how the flop works, there will be a delay within the internal flop circuitry, between mux 1 and mux 2. 
* These internal delays will restrict the combinational delay requirements.
* This internal delay is known as the setup time, and this setup time needs to be subtracted away from the complete clock period T. 
* Now the capture flop has enough time for it to compute the data within the flop and ensure the data is ready at Q by the time the second rise edge of clock reach.
  
![image](https://user-images.githubusercontent.com/118953917/214792585-bb290730-1517-41ed-98ff-46ef6c88df66.png)

</details>

<details>
  <summary>Theory 2: Introduction to clock jitter and uncertainty</summary>
 
### Introduction to clock jitter and uncertainty
  
**Jitter**
  
* Deviation of a clock edge from its ideal location
* Typically caused by clock generator circuitry, noise, power supply variations, interference from nearby circuitry etc. Jitter is a contributing factor to the design margin specified for timing closure
  
* The next practical scenario to take into consideration is jitter. 
* The clock is expected to reach the clock pin at exactly 0s or at Ts, but in real scenarios, the clock signal may not be able to reach at the exact moment, as the clock source generation may have its own built-in variation. 
* This is known as jitter, the temporary variation of the clock period. 
* The combinational delay will become more stringent as a result. Thus we change our combinational delay to factor in the uncertainty factor from the jitter.
  
![image](https://user-images.githubusercontent.com/118953917/214793401-a1382eea-22d8-4374-ae33-0bd2d36d76c2.png)

* The combinational delay of a path will look as shown above.
* The next challenge comes in performing timing analysis with multiple ideal clocks.
  
![image](https://user-images.githubusercontent.com/118953917/214793711-7f08ec10-1a94-4711-9cf2-f76aae4c3c01.png)

</details>

<details>
  <summary>Lab 1: Lab steps to configure OpenSTA for post-synth timing analysis</summary>
 
### Lab steps to configure OpenSTA for post-synth timing analysis
  
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane
vim pre_sta.conf                                          (For pre-layout timing analysis)
```
  
![image](https://user-images.githubusercontent.com/118953917/214798003-4d475ac5-16ca-4bfc-8f62-73e4642a4a27.png)

```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src
vim my_base.sdc
cd ~/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/libs
vim sky130_fd_sc_hd__typical.lib
```
  
![image](https://user-images.githubusercontent.com/118953917/214800764-3a700389-9330-4ac7-b43e-6dab567eaa91.png)
  
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane
sta pre_sta.conf
```

![image](https://user-images.githubusercontent.com/118953917/214802161-4bcb8008-5413-47af-b401-ec2ac29a9b8e.png)

</details>

<details>
  <summary>Lab 2: Lab steps to optimize synthesis to reduce setup violations</summary>
 
### Lab steps to optimize synthesis to reduce setup violations
  
![image](https://user-images.githubusercontent.com/118953917/214803801-892a4879-34ea-4753-ab7e-c76dc102c622.png)

```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/configuration
vim README.md
```
  
![image](https://user-images.githubusercontent.com/118953917/214804887-e1a8a2e7-fd97-4bdf-acd2-dd09fbd2f31b.png)
  
> In openlane
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane
echo $::env(SYNTH_MAX_FANOUT)
set ::env(SYNTH_MAX_FANOUT) 4
```
  
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-01_14-09/results/synthesis
rm -rf picorv32a.synthesis.v
```

> In openlane
```
run_synthesis
```
  
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane
sta pre_sta.conf
report_net -connections _18242_                           
replace_cell _41952_ sky130_fd_sc_hd__dfxtp_4             (Pick the highest fanout, cap, slew and replace the worst violations of the cell by increasing drive strength --> upsize cell from 2 to 4)
report_checks -fields {net cap dlew input pins} -digits 4
report_tns
report_wns
```

![image](https://user-images.githubusercontent.com/118953917/214859399-c9b6b58f-0584-4c04-a174-ae59bcab8cbc.png)
  
![image](https://user-images.githubusercontent.com/118953917/214857863-aac7b3eb-65c4-4582-8cd7-65b96a2b1c46.png)

</details>

<details>
  <summary>Lab 3: Lab steps to do basic timing ECO</summary>
 
### Lab steps to do basic timing ECO
  
```
report_checks -from _41952_ -through _41879_
```

![image](https://user-images.githubusercontent.com/118953917/214862134-a1d92120-83bf-4517-8953-c1fd454e2c3f.png)

</details>

### Clock tree synthesis TritonCTS and signal integrity
<details>
  <summary>Theory 1:  Clock tree routing and buffering using H-Tree algorithm</summary>
 
### Clock tree routing and buffering using H-Tree algorithm
  

  
  
  
  
</details>

<details>
  <summary>Theory 2:  Crosstalk and clock net shielding</summary>
 
### Crosstalk and clock net shielding
  

  
  
  
  
</details>

<details>
  <summary>Lab 1: Lab steps to run CTS using TritonCTS</summary>
 
### Lab steps to run CTS using TritonCTS
  
> In sta terminal
```
write_verilog ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-01_14-09/results/synthesis/picorv32a.synthesis.v
```
  
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-01_14-09/results/synthesis
ls -lrt picorv32a.synthesis.v
date
```
  
![image](https://user-images.githubusercontent.com/118953917/214867050-6e2e1327-2021-4170-a662-2d7de1940ef4.png)

> In openlane
```
run_floorplan
run_placement
run_cts
```
  
  
  
  
  
  
  
</details>

<details>
  <summary>Lab 2: Lab steps to verify CTS runs</summary>
 
### Lab steps to verify CTS runs
  
  
  
  
  
</details>
