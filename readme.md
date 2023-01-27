## Day-18

### Topic: Pre-layout timing analysis and importance of good clock tree

### Timing analysis with real clocks using openSTA
<details>
  <summary>Theory 1: Setup timing analysis using real clocks</summary>
 
### Setup timing analysis using real clocks
  
* After buffer has been added in, more clock network delay has been introduced and it will combining all the delays.
* With real clocks, we will need to have buffers inserted into the clock path to ensure the clock signal integrity. 
* Because of the buffer introduction, the clock edge will reach the clock pin with consideration to the delays of the buffers inserted. 
* The clock network delay will also need to take into consideration the delays from the buffers inserted. 
* The window will become shifted as a result of the delays from the buffers inserted. 
* The skew for this design will now be the difference between the deltas, and the equation for setup timing analysis will also changed based on the image shown. 
* If the data arrival time is higher than the data required time, then we will have negative slack on the path, meaning we have violations.
  
![image](https://user-images.githubusercontent.com/118953917/214993881-dec5a151-4e8b-4212-b8ba-64f1a8eca181.png)

* For hold timing analysis, where the capture edge is on the o clock rise edge, the combinational delay should be greater than the hold time of the flop.
* Hold time refers to the second mux delay, which is the time required for the data to be sent after the clock edge within the flop. 
* So the data needs to be arrived after the hold time, so the new data can be captured into the flop, after existing data is launched out.
  
![image](https://user-images.githubusercontent.com/118953917/214994634-1d64f057-197d-45f4-b7b0-65494f8cb69d.png)

</details>

<details>
  <summary>Theory 2: Hold timing analysis using real clocks</summary>
 
### Hold timing analysis using real clocks
  
* Introducing more real factors into our design for hold analysis will yield the below equation for hold timing. 
* Jitter for the launch clock and capture flop will not need to be taken into consideration as the design is on the 0 clock edge, and the arrival difference for the capture and launch flop will be the same.
* So, the uncertainty should be kept low for the hold analysis. 
* The slack formula will be --> data arrival time – data required time
* If data required time is higher, we will have negative slack, meaning the timing path for hold will be violated.
  
![image](https://user-images.githubusercontent.com/118953917/214995292-07ac14b8-171d-4058-9440-ff23b2804206.png)

* For the timing path setup for real clocks, we need to take into considerations the deltas that were mentioned earlier.
* For delta1, will be launch clock network delay, while delta2, will be capture clock network delay. 
* The equations for setup time and hold time are shown below.
  
![image](https://user-images.githubusercontent.com/118953917/214995599-ba697707-5ed4-463a-8ba0-0deecc7594b3.png)

</details>

<details>
  <summary>Lab 1: Lab steps to analyze timing with real clocks using OpenSTA</summary>
 
### Lab steps to analyze timing with real clocks using OpenSTA
  
*Note: Refer https://github.com/AngeloJacobo/OpenLANE-Sky130-Physical-Design-Workshop#timing-analysis-with-real-clocks*
  
> In openlane
```
openroad                                                                                                       (Invoking openroad)
read_lef /openLANE_flow/designs/picorv32a/runs/13-01_14-09/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/13-01_14-09/results/cts/picorv32a.cts.def
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/13-01_14-09/results/synthesis/picorv32a.synthesis_cts.v
read_liberty -max $::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib
read_liberty -min $::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib
set_propagated_clock [all_clocks]
read_sdc designs/picorv32a/src/my_base.sdc
report_checks -path_delay min_max -format full_clock_expanded -digits 4
```

* This step is not practical, therefore it violated.
  
![image](https://user-images.githubusercontent.com/118953917/215005919-ddd073ff-17b8-4fc5-80d8-a7d2f676c794.png)

![image](https://user-images.githubusercontent.com/118953917/215007150-7985c511-e27e-4bbd-bd35-1739d6667df2.png)

</details>

<details>
  <summary>Lab 2: Lab steps to execute OpenSTA with right timing libraries and CTS assignment</summary>
 
### Lab steps to execute OpenSTA with right timing libraries and CTS assignment
  
> Continuing from previous lab
```
exit        (Exit openroad)
openroad
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/13-01_14-09/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew trans net cap input pin} -format full_clock_expanded
echo $::env(CTS_CLK_BUFFER_LIST)                              (To see the list of buffers)
```
  
* Both timing are already met after post CTS
* The tool picked small cell first to meet the skew and area
  + skew values are within 10% of the max clock period
  
![image](https://user-images.githubusercontent.com/118953917/215010210-b7f01f8d-f0ff-4bc6-ae4d-9bdbcdc3df86.png)

![image](https://user-images.githubusercontent.com/118953917/215010920-912b1639-6412-4db4-a7dc-8a75afbb3d9f.png)

![image](https://user-images.githubusercontent.com/118953917/215011382-00bfd9c5-05cf-4a1d-b14d-c49d2b726ce8.png)

</details>

<details>
  <summary>Lab 3: Lab steps to observe impact of bigger CTS buffers on setup and hold timing</summary>
 
### Lab steps to observe impact of bigger CTS buffers on setup and hold timing
  
```
exit 
echo $::env(CTS_CLK_BUFFER_LIST)
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
echo $::env(CURRENT_DEF)
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/13-01_14-09/results/placement/picorv32a.placement.def
```
