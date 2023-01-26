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
  
![image](https://user-images.githubusercontent.com/118953917/214750890-3dd7cd6a-5968-4308-8540-b2314e70d628.png)

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
  <summary>Lab 4:  Introduction to delay tables</summary>
 
### Introduction to delay tables

