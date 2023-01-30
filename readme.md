## Day-20

### Topic: Floorplanning and power planning labs

<details>
  <summary>Theory</summary>
 
### Physical Design Flow
  
* Refers to a fundamental process of converting synthesized netlist design restriction and standard library to a layout as per the design rules provided by the foundary. The layout is sent to the foundary for the chip creation.
  
* It is an algorithm with definite objectives, some of them consist of wire length, minimum area, and power optimization.
  
* Steps in the Physical Design Flow are divided into several main processes. 
  
* Firstly, partitioning, where it divides a circuit into smaller sub-circuits or modules, each of which can be constructed and examined separately.
  
* Chip planning may consists of floorplanning and power planning process.
  
* Floorplanning determines the dimensions of all the blocks and place them in appropriate spots on the chip.  
  
* Another step is power planning, which distributes power (VDD) and ground (GND) nets throughout the chip, and is commonly associated with floorplanning.  
  
* Placement is the process of determining the geographic placements of all cells within a block.  
  
* Clock network/tree synthesis establishes how the clock signal is buffered, gated and routed to fulfil specified skew and latency criteria. 
  
* The next step would be signal/global routing which allocates routing resources that are used for connections. Within the global routing resources, detailed routing assigns routes to individual metal layers and routing tracks. 
  
* Lastly, timing closure is used for unique placement or routing strategies to improve circuit performance.
  
*Source: https://chipedge.com/steps-in-vlsi-physical-design-flow/#:~:text=VLSI%20Physical%20Design%20Flow%20is,the%20creation%20of%20the%20chip.*
  
*Source: slide material provided for Day 20 of training*
  
![image](https://user-images.githubusercontent.com/118953917/215411723-547f98c2-d8ac-446c-92b2-19e020ed9f1c.png)

