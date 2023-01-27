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
  
* Error encountered during “gen_pdn” in which the stage cannot perform routing as current_def has not changed to floorplan.pdn
  
![image](https://user-images.githubusercontent.com/118953917/215052338-090fb2b0-91d4-4069-aec0-4c72264fe319.png)

</details>

<details>
  <summary>Lab 2: Lab steps from power straps to std cell power</summary>
 
### Lab steps from power straps to std cell power
  
* This is just a review on PDN
  
* If the power and ground rails has a pitch of 2.72, thus the customized inverter cell will have a height of 2.72 or else the power and ground rails will not be able to power up the cell. 
* As shown below, power and ground flows from power/ground pads --> power/ground ring --> power/ground straps --> power/ground rails.
  
*Source: https://github.com/AngeloJacobo/OpenLANE-Sky130-Physical-Design-Workshop#timing-analysis-with-real-clocks*
  
![image](https://user-images.githubusercontent.com/118953917/215058376-777e3768-0d83-4b71-9d07-1594bdda4e2b.png)

</details>

<details>
  <summary>Lab 3: Basics of global and detail routing and configure TritonRoute</summary>
 
### Basics of global and detail routing and configure TritonRoute
  
```
echo $::env(CURRENT_DEF)            (Ensure the def file of pdn has been created)
echo $::env(ROUTING_STRATEGY)
run_routing
```
  
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/configuration
vim README.md
```

* Assuming power distrivution network has been successfully generated
  
![image](https://user-images.githubusercontent.com/118953917/215117743-5048a812-29a0-42cb-9863-a8c497402b80.png)
  
</details> 

### TritonRoute Features
<details>
  <summary>Theory 1: TritonRoute feature 1 - Honors pre-processed route guides</summary>
 
### TritonRoute feature 1 - Honors pre-processed route guides
  
* OpenLane routing stage consists of two stages:
  + **Global Routing**: Form routing guides that can route all the nets. The tool used is FastRoute.
  + **Detailed Routing**: Uses the global routing's guide to connect the pins with the least amount of wire and bends. The tool used is TritonRoute.

**Triton Route**
  
* In fast routing, a rough routing draft is created. 
  
* Fast routing is the engine which is used for global routing. 
  
* During global routing, the region is divided into grid cells, which acts as a route guide for the TritonRoute to be used for the detail routing, where an algorithm is used to find the best connectivity between the points.

* It honours the preprocessed route guides (obtained after fast routes), wherein the tool attempts as much as possible to route within route guides. 
  
* The tool assumes route guides for each net satisfies inter-guide connectivity. 
  
* Triton route works on proposed MILP (Mixed integer liner programming) based panel routing scheme with intra-layer parallel and inter-layer sequential routing framework, to finds the best way to perform the routing.
  
* Intra-layer refers to the routing within a layer, while inter-layer routing refers to routing between layers, through the uses of vias.
  
![image](https://user-images.githubusercontent.com/118953917/215102497-8e9c6337-56b7-47b4-a098-8cdd9ba1fe75.png)

</details>

<details>
  <summary>Theory 2: TritonRoute Feature2 & 3 - Inter-guide connectivity and intra- & inter-layer routing</summary>
 
### TritonRoute Feature 2 & 3 - Inter-guide connectivity and intra- & inter-layer routing
  
* Preprocessed guides should have unit width and must be in the preferred direction of the layer. 
  
* Global route is done by fast route, and the output would be the routing guide. 
  
* The initial route guides are transformed into the preprocessed guides through splitting, merging, and bridging. 
  
* Whenever the tool detected the route guide with non-preferred direction, it divides the route guide into unit widths, and then the route with preferred direction is merged, while the non-preferred direction route is bridged to be a route on another layer, which is the preferred direction for routing. 
  
* This way, parallel routing with higher layers are avoided, as well as avoiding parallel plate capacitance.
  
![image](https://user-images.githubusercontent.com/118953917/215103703-a8f8b000-0aa2-4edc-b386-b5dd65ff421d.png)

* Each layer or panel will have its own preferred routing direction assigned to it, in which the routes should be formed. 
  
* Routing in higher layers will begin only once the routing the bottom layers have been completed.

* Two guides are connected if they are on the same metal layer with touching edges, or if they are on neighbouring metal layers with a non-zero vertically overlapped area.
  
![image](https://user-images.githubusercontent.com/118953917/215104070-3025a21a-61ca-441e-855a-0ebcda219df0.png)

* Each unconnected terminal i.e. pin of a standard cell instance, should have its pin shape overlapped by a route guide. 
  
* The purple box in the RHS figure below would be the routing guide on the metal 1 layer that would overlap with the unconnected terminal.

![image](https://user-images.githubusercontent.com/118953917/215105174-cb8ca1d4-96ae-4d19-8106-25ed5480d734.png)

</details>

<details>
  <summary>Theory 3: TritonRoute method to handle connectivity</summary>
 
### TritonRoute method to handle connectivity
  
* Inputs file needed for TritonRoute are the LEF file, DEF file, and the Preprocessed route guides.

* Outputs from the TritonRoute would be a detailed routing solution with optimized wire-length and via count.

* Constraints needed in TritonRoute are the route guide honouring, connectivity constraints and design rules.
  
**TritonRoute method**
  
* TritonRoute handles connectivity through 2 ways
  + Access Point (AP)
  + Access Point Cluster (APC)
  
* **Access point**: on-grid point on the metal layer of the route guide, and is used to connect to lower-layer segments, upper-layer segments, pins or IO ports. 
  
* **Access Point Cluster**: a union of all Access Points derived from the same lower-layer segment, upper-layer guide, a pin or an IO port. 
  
* Access point refers to the point where the via can be placed to allow connectivity between layers. 
  
* The objective of the Mixed Integer Liner Programming (MILP) is to connect one access point to another optimally. 
  1. Choose one of the access points where the via should be dropped
  2. Determining how the first access point will be connected to the next access point.

![image](https://user-images.githubusercontent.com/118953917/215107218-454cb310-8936-43c7-90ed-695569d3fef8.png)

</details>

<details>
  <summary>Lab 1: Routing topology algorithm and final files list post-route</summary>
 
### Routing topology algorithm and final files list post-route
  
**Optimization algorithm for routing topology**
  
* For each APC, the algorithm needs to find the cost associated with the distance between 2 APCs which are the minimum spanning tree, MST, between the APCs and the costs. 
  
* The objective of the algorithm
  1. To find the minimal and most optimal point between the 2 APCs.
  
![image](https://user-images.githubusercontent.com/118953917/215117524-620c7049-f13d-4a3c-80b0-3d650d7ca0b7.png)

* There are 3 violations as shown in the figure below

* TritonRoute strategy = 0 is chosen right now
  
* If TritonRoute strategy = 14 is chosen, the violations might be 0 but it might take some time to finish running
  
* Therefore, we need to fix the violations manually
  
![image](https://user-images.githubusercontent.com/118953917/215118656-b1f2df2a-c34c-4b9d-aa57-1cb22a4f9b9a.png)
  
* Referring to the below figure, there are 3 violations on li1 layer that needs to fix manually

![image](https://user-images.githubusercontent.com/118953917/215119749-96387581-8583-496b-9330-fafa6972ac76.png)

* Creating a new spef file that requires merged.lef and picorv32a.def

![image](https://user-images.githubusercontent.com/118953917/215120595-1fd696bd-0a6f-4904-a31b-66c300db50ed.png)

</details>
