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

gambar 1

gambar 2

```
./simv
dve &                         (Error -> should state how many bits to display in the command)
dve -full64                   (Error fixed)
```

gambar 3

> DAC modelling
```
/nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/dac
git clone https://github.com/vsdip/rvmyth_avsddac_interface.git
csh
cp -r rvmyth_avsddac_interface /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/babysoc
vcs rvmyth_avsddac_interface/iverilog/Pre-synthesis/avsddac.v rvmyth_avsddac_interface/iverilog/Pre-synthesis/avsddac_tb_test.v


