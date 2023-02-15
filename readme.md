## Day-25

### Topic: RISC-V core RTL2GDS flow

<details>
  <summary>Theory</summary>
  
### 
  

  
</details>

<details>
  <summary>Lab</summary>
 
### 
  

  
</details>

## Day-26

### Topic: Introduction to mixed-signal flow

<details>
  <summary>Theory</summary>
  
### Understanding mixed signal design
  
**What is mixed signal design?**
  
* What is electronic signals?
  
Electronic signal is a message/information encoded by changing the voltage of an electric current and it is used to communicate within several devices
  
* Types of electronic signals
  
Analog and digital signals
  
* What kind of signal that is most available in the nature? 
  
Analog signals since every process produced analog signals instead of digital signals. Digital signals are usually being converted from the analog signals.
  
* Which signal does microcontrollers/microprocessors understand/speak?
  
Digital signals where those electronic devices only understand the signals digitally by using 1's and 0's 
  
* Since microprocessors/microcontrollers speak in digital form, but we need to get or take in analog signals, what to do?
  
Convert the digital signals to analog signals or vice versa using ADC/DAC
  
* **Mixed-signal chips** are those that at least partially deal with input signals whose precise values matter
  
  1. This broad class includes RF, Analog, Analog-to-Digital and Digital-to-Analog conversion
  2. More recently, a large number of Mixed-Signal chips where at least part of the chip design needs to measure signals with high precision
  3. These chips have very different Design and Process Technology demands than normal Digital circuits
  
### AMS: Analog and Mixed Signal (digital and analog)
  
**AMS flow**
  
![image](https://user-images.githubusercontent.com/118953917/218935017-b6e05d14-a790-4607-8612-a27c89c5b98b.png)

**Block diagram representation for mixed signal design**

![image](https://user-images.githubusercontent.com/118953917/218935237-12b92d40-67b3-4777-aec7-15b6ad7fc9c2.png)

### Exploring the example of VSDBabySoC
  
* RVMYTH processor --> digital block
* PLL --> analog block
* DAC --> analog block (for digital to analog conversion)
  
### Introduction to various files
  
* LEF (Library Exchange Format) file: physical properties such as width, height etc regarding the standard cells
  + tf (technology file) or tlef (technology lef) --> contains same information
  + Cell tf
  
* LIBerty file --> contains timing information of the cells
  
* gdsII and OASIS file --> GDSII is a file format similar to JPEG, DOCX, XLSX etc to enable a layout design to be transferred from one place to another (IP owner handoff to PD team, PD team to foundry for fabrication), to be viewed/used for verifications like Physical verification checks by EDA tools.
  
*Source: https://teamvlsi.com/2020/08/inputs-for-physical-design-physical-design-input-files.html*

![image](https://user-images.githubusercontent.com/118953917/218978973-19dae4f1-8f5a-4916-acdc-a9cfdf711425.png)

* PnR tool

The tool tries to place the standard cell in such a way that the design should have minimal congestions and the best timing. Every PnR tool provides various commands/switches so that users can optimize the design in a better way in terms of timing, congestion, area, and power as per their requirements.
  
* WHAT and WHY TO DO after getting the information?
  
This is why we need the following files:
  
  1. LEF file
  2. LIB file
  3. Tf files (tlu+ file)
  
*Source: https://www.slideshare.net/vlsisyst/vlsi-physical-design-flow*
  
![image](https://user-images.githubusercontent.com/118953917/218981759-27c9b056-ae06-481c-8ac1-826549041f43.png)
  
* Sources of various files
  
*Source: notes are taken from lecture slides*
  
![image](https://user-images.githubusercontent.com/118953917/218983204-5a925e58-9748-4ec4-902d-9c804e265cbd.png)
  
### Discovering IP cores
  
**What are IP cores?**
  
* An IP core consists of a block of logic or data that is used in a semiconductor chip.
  
* It is usually the intellectual property of a particular person or company. IP cores are used when making a fieldprogrammable gate array (FPGA) or application-specific integrated circuit (ASIC).
  
* IP cores are created throughout the design process and can be turned into components for reuse.
  
* There are different categories for IP cores including hard IP cores and soft IP cores.
  
* The soft IP core , can be customized during the physical design phase and mapped to any process technology.
 
* A hard IP core is one that has the logic implementation and the physical implementation. In other words, the physicallayout of a hard macro-IP is finished and fixed in a particular process technology.
  
**How it works in semiconductor industry**
  
*Source: notes are taken from lecture slides*
  
![image](https://user-images.githubusercontent.com/118953917/218985408-52ba048f-00c0-49bf-b1af-694b6094a823.png)

  
</details>

<details>
  <summary>Lab</summary>
 
### 
  

  
</details>
