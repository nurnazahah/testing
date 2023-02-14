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
  
* Estimated report timing before CTS
* Clock network delay is ideal, hence there is no delay reported and the slack is good
  
![image](https://user-images.githubusercontent.com/118953917/218665586-61ee4256-e9dd-4eb5-9fda-598218330217.png)
  
```
report_timing -from core/CPU_is_add_a3_reg -to core/CPU_Xreg_value_a4_reg[24][31] -corner estimated_corner -mode [all_modes]
```
  
* Estimated report timing after CTS
* Clock network delay is propagated, hence there is some delay which makes the slack is slightly higher than pre-CTS
  
![image](https://user-images.githubusercontent.com/118953917/218665665-cf31a551-ce38-4357-bc30-5083d303ab0a.png)
  
```
report_timing -from core/CPU_is_add_a3_reg -to core/CPU_Xreg_value_a4_reg[24][31] -path_type full_clock_expanded -capacitance -nets -physical
```
  
* Need to upsize the cell to improve the timing
* Upsizing the cell will increase the drive strength of the cell which will help in reducing the delay
* Upgrading the drive strength depending on the analysis stated in the report timing
  
![image](https://user-images.githubusercontent.com/118953917/218665739-6fce88e8-9f72-45c1-9038-0da25907596b.png)
  
* Picking some cells to upsize in order to reduce the delay arrival time
  
> Upsizing the cell
```
size_cell core/U6 sky130_fd_sc_hd__nand2_4
size_cell core/U561 sky130_fd_sc_hd__a21oi_4
size_cell core/U67 sky130_fd_sc_hd__nand2_8
```
  
```
report_timing -from core/CPU_is_add_a3_reg -to core/CPU_Xreg_value_a4_reg[24][31] -path_type full_clock_expanded -capacitance -nets -physical
```
  
* The slack is still violated, however, the slack has improved after upsizing the cell
  
![image](https://user-images.githubusercontent.com/118953917/218665848-c55afe4c-2aa5-4f86-a1c1-64a8cae39968.png)
  
**Comparison reports before and after ECO**
  
* Timing perspective --> the timing has improved
  
```
report_qor
```
  
![image](https://user-images.githubusercontent.com/118953917/218699141-b4f53577-adfb-403b-ae38-447ec1b11b44.png)
  
* Area perspective --> the area has increased since the the cell has been upsized
  
![image](https://user-images.githubusercontent.com/118953917/218699223-9a3ead61-bd63-42f5-bf11-7677a35383a0.png)
  
* Power perspective --> theoretically, total power usage will increase since the area has increased due to upsizing cell. However, the power usage for both pre and post CTS are the same since the total area increased is not that much.
  
```
report_power
```

![image](https://user-images.githubusercontent.com/118953917/218699307-226d4417-e434-4676-89c0-29119e26f237.png)
  
### Adding in decaps
  
```
gvim /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/top.tcl       (Insert decaps command in top.tcl)
source /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/top.tcl
gvim /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/shell/icc2_output.txt
```
  
![image](https://user-images.githubusercontent.com/118953917/218705451-28cb9312-ad5d-4651-bc3d-58d95218e9bd.png)
  
```
report_power
```
  
![image](https://user-images.githubusercontent.com/118953917/218706906-cc9a14df-48aa-453b-a562-0219043d33b7.png)  
  
</details>
