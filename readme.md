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
  

**Main steps in Physical Design Flow**
  
* Create a gate-level netlist (after synthesis)
  1. The netlist is the result of the synthesis process and it is the foundation for physical design. 
  2. Synthesis translates RTL designs written in VHDL or Verilog HDL into gate-level specifications that can be understood by the next set of tools. 
  3. The cells employed, their interconnections, the area used, and other parameters are all listed in this netlist.
  
* Floorplanning
  1. Under this step, we calculate the dimensions of all the blocks and place them in appropriate spots on the chip. 
  2. This step is performed to keep the blocks that are highly connected close to one another. 
  
* Partitioning
  1. The next step of partitioning helps in dividing the chip into separate chunks. 
  2. This procedure is performed primarily to distinguish between distinct functional blocks and to facilitate placement and routing. When the design engineer separates the overall design into sub-blocks and then proceeds to design each module during the RTL design phase, this is known as partitioning. 
  
* Placement
  1. Placement is the process of placing the standard cells inside the core boundary in an optimal location. 
  2. The tool tries to place the standard cell in such a way that the design should have minimal congestions and the best timing. 
  3. Every PnR tool provides various commands/switches so that users can optimize the design in a better way in terms of timing, congestion, area, and power as per their requirements. 
  4. Based on the preferences set by the user, the tool tray to place and optimize it for better QoR. 
  5. Placement does not place only the standard cells present in the synthesized netlist but also places many physical only cells and adds buffers/inverters as per the requirement to meet the timings, DRV, and foundry requirements. 
  6. Here are the basic steps which the tool performs during the placement and optimization stage. 
  
* Static Time Analysis  
  1. Static timing analysis (STA) is a method of validating the timing performance of a design by checking all possible paths for timing violations. 
  2. STA breaks a design down into timing paths, calculates the signal propagation delay along each path, and checks for violations of timing constraints inside the design and at the input/output interface. 
  3. Another way to perform timing analysis is to use dynamic simulation, which determines the full behaviour of the circuit for a given set of input stimulus vectors. 
  4. Compared to dynamic simulation, static timing analysis is much faster because it is not necessary to simulate the logical operation of the circuit. 
  5. STA is also more thorough because it checks all timing paths, not just the logical conditions that are sensitized by a set of test vectors. 
  6. However, STA can only check the timing, not the functionality, of a circuit design.
  
* Clock Tree Synthesis (CTS)  
  1. Clock Tree Synthesis(CTS) is one of the crucial steps in VLSI physical design flow. 
  2. It is used to reduce skew and insertion delay. 
  3. This step helps distribute the clock evenly among all sequential elements of a design.
  
* Routing  
  1. Routing helps in making the links between the cells and the blocks. 
  2. There are two types of routing: global routing and detailed routing. 
  3. Connections are routed through global routing, which assigns routing resources. 
  4. It also keeps track of a network’s assignment. 
  5. Whereas, the actual connections are made by detailed routing. 
  
* Physical verification 
  1. Physical verification ensures that the produced layout design is valid. 
  2. This involves ensuring that the layout is correct and includes all technological prerequisites, density verification, cleaning density etc.  
  
*Source: slide material provided for Day 20 of training*
  
![image](https://user-images.githubusercontent.com/118953917/215418212-f6a6c325-c64d-43c9-8628-ef15c05ae168.png)
  
</details>

<details>
  <summary>Lab</summary>
 
### Physical Design Flow
  
Sources
  1. https://github.com/Devipriya1921/VSDBabySoC_ICC2#getting-started-with-vsdbabysoc

  
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20
git clone https://github.com/manili/VSDBabySoC.git
git clone https://github.com/Devipriya1921/VSDBabySoC_ICC2.git
git clone https://github.com/bharath19-gs/synopsys_ICC2flow_130nm.git
```
  
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files
gvim vsdbabysoc.tcl &
gvim avsdpll.lib &
```
  
**vsdbabysoc.tcl**
  
![image](https://user-images.githubusercontent.com/118953917/215930668-24e8267c-5265-4406-9c33-b39c6227e8bc.png)
  
**avsdpll.lib**
  
![image](https://user-images.githubusercontent.com/118953917/215930725-1f1c96f4-b71b-4fe3-824b-26259c964e5c.png)
  
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/shell
/p/hdk/bin/cth_psetup -p ipde/rc -cfg 76p31_r08hp71_ipg.cth -ward . -tool ipde_all -quiet -x '$SETUP_IPDE -b ip76p31ddrgen6mod_ddriolvrpgcombo' 
dc_shell
source /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files/vsdbabysoc.tcl
```
  
![image](https://user-images.githubusercontent.com/118953917/215796297-d6e947f8-be20-4265-ae4a-4b27677cf596.png)

![image](https://user-images.githubusercontent.com/118953917/215796409-4f6e8041-365d-4515-8579-df04c14fa8ff.png)
  
**Reports**
  
**Report area**
  
![image](https://user-images.githubusercontent.com/118953917/215930931-9978ad14-a2f7-4b64-b995-abb653450e34.png)
  
**Report power**

![image](https://user-images.githubusercontent.com/118953917/215930965-ebf8d41f-3169-45f3-929c-83dbef4c7c58.png)
  
**Report timing**

![image](https://user-images.githubusercontent.com/118953917/215931015-297d3787-2f87-418c-97d0-a487abc579be.png)
  
**Report constraints**

![image](https://user-images.githubusercontent.com/118953917/215931057-fcb52a51-7403-4826-b3d4-139b27337cbd.png)
  
### Output schematic
  
![image](https://user-images.githubusercontent.com/118953917/215931319-503a58df-8829-46aa-bd6c-1a472bb0171a.png)
  
**RVMYTH core**
  
![image](https://user-images.githubusercontent.com/118953917/215931465-2f9ae8f5-93f5-47b3-8eef-b450b5992a3d.png)
![image](https://user-images.githubusercontent.com/118953917/215931636-d06c40be-90cc-4970-9bf7-91ff75e03066.png)

### Performing physical design 
  
> Invoking icc2_shell
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/shell
/p/hdk/pu_tu/prd/sams/mig76_wlw/setup/enter_p31 -cfg ip76p31r08hp7rev03 -ov ./
icc2_shell
source /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files/top.tcl
```
  
