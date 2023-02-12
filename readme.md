## Day-21

### Topic: Placement and CTS labs

<details>
  <summary>Theory</summary>
 
### Placement
  
* Pre-placement sanity check: floating pins in netlist, unconstrained pins, timing, pin direction mismatch, and etc.
  
**What is placement?**
  
* Standard cell
* Placement stages including:
  + Global placement
  + Legalization
  + Detailed placement
  
**Placement objectives**
  
* Congestion
* Performance
* Timing
* Routability
* Runtime
  
### Clock Tree Synthesis (CTS)
  
**Inputs of CTS**
  
* Placement DB
* CTS Spec file
  
**CTS Steps**
  
1. Clustering
2. DRV Fixing
3. Insertion Delay Reduction
4. Power reduction
5. Balancing
6. Post-conditioning
  
**CTS Quality Checks**
  
* Skew
* Pulse width
* Duty cycle
* Latency
* Clock tree power
* Signal integrity and Crosstalk 
* Timing analysis and fixing
  
*Source: notes were taken from lecture slides*
  
</details>

<details>
  <summary>Lab</summary>
 
### Placement, CTS and Routing
  
**Using core utilization = 40% in top.tcl**
  
**Script in top.tcl**
  
* ```create_placement``` is used to create placement for the design. ```floorplan``` option is selected to make the design planning styled as placement. 
* Pin Placement is done by sourcing pns.tcl to sync with the current technology file regarding power grid creation.
  
![image](https://user-images.githubusercontent.com/118953917/218297745-96602f27-19c0-44a6-979e-e81bf0bc81cf.png)
![image](https://user-images.githubusercontent.com/118953917/218297768-d0730840-039b-4ccc-a2d3-a2002d5d9141.png)

**Reports that were generated from the run**
  
```
gvim /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/shell/rpts_icc2/place_pins/check_design.pre_pin_placement
```
  
* There are 3 warnings in total for pre-placement while checking the design.
  
![image](https://user-images.githubusercontent.com/118953917/218298713-739fb52c-e1e5-4e39-8de7-17e8789cd72c.png)
  
```
gvim /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/shell/rpts_icc2/place_pins/report_port_placement.rpt
```
  
![image](https://user-images.githubusercontent.com/118953917/218299564-5e967467-86e8-422a-aaff-f257de6513d4.png)
  
```
gvim /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/shell/icc2_output.txt
```
![image](https://user-images.githubusercontent.com/118953917/218299493-57808111-de38-4335-b5f0-9b318f396143.png)
![image](https://user-images.githubusercontent.com/118953917/218298961-aefdde53-150b-4f37-83de-ce857be8f10e.png)
  
```
gvim /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/shell/rpts_icc2/timing_estimation/vsdbabysoc.post_estimated_timing.rpt
```
  
* Post estimated timing report for the design shows the slack has met with the value of 0.67

![image](https://user-images.githubusercontent.com/118953917/218299368-03ff1290-7716-45ee-a329-ca1d90f110d2.png)

```
gvim /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/shell/rpts_icc2/timing_estimation/vsdbabysoc.post_estimated_timing.qor
```
  
* Post estimated timing qor shows there is no violating path reported with 92 nets having violations, 79 max trans violations, and 83 max cap violations.
  
![image](https://user-images.githubusercontent.com/118953917/218299765-d3206736-8890-4cea-8e50-5a602b27254d.png)
  
```
gvim /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/shell/rpts_icc2/timing_estimation/vsdbabysoc.post_estimated_timing.qor.sum
```
  
* Summary of Post estimated timing qor
  
![image](https://user-images.githubusercontent.com/118953917/218299961-09f62744-36a4-4d49-8749-616a64b94490.png)

**CTS schematic design**
  
*Note: before CTS, the clock would be ideal, while after CTS, the clock will be propagated*
  
![image](https://user-images.githubusercontent.com/118953917/218315845-f5377f9c-c93c-4be1-8fa1-38723bc24b0c.png)

![image](https://user-images.githubusercontent.com/118953917/218317392-6ae95cc9-a0bd-4dc2-83f8-779f972ba514.png)

  
</details>


## Day-22

### Topic: CTS analysis labs

<details>
  <summary>Theory</summary>
 
### CTS
  
**What is CTS?**
  
* Clock Tree Synthesis
* A technique for distributing the clock equally among all sequential parts of VLSI design
* It will balancing the delays to all clock input pins when the clock is distributed equally
* The goal of CTS is to minimize skew and insertion delay
  
![image](https://user-images.githubusercontent.com/118953917/217413608-7e43ff8e-67b5-4c46-acea-04d2bf6898ec.png)

**Various algo’s used for CTS**
  
![image](https://user-images.githubusercontent.com/118953917/217414647-8c241be6-200a-4a4a-b854-337c95911ede.png)

**H-tree algorithm**
  
1. Find out all the flops present
2. Find out the center of all the flops
3. Trace clock port ot the center point
4. Divide the core into two parts, trace both the parts and reach to each center
5. From the center, again, divide the area into two and again trace till center at both the end
6. Repeat this algo till the time we reach the flop clock pin
  
![image](https://user-images.githubusercontent.com/118953917/217415097-2f1675c7-533e-4d1b-858f-524f7a2cbd77.png)

**Various CTS checks**
  
* Skew check
* Pulse width check
* Duty cycle check
* Latency check
* Power check
* Crosstalk Quality check
* Delta Delay Quality check
* Glitch Quality check
  
**Some useful commands**
  
* This command checks and reports issues that can lead to bad QoR
  + Clock tree structure
  + Constraints
  + Clock tree exceptions
  
```
check_clock_tree
```
  
* This command checks the design whether the placement done input is perfect or not
  
* If it is legal, it is well and good
  
```
check_legality
```
  
* Else, use below command
  
```
legalize_placement
```
  
* There are some default constraints that CTS used, refer table below
  
![image](https://user-images.githubusercontent.com/118953917/217416747-e4f7ede4-49ed-4f99-8cc3-d4016d7a99e4.png)
  
* To edit those default constraints, use command below

```
set_clock_tree_options
```
  
* Below are some examples (be careful keeping min and max values in sight)
  
```
-max_capacitance
-max_transition 
```

![image](https://user-images.githubusercontent.com/118953917/217417327-52c7cfa3-2d30-44b0-a6f2-3f60eccf0c50.png)

* IC Compiler uses the CTS design rule constraints for all optimization phases, as well as for CTS
  
* For information about setting the clock tree synthesis design rule constraint, below are the commands
  
![image](https://user-images.githubusercontent.com/118953917/217417626-e74f29f4-7353-42a8-9776-1562647ed9dd.png)

* We can use ICC2 with debug mode by using below command
  
*Note: Please refer the ICC2 manual from synopsys*
  
```
-set cts_use_debug_mode true
```
  
* The main command we need to do is as below command
  
```
compile_clock_tree
```
  
**CTS results analysis**
  
* We can use the report for the tree that has been built using below command
  
```
report_clock_tree
-summary 
-settings 
```

* Other reports to see
  + Reports Max global skew
  + Late/Early insertion delay
  + Number of levels in clock tree
  + Number of clock tree references (Buffers)
  + Clock DRC violations 
  
* Also, another report related to clock timing report for paths that are related
  + Reports actual
  + Relevant skew
  + Latency
  + Interclock latency 
  
```
report_clock_timing
–type skew
```
  
* After observing the reports, if we see the clock tree could be better, perform CTS and incremental physical optimization as command below
  
* This process results in a timing optimized design with fully implemented clock trees
  
* The command below does the following:
  + Performs clock tree power optimization
  + Synthesizes(Re-Synthesizes) the clock trees
  + Optimizes the clock trees
  + Adjusts the I/O timing 
  + Performs RC extraction of the clock nets and computes accurate clock arrival times
  + Performs placement and timing optimization
  
```
clock_opt
```
  
* Sometimes, there will be some unrouted clock trees
  
* To remove them, perform the command below
  
```
remove_clock_tree
```
  
**After CTS, we do synthesis**
  
* Before we synthesize the clock trees, use below command to verify that the clock trees are properly defined
  
```
check_clock_tree 
icc2_shell> check_clock_tree -clocks my_clk
```
  
**Another useful commands**
  
![image](https://user-images.githubusercontent.com/118953917/217419707-46912d99-d641-4234-b994-490871c9442c.png)
  
*Source: notes were taken from lecture slides*

</details>

<details>
  <summary>Lab</summary>
 
### CTS Lab analysis
  
  
</details>
