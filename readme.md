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
  
* Overview of pre-placed cells on a netlist across the decoupling capacitors and the blocks
  
![image](https://user-images.githubusercontent.com/118953917/212817436-6134ebd8-14bc-4e5f-970b-bcce194cc73a.png)

</details>

<details>
<summary>Lecture 4: Power planning</summary>

### Power planning




  
  
