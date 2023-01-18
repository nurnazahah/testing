## Day-16

### Topic: Understand importance of good floorplan vs bad floor plan and introduction to library cells

### Chip Floor planning considerations
<details>
  <summary>Lecture 1: Utilization factor and aspect ratio</summary>
 
### Utilization factor and aspect ratio
  
**Defining width and height of the core and die**
  
1. Begin with a simple netlist as an example
* Consider a netlist with 2 flops and 2 gates with below connections 
*Note: a netlist describes the connectivity of an electronic design*

![image](https://user-images.githubusercontent.com/118953917/212586111-49b04c72-3ef0-4de2-92af-77182d19a591.png)

2. Convert the components/symbols into physical dimension
  
![image](https://user-images.githubusercontent.com/118953917/212587431-c7aa9bb0-1bfc-45f2-9a92-2c2533601a7a.png)
  
3. Find out the dimesion of the core and die
*Note: Only focus on the dimensions of the standard cells and flops, not the wires*
  
Let say, the dimension of standard cell is 1 unit by 1 unit. Hence, the area is 1 sq. unit as shown
  
![image](https://user-images.githubusercontent.com/118953917/212588313-edb52e12-b691-4c08-bb16-c5842866558e.png)

Assume the same for flip flop
  
![image](https://user-images.githubusercontent.com/118953917/212589262-5800a3b2-a07e-45f4-a6f3-cef4246d1822.png)

4. Calculate the area occupied by the above netlist on a Silicon wafer by:
  * Remove the wires and place all the physical dimensions into a single grid
  
![image](https://user-images.githubusercontent.com/118953917/212590234-3532ee54-547a-4407-909a-027e916d4f81.png)
  
**Recap: What is core and die section of a chip?**
  
* Core: the section of the chip where the fundamental logic of the design is placed
* Die: consists of core (encapsulate the core), it is a small semiconductor material specimen on which the fundamental circuit is fabricated
  
*Source: the illustration has been taken from instructor's video*
  
![image](https://user-images.githubusercontent.com/118953917/212591511-3af66b9e-e24a-4fed-9ef9-9192b0693289.png)
  
* How to arrive on its dimensions?
5. Place all the logic cells inside the core
  * The logical cells occupies the complete area of the core with 100% utilization
  * If there is some areas left out, it will not be 100% utilization, it may be some other number
  
  
![image](https://user-images.githubusercontent.com/118953917/212591281-a945f01a-0a28-42ef-92c6-68f47e442ef7.png)
  
* Since utilization factor = 1, equivalent to 100% utilization, and if there is some extra logic needs to be added, that is not allowed because there is no space left
* This 100% utilization is usually as an ideal scenario, but in practical scenario, we only utilized up until 50% to 60% utilization
* Whenever the aspect ratio = 1, it signifies that the chip is in square-shaped, else if aspect ratio is the other number except 1, it signifies that the chip is in rectangular-shaped

![image](https://user-images.githubusercontent.com/118953917/212791884-9a22ef2f-b4a5-4c4d-b1c3-3e88cc81c307.png)
  
* Let say the dimentions of the die and core is wider;
  
* As we can see from the example below, the core is 50% utilized by the logic and another remaining space/area can be used for any additional cells 
* Defining the utilization factor and aspect ratio are significant especially when we're doing floor planning
  
![image](https://user-images.githubusercontent.com/118953917/212794558-13c1047d-392d-4edd-a79e-36f832cbb1c2.png)
  
</details>

<details>
  <summary>Lecture 2: Concept of pre-placed cells</summary>
 
### Concept of pre-placed cells
  
**Continue on defining width and height of the core and die**
  
* Let say the core and die is bigger with the same height and width;
  
* Referring to the example below, the core has been utilized with 25% of the combinational logic and it has another 75% space/area which can be used for any additional cells
* Also, the free space can be used for more additional layers of routing
* Since aspect ratio = 1, it signifies the square-shaped
  
![image](https://user-images.githubusercontent.com/118953917/212796719-e8d0b937-3c9d-4a2e-a61e-3f7619ee81ed.png)
  
**Defining the location of the pre-placed cells**
  
![image](https://user-images.githubusercontent.com/118953917/212799249-0a383654-bdd4-4f3c-aa17-863867a8e727.png)

**What are the pre-placed cells?**
  
* Combinational logic basically does some functionalities i.e. memory, mux, any complex clock divider or etc.
* Combinational logic does a huge task and produced some huge output circuits
* Therefore, we need to extract it out and divide that huge output circuits into multiple blocks and then separate them out into different blocks 
* Referring to the separated blocks below, block 1 and block 2 will be implemented separately 
  
![image](https://user-images.githubusercontent.com/118953917/212802067-76e7d27c-f106-4305-84ab-f6afa1276c23.png)

* Block 1 contains the input of the pins while block 2 contains the output of the pins
* We then need to extend those IO pins of the two blocks and detached those blocks with the black boxes
* Those two blocks will then be invisible for the side that is looking from the top/main netlist
* Since those sections of the boxes are now invisible to the top netlist that we're implemented, we will now separate the black boxes as two different IP's or modules
* The advantages of separating those blocks are it is more convenient to use them for multiple times when needed with different users where they will be implemented separately based upon their functionalities. Also, those blocks can be reused separately for each block for multiple times when needed.
  
![image](https://user-images.githubusercontent.com/118953917/212806088-5256765d-6e0c-4b12-bce6-e37f80f5bd87.png)

* Similarly, there are any other IP's that are available, i.e.;
  + Memory
  + Clock-gating cell
  + Comparator
  + Mux
* All of them can be implemented once and can be instantiated multiple times onto a netlist
* However, this is no need to be implemented for multiple times because this is a part of top-level netlist where they perform some functions, where they receive some inputs signals, they deliver some output signals, but the functionality of the particular cells will be implemented only once
* This is what we called as pre-placed cells since the cells are being just placed once in a chip where we have to define the locations/arrangements of the particular cells and it has to be done before the routing
* The placement and arrangements of the cells on top-level chip will be fixed and they will not being moved even by automated tools
* Since they are being placed before the routing, they are called pre-placed cells 
  
* The arrangement of these IP’s in a chip is referred as Floorplanning
* These IP’s/blocks have user-defined locations, and hence are placed in chip  before automated placement-and-routing and are called as pre-placed cells.
* Automated placement and routing tools places the remaining logical cells in the design onto chip

</details>
  
<details>
  <summary>Lecture 3: De-coupling capacitors</summary>
 
### De-coupling capacitors
  
**Defining on pre-placed cells**
  
* For example, consider there are three pre-placed cell blocks including block A, block B, and block C where we implemented memories for once and they will be reused for multiple times in the circuit
* Most of the blocks are communicating with input pins. Hence, the blocks will be usually being placed near the input side depending on the scenario/design background
* Once the location has been set to place the blocks, the location can't be moved. Therefore, the location needs to be very well defined

![image](https://user-images.githubusercontent.com/118953917/212816258-75cdada2-e5d4-4f22-9b46-43e7e60aca6f.png)
  
**Surrounding the pre-placed cells with Decoupling Capacitors**
  
* Consider the amount of the switching current required for a complex circuit something like below
  
* Referring to the diagram below;
  
1.	Consider capacitance to be zero for the discussion. Rdd, Rss, Ldd and Lss are  well defined values.
2.	During switching  operation, the circuit demands switching current i.e. peak current (Ipeak).
3.	Now, due to the presence of Rdd and Ldd, there will be a voltage drop across them and the voltage at Node 'A' would be Vdd' instead of Vdd.
4.	If Vdd' goes below the noise margin, due to Rdd and Ldd, the logic '1' at the output of circuit won't be detected as logic '1' at the input of the circuit following this circuit.
  
*Source: https://www.vlsisystemdesign.com/*

![image](https://user-images.githubusercontent.com/118953917/212812045-c46db167-d66c-4a81-8d90-73e4f1246306.png)

**Noise margin summary**
  
* Anything that lies between Vol and Vll will be considered as logic 0 
* Any voltage that lies between Vll and Vih will be considered as undefined region 
* Undefined region -> the logic can either moved from logic 1 to logic 0 or from the interception point of (b) to logic 0. Undefined region is a danger case 
* Whenever the voltage lies between Vih and Voh, it will always being treated as 1V or logic 1
* Therefore, we have to ensure that the voltage didn't enter in undefined region since it cannot be identified whether the voltage might be in logic 1 or not
* That is the problem when we are having a large physical distance from the main power supply to the circuit
  
*Source: https://www.vlsisystemdesign.com/*
  
![image](https://user-images.githubusercontent.com/118953917/212813997-d1f1a5c0-dc8e-4356-b175-c692c4ec5c4c.png)

  
* To solve this issue, we have to add decopuling capacitors 
* Consider the decoupling capacitor is a huge capacitor which is completely be filled with the charge and the equivalent voltage across the decoupling capacitor is similar to what we have in the power supply
* Add the decoupling capacitor in parallel with the circuit
* Decoupling capacitor would decouples the circuit from the main supply 
* Everytime the circuit switches, it draws current from Cd. Whereas, the RL network is used to replenish the charge into Cd
  
*Source: https://www.vlsisystemdesign.com/*
  
![image](https://user-images.githubusercontent.com/118953917/212817283-9d6cda09-9700-4ed4-a70c-6b0527215c25.png)
  
* Overview of pre-placed cells on a top-level netlist across the decoupling capacitors and the blocks
  
![image](https://user-images.githubusercontent.com/118953917/212817436-6134ebd8-14bc-4e5f-970b-bcce194cc73a.png)

</details>

<details>
<summary>Lecture 4: Power planning</summary>

### Power planning
  
* From previous example, let's take the circuit and converted into a black box and denoted it as a macro/block
  
*Source: https://www.vlsisystemdesign.com/*
  
![image](https://user-images.githubusercontent.com/118953917/212826749-fba0ba9b-69fb-4669-a7d7-2bb77b0f8a49.png)

* Let say there are 4 different macros/blocks containing driver, load, and etc that are usually available in a complete chip with different functionalities
* The voltage source was supplied for 4 blocks. Hence, the supply from the source will be not stable where it is impossible to put all the decoupling capacitors.
* Let's assume the orange line was 16 bit bus
* Logic 1 indicates that the capacitor is being charged to Vdd while logic 0 will be discharged to the Ground
* This 16 bit bus is connected with the inverter, so the output will be inverted from the input. Logic 1 will be discharged and while logic 0 will be charged.
* All the capacitors that were charged to volts will be discharged to 0 volt through single Ground tap point. This will cause a bounce in Ground tap point.
* If this bounce exceeds the noise margin level, it will reaches undefined state, where during that stage, the voltage might be changed from logic 1 to logic 0 and it is unpredictable 
* All capacitors which were '0' volt will have to charge to 'V' volts through single Vdd tap point. This will cause lowering of voltage at Vdd tap point
* The level of Ground Bounce and Voltage Droop will be increased due to the multiple process of tap points happened at the same time. 
* If this voltage droop is within the noise margin level, it will be enough but if it exceeded undefined region in the noise margin level, we will in danger zone

*Source: https://www.vlsisystemdesign.com/*

![image](https://user-images.githubusercontent.com/118953917/212834088-97e65ccf-00db-4b79-8b59-68e105fb198e.png)


* To fix this issue, we need to use multiple power supply (i.e. multiple  Vdd and Vss) instead of using 1 power supply only
* Multiple power supply will be sourced to the nearest block and it will prevent the block from being missed to get the power supply
* Therefore, all the logics will take the nearest power supply
* The driver and the load also can be brought closed to each other in 'L' sense

*Source: https://www.vlsisystemdesign.com/*
  
![image](https://user-images.githubusercontent.com/118953917/212835786-e010e5f5-4576-407f-a43e-b08ed5e6aea3.png)
  
</details>

<details>
<summary>Lecture 5: Pin placement and logical cell placement blockage</summary>

### Pin placement and logical cell placement blockage

* Let's take several designs as examples that need to be implemented
* Along with the circuit, there are some pre-placed blocks as well that is being connected to the input circuit and being clocked out 
* There are 4 designs in total with different connections to be looking through in this section
* By merging all of the four designs, it will be a complete design 

![image](https://user-images.githubusercontent.com/118953917/212839899-2651497a-b8bf-4d8d-9f5d-c45188a2f13d.png)

**Pin placement of the complete design**
  
* For the design plan, the input port is set to be on the LHS while the output port is set to be on the RHS
* Input ports and output ports are placed randomly since it is depending on our the design planning
* The pins are placed depending on the blocks
* No cells/flip flops can be placed on the block a, block b and block c area
* Clock ports which are CLK1, CLK2 and Clkout are bigger in size as compared to data ports since the clock is the ports that are driving all the cells and sending the signals to all flip flops
* Bigger the size, least the resistance. Therefore, clock ports need to be bigger in size to avoid resistance during signal transmission since clock plays an important role in sending the signals
  
*Source: https://www.vlsisystemdesign.com/*
  
![image](https://user-images.githubusercontent.com/118953917/212844989-78948f55-f883-4eb1-bd09-7e6a4fc9cf35.png)

</details>

<details>
<summary>Lab 1: Steps to run floorplan using OpenLANE</summary>

### Steps to run floorplan using OpenLANE
  
```
cd ../Desktop/work/tools/openlane_working_dir/openlane/configuration
vim README.md
```
  
![image](https://user-images.githubusercontent.com/118953917/212850404-c5806032-162e-46fd-afbf-ac313433a657.png)

```
vim floorplan.tcl
```
  
![image](https://user-images.githubusercontent.com/118953917/212851351-44319a52-c656-4d87-931a-aa8079ccdf0a.png)

```
cd ../Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a
vim config.tcl
```
  
![image](https://user-images.githubusercontent.com/118953917/212937440-c3afd92a-87d8-4ffb-8144-d98212cb3f76.png)
  
> In OpenLANE terminal
```
run_floorplan
```
  
![image](https://user-images.githubusercontent.com/118953917/212854551-c8c84876-318a-496b-947f-6c28f2d0de0a.png)

</details>

<details>
<summary>Lab 2: Review floorplan files and steps to view floorplan</summary>

### Review floorplan files and steps to view floorplan
  
```
cd ../Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-01_14-09/logs/floorplan
vim 4-ioPlacer.log
```
  
![image](https://user-images.githubusercontent.com/118953917/212937620-16ef4520-7e0c-48b9-b428-ef5b3ddc7dff.png)
  
```
cd ../Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-01_14-09/
vim config.tcl
```

![image](https://user-images.githubusercontent.com/118953917/212937722-d220fefa-48ec-4f46-bc95-954d11ae697b.png)
  
```
cd ../Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-01_14-09/results/floorplan
vim picorv32a.floorplan.def
```

![image](https://user-images.githubusercontent.com/118953917/212937842-402075ea-8ab9-4028-b6dd-f76e3c6e4e80.png)

</details>

<details>
<summary>Lab 3: Review floorplan layout in Magic</summary>

### Review floorplan layout in Magic
  
```
magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
  
![image](https://user-images.githubusercontent.com/118953917/213065626-ef4a7930-e0ba-45e2-a747-005fcb1677f4.png)
  
![image](https://user-images.githubusercontent.com/118953917/213065684-9726f790-fd91-44d4-b1b3-96d2167c6e5c.png)
  
</details>

### Library Binding and Placement
<details>
  <summary>Lecture 1: Netlist binding and initial place design</summary>
 
### Netlist binding and initial place design

* Usually, the combinational logic gate will have its standard shape like AND gate, inverter and etc.
* But, in reality, the combinational logic gate will be converted to the box. The physical gate will be changed to the physical dimension.
* Therefore, all the gates will be removed and changed as in the figure
* Library: the place where we can find all the boxes and it has timing information of the delay of each gate
* Library also can be separated into sub library i.e. the first library consisting of the shape and size while the second library only consists of delay information.
* Library offers several options of library in same cell depending on its timing conditions and its available spaces in the floorplan. The bigger the cell size, the lesser the resistance 
* Hence, we have different flavours of library and we can pick up the desired library based upon the requirements
  
![image](https://user-images.githubusercontent.com/118953917/212942911-f3f8c138-8b65-4be0-917d-d591f6e443ca.png)

* The physical view of logic gates will be placed into the floorplan 
* We need to bind the netlist of the physical cells where the cells will have a real view of a box with a specific width and height
* These boxes will be available in the library, which will be having the width and height of the cells, as well as other details such as delay or required information, as well as the flavor of the cells.
* The arrangements in the floorplan is decided by taking into considerations of the connections of the physical cells i.e. where is the nearest locations between inputs and outputs and etc.
* During placement stage, we must ensure that the area used for the black boxes and decap cells do not have any cells inserted that can cause overlapping. The placement needs to be timing conscious as well to not make the routing long.
  
![image](https://user-images.githubusercontent.com/118953917/212945356-a4f1298a-b3d0-4b57-94b0-5bd0ef70acd1.png)

</details>

<details>
  <summary>Lecture 2:  Optimize placement using estimated wire-length and capacitance</summary>
 
###  Optimize placement using estimated wire-length and capacitance
  
**Optimizing placement**
  
* Optimizing is done by some estimations, let's take Din2 to FF1 in the figure as example. 
* As we can see, the route across Din2 to FF1 would make the data slew (long distance) where higher capacitance makes the amount needed to charge the capacitance becomes high, resulting in worst slew (transition).
* Before routing, we estimate the wire length and capacitance where based on that, the repeaters are inserted.
* In this case, the buffer is added (to lower the capacitance) and will act as repeaters that recondition the signals and making new signals or replicate the signal woth no distortion. However, more repeaters would results in loosing some data. 

![image](https://user-images.githubusercontent.com/118953917/213071175-5785104a-479a-4e24-955f-14a7c9a89c61.png)

</details>

<details>
  <summary>Lecture 3:  Final placement optimization</summary>
 
###  Final placement optimization

* Buffers are added due to its signal transmission length. For example, the length between FF1 and Din4 is to high, so we need the repeaters to recondition the signal and send it to FF1.
* From FF1 to 1, the length is good enough but since we will going across the other FF and buffer, therefore we need to make it on separated layer. 
* From 1 to 2 is a huge gap. Hence, we consider to put the buffer.
* The more the gate and FF closed to each other, the lower delay.
* Moreover, we need to do timing setup checking to see whether our placement acceptable/not as well as identifying the specifications have been met/not.

![image](https://user-images.githubusercontent.com/118953917/213074569-4be5098a-798d-438d-b07a-de873c8d3c1c.png)

</details>

<details>
  <summary>Lecture 4:  Need for libraries and characterization</summary>
 
###  Libraries characterization and modelling

**Part 1: Concepts and theory - NLDM, CCS timing, power and noise characterization**
  
Steps in mcharacterizing and modelling 
  
![image](https://user-images.githubusercontent.com/118953917/213076122-994fd13f-0d90-4e4c-9913-cbaa9ea1e5df.png)

* Common thing across all stages "GATES or Cells"
* Those things in the library must be categorized as library and making sure that the tools will understand what are the gates/cells are 
* To make the tool understands, we have to design, chategorize and model each gate/cell in such a way that the tool understand using EDA tool
  
![image](https://user-images.githubusercontent.com/118953917/213076726-0596bace-b53c-449b-a4b7-20c8dc8a2fac.png)

  
</details>

<details>
  <summary>Lab 1:  Congestion aware placement using RePlAce</summary>
 
###  Congestion aware placement using RePlAce

* Global placement: assigns general locations to movable objects. It used to reduce wire length
* Detailed placement: refines object locations to legal cell sites and enforces non-overlapping constraints 
* The detailed locations enable more accurate estimations of the circuit delay for the purpose of timing optimization
* Legalization is an essential step where the overlaps between gates/macros must be removed

> In openLANE
```
run_placement
```
  
> In terminal
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-01_14-09/results/placement
magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
  
* Half Parameter Wire Length (HPWL) is applied to reduce wire length
  
