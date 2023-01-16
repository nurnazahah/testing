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
  
![image](https://user-images.githubusercontent.com/118953917/212591511-3af66b9e-e24a-4fed-9ef9-9192b0693289.png)
  
* How to arrive on its dimensions?
5. Place all the logic cells inside the core
  * The logical cells occupies the complete area of the core with 100% utilization
  * If there is some areas left out, it will not be 100% utilization, it may be some other number
  
  
![image](https://user-images.githubusercontent.com/118953917/212591281-a945f01a-0a28-42ef-92c6-68f47e442ef7.png)
  
* Since utilization factor = 1, equivalent to 100% utilization, and if there is some extra logic needs to be added, that is not allowed because there is no space left
* This 100% utilization is usually as an ideal scenario, but in practical scenario, we only utilized up until 50% to 60% utilization
* Whenever the aspect ratio = 1, it signifies that the chip is in square-shaped, else if aspect ratio is the other number except 1, it signifies that the chip is in rectangular-shaped

![image](https://user-images.githubusercontent.com/118953917/212593627-1cd1e73b-836a-47f9-bbb7-c98e3dd51bfb.png)
  
</details>

<details>
  <summary>Lecture 1: Concept of pre-placed cells</summary>
 
### Concept of pre-placed cells
  
**Defining the location of the pre-placed cells**
  
