## Day-24

### Topic: Timing violations and ECO

<details>
  <summary>Theory</summary>
  
### Introduction to Engineering Change Order (ECO)
  
* Basically, ECO is the way on how we incorporate last minute changes in our design
  
* Typically, ECO has been done on the gate level netlist, but designer needs to edit the gate-level netlist and perform the same changes in RTL
  
* Then, all the verifications must be passed before it is being passed onto the layout and ensure that the ECO is passing a formal and functional verification before we start editing the layout
  
* In this stage, all the violations are fixed and all the sign-off checks that were not done during the PD flow are sealed
  
* Steps to perform ECO
  1. Investigate the problem using the recent database
  2. ECO generation to address the problem
  3. ECO implementation with the recent database
  4. After implementing and fixing the problem, save it in the database for future
  
* ECO strategies
  
![image](https://user-images.githubusercontent.com/118953917/218370188-c5e95462-e61e-4cb5-8637-352703b55461.png)
  
</details>

<details>
  <summary>Lab</summary>
 
### Timing violations and ECO
  
**Reviewing full report timing**
  
```
gvim /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/shell/rpts_icc2/timing_estimation/vsdbabysoc.post_estimated_timing.rpt
```
  
![image](https://user-images.githubusercontent.com/118953917/218626734-e74b6980-5011-43d0-8171-cf3f1c425403.png)
  
```
report_timing -path_type full_clock_expanded -capacitance -nets -physical
```
  
* Need to upsize the cell to improve the timing
* Upsizing the cell will increase the drive strength of the cell which will help in reducing the delay
  
![image](https://user-images.githubusercontent.com/118953917/218628329-99cb8f53-fec0-498b-a9a3-4dcf4ef9dd64.png)

```
size_cell core/U6 sky130_fd_sc_hd__nand2_4
size_cell core/U561 sky130_fd_sc_hd__a21oi_4
size_cell core/U67 sky130_fd_sc_hd__nand2_8


```
  
  
  
</details>
