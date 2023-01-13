## Day-15

### Topic: Inception of EDA and PDK

<details>
  <summary>What is a package?</summary>
 
### Introduction to QFN-48 Package, chip, pads, core, die and IPs
  
**What is package?**
  
It is the material that contains semiconductor device attached with a die using wire bonding. Package is a container that holds die and was connected to outside (external device) by using wire bonding. Example of package - Quadruple in-line package (QIP) and Dual in-line package (DIP).
  
**Example of a real package in industry**

![image](https://user-images.githubusercontent.com/118953917/212086264-1703bc90-452c-41f9-8c76-f9f7bff3aef0.png)
  
**Example of typical ports in an Arduino**
  
![image](https://user-images.githubusercontent.com/118953917/212086463-019c1922-2dae-4ca4-8c55-0e57a5b24c28.png)
  
**Example of the package inside Arduino**
  
* Package: QFN-48
* Quadflat no leads
* Comes in a square size where both length and width are 7mm
* The chip will be placed at the centra; of the package
* Wire bond will be interconnecting the chip with the ports and pins
  
*Source: https://www.vlsisystemdesign.com/*
  
![image](https://user-images.githubusercontent.com/118953917/212086529-6dee0954-fca5-4189-9e9e-056381851004.png)
  
**Elements inside a package**
  
* Pad: surface mount of pins connected and it is used to connect inside (core) to outside (I/O), good at ESD protection to prevent charge coming from outside that would damage the core inside
* Core: consists of all the main logic gate (NMOS/PMOS) and cell block such as macro cell and foundry IP's
* Die: the actual silicon chip (IC) that would normally be inside a package/chip
* Foundry IPs: cell with more specific functionality and the design was patent/owned by a company, as well as having a higher value compared to macros and cannot be found online
* Macros: protocol to transfer data using simple functionality and can be found online

</details>
    
<details>
  <summary>Introduction to RISC-V</summary>
 
### Introduction to RISC-V
  
**RISC-V Instruction Set Architecture (ISA)**
  
**Process of converting RISC-V architecture to layout**
  
* The C codes will be compiled in RIS-V assembly language program
* The assembly language will be converted to machine language which is binary language containing logic 0 and logic 1 that is understood by the computer/hardware
The converted binary language will be sketched in a layout by the computer program and we will get the required output
* However, there is another interface that should be presented between architecture and layout of RISC-V which is **Hardware Description Language (HDL)** where it used RTL implementing the specifications of RISC-V
  
*Source: https://www.vlsisystemdesign.com/*

*Credit to: Kunal Gosh*
  
![image](https://user-images.githubusercontent.com/118953917/212091056-24dde7f6-fc24-46cd-9510-09f3237cf2c8.png)

</details>
    
<details>
  <summary>How to talk to computers?</summary>
 
### Communication of Software and Hardware
  
* The tool will undergo synthesis process which is converting software’s instructions in HLL to machine language in binary format
  
**Flow of the the synthesis process**
* Software (HLL) -> System software (assembly language) -> Hardware (machine language)
* Basically, the application software would enter the clock of the system software and the system software would convert the application program into the binary language
* One of the major operation in system software is Operating System (OS) would take the particular applications and convert it into respective assembly language program and binary program, so that the program will be understood by the hardware
* The converted binary language/machine language will fed into the hardware and it will generate the output
  
**Major layer/components of system software**
 
* Operating System (OS)
  + Handle IO operations
  + Allocate memory
  + Low level system functions

* Compiler
  + Converting the programming language into the respective intructions 
  + The syntax of the instruction is depending upon what kind of the hardware is, i.e. if the hardware belongs to RISc-V format, the syntax would have the syntax of RISc-V file format instructions

* Assembler
  + Take particular instructions and convert it into the respective binary number which is machine language program
  + Consisting of logic 1 and logic 0 where the computer/hardware understands the language

**Process flow and its example**
  
*Source: https://www.vlsisystemdesign.com/*
  
![image](https://user-images.githubusercontent.com/118953917/212218492-1e0bc3d1-bbe5-4574-a8dc-4bd8517e652d.png)

![image](https://user-images.githubusercontent.com/118953917/212219102-4e6d41fc-2de0-41fd-b04c-b02675886d0e.png)

</details>
    
<details>
  <summary>Introduction to ASIC design</summary>
 
### Introduction to all components of open-source digital ASIC design
  
* Designing ASIC flow in an automated way requires several elements:
  
  + Hardware Description Language (HDL) which Register Transfer Level (RTL) model of the function we want to implement including RTL's IP
  + Tool used for automation i.e. EDA tools
  + Process Design Kit (PDK) data
  
**Open source digital ASIC design**
  
*Source: the image has been taken from instructor's video*
  
![image](https://user-images.githubusercontent.com/118953917/212222308-d4607e49-a968-45c1-b69c-54b25f369950.png)
  
**Getting to know PDK**
  
* PDK: the interface between the fabrications and the designers
* PDK -> stands for Process Design Kit

* Collection of files used to model a fabrication process for EDA tools used to design an IC
  + Process Design Rules: DRC, LVS, PEX
  + Device models
  + Digital Standard Cell Libraries
  + I/O libraries
  
**List of EDA tools available**
  
* HDL simulation
* Floor planning
* Detail placement
* HDL design entry
* IR drop analysis
* DRC
* Logic equivalence checking
* Fill insertion
* LVS
* DFM
* RC extraction
* DFT
* Power planning
* STA
* Global placement
* Logic synthesis
* RTL synthesis
* Global routing
* Detailed routing
* Static code analysis
* CTS
  
</details>
    
<details>
  <summary>Simplified RTL2GDS flow</summary>
 
### Simplified RTL to GDS flow
  
**ASIC Design Flow**

* Flow objective: RTL to GDSII
  + Also called Automated PnR and/or Physical Implementation
  
**Simplified RTL to GDSII flow**
  
* major implementations of ASIC flow:
  + Synthesis 
  + Floor/power planning
  + Placement 
  + Clock tree Synthesis
  + Routing
  + Sign-off
  
![image](https://user-images.githubusercontent.com/118953917/212224976-4eef3690-43ec-4240-a466-7065c5eb7865.png)

**Synthesis**
  
* The process of converting RTL to a circuit out of components from the standard cell library (SCL)
* The result of the circuit is described in HDL and usually refered to as gate level netlist, also it is functional equivalent to RTL

* "Standard Cells" have regular layout and each cell has different views/models:
  + Electrical, HDL, SPICE
  + Layout (Abstract and detailed)

*Source: http://vlsitechnology.org/*
  
![image](https://user-images.githubusercontent.com/118953917/212226980-4c5645c4-1086-4456-b1a0-9acc4f9fd039.png)

**Floor and Power Planning**
  
* Chip Floor Planning: partition the chip die between different system building blocks and place the IO pads
  
*Source: the image has been taken from instructor's video*
  
![image](https://user-images.githubusercontent.com/118953917/212227502-cc0232d6-b93b-439d-85e4-f17d608ca454.png)
  
* Macro Floor Planning: dimensions, pin locations, rows and definition
* Power Planning: the power is provided  to the every macros, standard cells, and all other cells are present in the design. Typically, the power distribution network using upper metal layers since they are thicker than lower metal layers
  
*Source: the image has been taken from instructor's video*
  
![image](https://user-images.githubusercontent.com/118953917/212228282-a5655967-278a-4123-a2a3-8ac4059a86fc.png)

**Placement**
  
* The process of placing the cells on the floor plan rows, aligned with the sites
* Macros would place the gate level netlist cells on the rows and the cells should be placed very close to each other to reduce an interconnect delays and enable successful routing 
* Usually done in two steps: 
  + Global: finding the 
  + Detailed 
  
*Source: the image has been taken from instructor's video*
  
![image](https://user-images.githubusercontent.com/118953917/212229348-d78afab6-bb60-452a-a4b5-91c171ae389b.png)

![image](https://user-images.githubusercontent.com/118953917/212229384-2435bef0-9e0d-44e5-be2a-484b8cc5fe5e.png)
