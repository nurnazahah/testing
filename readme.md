## Day-15

### Topic: Inception of EDA and PDK

<details>
  <summary>How to talk to computers</summary>
 
### How to talk to computers?
  
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
  <summary>From Software Applications to Hardware</summary>
 
### Communication of Software and Hardware
  

  
