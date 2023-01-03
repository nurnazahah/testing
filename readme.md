## Day-11

### Topic: Introduction to the BabySoC

<details>
  <summary>SoC</summary>
 

### Introduction to BabySoC

**What is System on Chip (SoC)?**

* SoC is a single-die chip that has some different IP cores on it. These IPs could vary from microprocessors which are completely digital to 5G Broadband modems which are completely analog. 
* The structural design of SoC usually includes a central processing unit, memory, ports for inputs and outputs, secondary storage devices, and peripheral interfaces i.e. timers and etc.

**Why SoC is used?**

* Depending upon the requirement, it can also consists of a digital/analog signal processing system or a floating-point unit.
* SoC with equivalent functionality will have increased performance and reduced power consumption as well as a smaller semiconductor die area.

**Typical structure of MediaTek Dimensity 9000 Processor**

* This processor is made using TSMC’s 4nm semiconductor chip fabrication process, and it is said to be more power-efficient compared to Samsung’s 4nm process which is used to make the Exynos 2200 and the Qualcomm Snapdragon 8 Gen 1. 
* The precessor has a 1+3+4 CPU core configuration with one ARM Cortex-X2 CPU core clocked at 3.05GHz, three ARM Cortex-A710 CPU cores clocked at 2.85GHz, and four ARM Cortex-A510 CPU cores clocked at 1.8GHz.

*Source: https://onsitego.com/blog/mediatek-dimensity-9000-smartphones-list/#:~:text=The%20MediaTek%20Dimensity%209000%20processor%20is%20made%20using%20TSMC's%204nm,3%2B4%20CPU%20core%20configuration.*

![image](https://user-images.githubusercontent.com/118953917/210308234-d3128bbf-0ed6-4ceb-b4e4-895c0737b607.png)

**Types of SoC**

* SoCs built around a microcontroller (board).
* SoCs built around a microprocessor (chip), where it is often found in cell phones.
* Specialized application-specific integrated circuit SoCs designed for specific applications that do not fit into the above two categories.

**SoC Structure**

* SoC consists of hardware functional units, including microprocessors that run software code, as well as communication subsystems to connect, control, direct and interface between these functional modules. 
* **Functional components**: processor cores, memory, interfaces, digital signal processor (DSP), others.
* Intermodule communication**: bus-based communication, network on a chip.

**SoC design flow**

![image](https://user-images.githubusercontent.com/118953917/210308326-272eefdc-d3bd-4238-8e1e-d7bb9acadefc.png)

</details>

<details>
  <summary>BabySoc</summary>
 

### Introduction to BabySoc

* BabySoc is a small yet powerful RISCV-based SoC. 
* The main purpose of designing such a small SoC is to test three open-source IP cores together for the first time and calibrate the analog part of it. 
* BabySoC contains one RVMYTH microprocessor, an 8x-PLL to generate a stable clock, and a 10-bit DAC to communicate with other analog devices.

*Source: https://github.com/manili/VSDBabySoC#introduction-to-the-vsdbabysoc*

![image](https://user-images.githubusercontent.com/118953917/210308374-b36bf404-f021-4415-a0fc-2ac8b37852f7.png)

* SoC is an IC that integrates multiple components of a system onto a single chip.
* MPSoC addresses performance requirements. 

*Source: https://www.ecb.torontomu.ca/~courses/coe838/lectures/Intro-SoC.pdf*

![image](https://user-images.githubusercontent.com/118953917/210308411-01184a7b-44da-48d3-95c9-40e4ca249e67.png)

**Baby SoC components**

* RVMYTH: RVMYTH core is a simple RISCV-based CPU, introduced in a workshop by RedwoodEDA and VSD.
* Phase-locked loop (PLL): a control system that generates an output signal whose phase is related to the phase of an input signal. PLLs are widely used for synchronization purposes, including clock generation and distribution.
* Digital-to-analog converter (DAC): a system that converts a digital signal into an analog signal. DACs are widely used in modern communication systems enabling the generation of digitally-defined transmission signals.

*Source: https://github.com/manili/VSDBabySoC#problem-statement*

**Phase-locked loop (PLL)**

* PLL is a closed loop feedback circuit comprises of four main blocks.
* PLL mainly consists of four components: 
  + Charge pump (CP)
  + Phase Frequency Detector (PFD)
  + Voltage Controlled Oscillator (VCO)
  + Loop Filter
  + Frequency Divider (FD)
* These blocks are connected to form a closed loop feedback network so as to synchronize the output with the input in both phase and frequency. 
* This loop continues to run until PLL locked condition is achieved.
* As technology scales down, PLL with wide tuning range, low jitter, and PLL operating at high frequencies are preferred.

*Source: https://github.com/bharath19-gs/avsdpll28nm/#PLL-introduction*

![image](https://user-images.githubusercontent.com/118953917/210308453-089b2d25-3e34-4570-b945-cc8794c3c25c.png)

**Digital-to-analog converter (DAC)**

* DAC converts a digital input signal into an analog output signal. 
* The digital signal is represented with a binary code, which is a combination of bits 0 and 1. DAC consists of a number of binary inputs and a single output. 
* There are two types of DACs:
  + Weighted Resistor DAC
  + R-2R Ladder DAC
  
*Source: https://github.com/Devipriya1921/avsddac28nm#introduction*

</details>

<details>
  <summary>BabySoC Modeling</summary>
 

### Introduction to Modelling

* BabySoC modelling is simulated using ```iverilog``` as well as ```gtkwave``` tool  to display the result.
* Some initial input signals will be fed into BabySoC module making the PLL to start generating the proper ```CLK``` for the circuit.
* The clock signal will make the ```rvmyth``` to execute instructions in its ```imem```. As a result, the register ```r17``` will be filled with some values cycle-by-cycle.
* These values are used by DAC core to provide the final output signal named ```OUT```.
* So, we got 3 main elements (IP cores) and a wrapper as an SoC and of-course there would be also a testbench module out there.

*Source: https://github.com/manili/VSDBabySoC#introduction-to-the-vsdbabysoc*

</details>

<details>
  <summary>Microchips fabrication</summary>
 

### How Microchips are made?
  
**Introduction to microchips fabrication**
  
* All chips started out with a very simple raw material which is sand.
* Complex chemical and physical processes are required to create out of the sand an pure monocrystalline silicon ingot.
* The process to manufacture chips from a wafer starts with the layout and design phase where extremely thin wafers are being cut using a special sawing technique.
* They are fabricated in a range of different diameters, the most common sizes are 150, 200 and 300 mm.
* Wafers with large diameters offer more space for chips. 
* Transistors are the smallest control units in microchips. It used to control electric voltages and currents and it is the most important components of electronic circuits.
  
*Source: https://www.infineon.com/cms/en/product/promopages/how-a-chip-is-made-video/*

**Fabrication of wafers from transistors**
  
1. Highly complex chips are made up of billions of integrated and connected transistors, enabling sophisticated circuits i.e. microcontrollers and crypto chips.
2. Firstly, the surface of the wafer is oxidized in a high-temperature furnace operating at approximately 1000°C to create a non-conductive layer.
3. Then, a photo resist material is uniformly distributed on this non-conductive layer.
4. The wafer is then exposed to light through the photomask in special exposure machines known as steppers.
5. The exposed layer of oxide is then etched off in the areas that have been developed using wet or plasma etching.
6. The photoresist is being applied again, the wafer is exposed to light through the mask, and the exposed photoresist is stripped again. 
  
**Doping process**
  
1. Impurity atoms are introduced into the exposed silicon and ion implanter is used to shoot the impurity atoms into the silicon. This changes the conductivity of the exposed silicon by fractions of a micrometer.
2. After the photoresist residue has been stripped, another oxide layer is applied. 
3. The wafer undergoes another cycle of applying photoresist, exposure through the mask and stripping.
4. Contact holes are etched to provide access to the conductive layers, enabling the contacts and interconnections to be integrated in the wafer. 
5. Photoresist and mask are applied once again. 
6. Chemical-mechanical process is used to polish away excess material in order to give the insulation layer above the interconnections the smooth finish.
7. The steps may be repeated multiple times until the IC is completed.
  
**Assembly process (Final stage of the fabrication)**
  
1. The individual chips are placed in a package and terminals are attached.
2. The result is a finished semiconductor device, which can be mounted on circuit boards using different types of terminals. 
3. Over a thousand connection contacts can be realized. 
  * The high levels of precision and quality are essential at every stage of the workflow which is from the production of silicon boules - cleanroom fabrication - quality control.
  
**Overall fabrication process illustration**
  
*Source: https://link.springer.com/article/10.1557/s43577-020-00001-3**
  
![image](https://user-images.githubusercontent.com/118953917/210314860-b1ad8f93-f8ad-43ab-8298-2f5a27d878cf.png)
