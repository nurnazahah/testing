## Day-19

### Topic: Final steps for RTL2GDS

### Routing and design rule check (DRC)
<details>
  <summary>Theory 1: Introduction to Maze Routing using Lee's algorithm</summary>
 
### Introduction to Maze Routing using Lee's algorithm
  
**Routing stage**
  
* **Maze Routing-Lee’s Algorithm [Lee 1961]**: Algorithm to find the shortest path between two nodes in a grid

* Routing: the process of creating the physical wire connections within the design in which it helps in determining the best way of routing that can be done between two endpoints, the source, and the target, with the shortest distance as well as the least number of zig-zag turns.

* However, the algorithm needs to be aware of any blockages set that hinders any routing to be done in the particular area.
  
![image](https://user-images.githubusercontent.com/118953917/215030291-ceea2ccd-d922-4883-8370-97a39a423721.png)

**Steps in Lee's Algorithm**
  
1. Create the routing grid behind the floorplan
  
2. The two points were created which are the source and target
  * Algorithm will look for the best route to connect the two points with the help of the routing grid
  
3. The tool will label the grid ground (adjacent of horizontal and vertical grid)
  * However, it did not label the grid under the blockage and at the boundary
  
![image](https://user-images.githubusercontent.com/118953917/215032150-34401e02-d2db-4d5c-a437-3ca369b4cdec.png)

</details>

<details>
  <summary>Theory 2: Lee's Algorithm conclusion</summary>
 
### Lee's Algorithm conclusion
  
* It is preferable to choose the LHS instead of RHS of the figure below
* It is because the chosen route in the LHS figure got the best route which is less bending (L-shaped) rather than the RHS figure
* Therefore, LHS figure will proceed to do the global routing

* Performing the routing for 1 route will be fairly simple, but when we have millions of start and endpoints to route between, this method will consume a lot of time and memory
* Maze routing consumes more time and memory if the design is huge
* There are some algorithms that can help to reduce the time and memory consumption such as line-search algorithm, stanner-tree algorithm
  
![image](https://user-images.githubusercontent.com/118953917/215033857-8446f957-7d5d-4378-accf-81f824cfbdd9.png)

</details>

<details>
  <summary>Theory 3: Design Rule Check</summary>
 
### Design Rule Check
  
* Design Rule Check (DRC): the rules that should be followed whenever the routing of the design is performed.
  
* One of the rules may be the minimal wire width, where the width of the wire should be no less than a specified amount based on the limitations of the fabrication process. 
  
* Another rule that is based on the fabrication process of lithography is the wire pitch, where the centre-to-centre distance between 2 wires should be no smaller than a certain distance. 
  
* Another rule includes wire spacing rule, where distance between 2 wires should be no smaller than a certain distance. 
  
* These are many rules that the tool needs to take into account when performing the routing of the design.
  
![image](https://user-images.githubusercontent.com/118953917/215035532-aaae220b-4b3d-446c-8eed-5b697367212c.png)

* One type of DRC violation is a signal short, where two wires that are not intended to be connected becomes in contact on the same layer.
  
* This could lead to functional failure, so this needs to be taken care of.
  
* To fix this, we need to simply moving one of the wires onto a different metal layer.
  
* However, please keep in mind that there are new drc rules that need to be taken into account.

![image](https://user-images.githubusercontent.com/118953917/215036320-f0517b39-378a-4a40-9f0f-eb735c2001b4.png)

**New drc rules after fixing**
  
* **Via width** where the width of the via should be no less than a certain value. 

* **Via spacing** where the distance between 2 vias cannot be less than a specific distance. 
  
* Most of these DRC rules come from the lithographic process and the limitations that come with the technology.
  
![image](https://user-images.githubusercontent.com/118953917/215036993-58407624-384b-4965-a9b1-0519dc5a5670.png)

* Performing parasitic extraction, where the resistances and capacitances of the wires are extracted and will be used for further processes.
  
![image](https://user-images.githubusercontent.com/118953917/215037259-747e6a52-c7cd-4a01-ab20-209179aef4fe.png)

</details>

### Power Distribution Network and routing
<details>
  <summary>Lab 1: Lab steps to build power distribution network</summary>
 
### Lab steps to build power distribution network
  
> If exited from openlane
```
cd work/tools/openlane_working_dir/openlane
make mount
pwd
ls -ltr
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a -tag 13-01_14-09
```
  
*Note: If you want to retain the configurations form the last openlane job, you need to use the command “prep -design -tag ”. If you want to create a fresh run with new configurations but without changing the tag name, you need to use the command “prep -design -tag -overwrite”.*


> In openlane
```
echo $::env(CURRENT_DEF)    (Ensure current_def is on the CTS stage)
gen_pdn                     (To generate power distribution network)
```
  
![image](https://user-images.githubusercontent.com/118953917/215051786-c484ea4f-1a9d-4fae-8aa2-b97c72929501.png)


