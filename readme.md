## Day-11

### Topic: BabySoC Modelling

<details>
  <summary>Theory</summary>
 
### Lab: Theory

**What is modelling?**

* Modelling and simulation (M&S) is the use of a physical/logical representation of a given system to generate data and help determine decisions/make predictions about the system.
* M&S is widely used in the VLSI domain.

**Purpose of modelling**

System models are specifically developed to support analysis, specification, design, verification and validation of a system, as well as to communicate certain information.

**What are we modelling? (VSDBabySoC)**

* Some initial input signals will be fed into vsdbabysoc module
* PLL will start generating the proper CLK for the circuit
* Clock signal will make the rvmyth to execute instructions and some values are generated, these values are used by DAC core to provide the final output signal named OUT
* There are 3 main elements (IP cores) and a wrapper as SoC and also a testbench module

**RVMYTH - Risc-V based MYTH**

* RISC: Reduced instruction set computer
* RISC-V (“risk-five”): a base integer ISA, which must be presented in any implementation, plus optional extensions to the base ISA
* Each base integer instruction set is characterized by the width of the integer registers and the corresponding size of the address space and by the number of integer registers. There are two primary base integer variants, RV32I and RV64I

**Waterfall flow diagram for a pipelined Risc-v processor**

*Source: https://www.vlsisystemdesign.com/risc-v-waterfall-diagram-and-hazards/*

![image](https://user-images.githubusercontent.com/118953917/210520422-1b72c586-3ba4-4fbd-bca4-3fa881778c78.png)

**Phase Locked Loop (PLL)**

* Electronic circuit with a voltage or voltage-driven oscillator that constantlyadjusts to match the frequency of an input signal
* Purposes: to generate, stabilize, modulate, demodulate etc

**Why off-chip clocks can’t be used all the time?**

* Clock will be a supply for a lot of blocks on the chip, it will have delays due to long wires (if used only one clock source) - i.e. clock jitter
* Some blocks might need 200 MHz and some might need 100 MHz -> different frequencies just on one small chip
* Concept of ppm (clock accuracy) comes in, whenever quartz is acquired, it comes with x ppm error

**PLL used on SoC**

Main components:
* Phase detector
* Loop filter
* Voltage controlled oscillator
* Frequency divider 

*Source: https://onedrive.live.com/?authkey=%21AOwVpzXkVogukuk&cid=E0E9B5EEF85B162E&id=E0E9B5EEF85B162E%2199346&parId=E0E9B5EEF85B162E%2197363&o=OneUp*

![image](https://user-images.githubusercontent.com/118953917/210522255-2b1931b5-f254-47b7-b980-2388358680ec.png)

**Digital-to-Analog Converter (DAC)**

* Converts a digital input signal into an analog output signal
* Digital signal is represented with a binary code, which is a combination of bits 0 and 1. DAC consists of a number of binary inputs and a single output
* 2 types of DAC
  + Weighted Resistor DAC
  + R-2R Ladder DAC
  
**Tips on modelling the design**

* Avoid race Conditions -> can use VCS race detection tool
* Use an optimized Testbench for debugging the design
* Creating models that simulate faster
* Follows case statement behaviour

**DVE (Normal mode)**

* DVE provides a graphical user interface (GUI) to debug the design
* Steps to open GUI with normal mode:
  + Run the design with a valid testbench
  + Compile them
  + Cross check .vcd file 
  + Invoking DVE tool
  
**DVE (Interactive mode)**

* Debugging the design in interactive mode or in post-processing mode with a valid testbench
* Steps to open GUI with interactive mode:
  + Run the design with a valid testbench
  + Get the code dump file (.vcd)
  + This should open the tool automatically and we can fully run our test bench or debug it step by step

**RVMYTH modelling**

* RISC-V CPU core has been written in Verilog and already written testbench code for the same
* Entire C program will be converted into a hex format and will be loaded into memory
* CPU will then read the contents of the memory, process it and display the output result of sum numbers from 1 to n


</details>

<details>
  <summary>Lab</summary>
 

### Lab: Modelling

> Differences of interactive modes

> RVMYTH (RISC-V) modelling
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/babysoc
git clone https://github.com/kunalg123/rvmyth/
cd rvmyth
/p/hdk/pu_tu/prd/sams/mig76_wlw/setup/enter_p31 -cfg ip76p31r08hp7rev03 -ov ./
csh
vcs mythcore_test.v tb_mythcore_test.v                                                    (Got 1 error -> need to refer both files and fix that)
```

![image](https://user-images.githubusercontent.com/118953917/210686222-acdb0f86-e0d9-4352-9bc5-f6c1ae18a32e.png)
  
![image](https://user-images.githubusercontent.com/118953917/210686260-b0f35375-e60e-4764-9569-e95783155564.png)

```
./simv
dve &                         (Error -> should state how many bits to display in the command)
dve -full64                   (Error fixed)
```

![image](https://user-images.githubusercontent.com/118953917/210829380-5709b5fb-bfbc-45c1-815f-a132491061da.png)

> DAC modelling
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/dac
git clone https://github.com/vsdip/rvmyth_avsddac_interface.git
csh
cp -r rvmyth_avsddac_interface /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/babysoc
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/babysoc/rvmyth_avsddac_interface/iverilog/Pre-synthesis
vcs avsddac.v avsddac_tb_test.v
```
  
![image](https://user-images.githubusercontent.com/118953917/210829629-061ef761-6b84-4546-b762-b450005d89f1.png)
  
```
vcs -sverilog avsddac.v avsddac_tb_test.v           (To notice the system to support system verilog)
./simv
dve -full64  
```
  
![image](https://user-images.githubusercontent.com/118953917/210829699-97193411-e76c-4a35-93ec-146c18c27dab.png)
  
> PLL modelling 
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/babysoc/rvmyth_avsdpll_interface/verilog
csh
vcs avsd_pll_1v8.v pll_tb.v
./simv
dve -full64 
```
  
![image](https://user-images.githubusercontent.com/118953917/210829788-20be3b5d-6607-4669-a8a3-70e865becf26.png)

![image](https://user-images.githubusercontent.com/118953917/210829879-f03f9769-21ae-43b9-a307-22ed03ca1cd7.png)
  
> Simulating in interactive mode (debug mode)
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/babysoc/rvmyth
csh
vcs -lca -debug_access+all -full64 mythcore_test.v tb_mythcore_test.v
./simv -gui &
```
  
![image](https://user-images.githubusercontent.com/118953917/210829938-b59a4a6b-025a-4def-a5bc-f9c8cac94da4.png)
  
![image](https://user-images.githubusercontent.com/118953917/210829980-683feb81-ba33-4f16-9c46-0448d6b6c0e1.png)
  
>> Interfacing blocks together
> risc_v and pll 
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/babysoc
git clone https://github.com/vsdip/rvmyth_avsdpll_interface
cp -r rvmyth_avsdpll_interface /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/babysoc
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/babysoc/rvmyth_avsdpll_interface/verilog
csh
vcs rvmyth_pll.v rvmyth_pll_tb.v
```
  
![image](https://user-images.githubusercontent.com/118953917/210830075-142ceaae-d603-43e2-b27f-d6210a2e2280.png)
  
```
./simv
dve -full64 
```
  
![image](https://user-images.githubusercontent.com/118953917/210830186-d91efcb5-769a-4c19-9880-fe5cbf311dbe.png)
  
> DAC and RVMYTH
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/babysoc/rvmyth_avsdpll_interface/verilog
csh
vcs rvmyth_avsddac.v rvmyth_avsddac_TB.v
```

![image](https://user-images.githubusercontent.com/118953917/210830252-352144b5-3113-472b-8f7d-576dcbb6a9b2.png)
  
![image](https://user-images.githubusercontent.com/118953917/210830312-41b01106-2b8e-457d-85a0-c2f087d29c34.png)
  
```
vcs -sverilog rvmyth_avsddac2.v rvmyth_avsddac_TB2.v
./simv
dve -full64 
```
  
![image](https://user-images.githubusercontent.com/118953917/210830355-0a3cec67-4fde-4b67-b127-6bf3f420c1a6.png)
  



  
</details>

<details>
  <summary>Task</summary>
 

### Lab: Task to do
  
**Differences of the modes in interactive mode**
  
![image](https://user-images.githubusercontent.com/118953917/210830922-aa9a5a54-68e8-43d2-8e8e-d41836431916.png)
  
> Design chosen: up counter
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/babysoc/
csh 
vcs upcntr.v tb_upcntr.v
./simv
dve -full64 
```
  
![image](https://user-images.githubusercontent.com/118953917/210831067-ad8d9cd5-15de-432b-bcb8-08574be2f5ba.png)
  
**Block diagram of upcounter**
  
![image](https://user-images.githubusercontent.com/118953917/210831238-90c5c31c-35fd-4f46-bdb8-5c2ebfbaae03.png)

![image](https://user-images.githubusercontent.com/118953917/210831372-6e9e77e3-d9fe-4470-b080-3ed5f5159c26.png)

