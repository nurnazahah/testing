## Day-6

### Topic: Introduction to Logic Synthesis

<details>
  <summary>Introduction to the course</summary>
 

### Introduction to the course
  
**Tools used**
* iVerilog: for verilog compilation and simulation
* gtkwave: for viewing the simulation output
* Synopsys design compiler: for logic synthesis
* skywater 130nm library
  
**Pre-requisites**
* Digital electronics
* Boolean algebra
* Verilog Hardware Description Level (HDL) coding 
* Idea of basic synthesis


**What to expect from the course?**
* Understand various steps involved in Digital Logic Synthesis
* Understand and write Synopsis Design Constrants (SDC) for the given module
* Perform synthesis and write out netlist using design compiler
* Generate and analyze the synthesis reports/STA reports
  
**Basics of digital logic design and synthesis**
* Digital logic: including switching function, and automation and decision making
* Behavioral model of the design written in HDL: using VHDL and verilog
  
gambar 68 & 69
  
**Synthesis**
* RTL to gate level translation
* The design is converted into gates and the connections are made between the gates
* This is given out as a file called netlist
  
gambar 70

**What is .lib?**
* Collection of logical modules
* Includes basic logic gates like AND, OR, NOT, etc.
* Different flavors of the same gate
  
gambar 70
  
Why different flavours of gate is needed?
* Combinational delay in logic path determines the maximum speed of operation of digital logic circuit
* So, we need cells that work faster to make Tcombi small

gambar 71
  
Why we need slow cells?
* To ensure there are no "HOLD" issues at DFF_B, we need cells that work slowly
* Hence, we need cells that work fast to meet the required performance and we need cells that work slow to meet HOLD
* The collection forms the .lib
  
gambar 72
  
**Selection of Cells**
* Need to guide the synthesizer to select the flavour of the cells that is optimum for the implementation of logic circuit
* More use of faster cells in bad circuit in terms of Power and Area
* More use of slower cells in sluggish circuit where it may not meet the performance needed
* The guidance offered to the synthesizer -> Constraints
  
**Synthesis illustration**

The circuit on the right is created from RTL using the gates available in the .Lib and given out as netlist
  
gambar 73
  
**Logic synthesis basics**
  
Notes: Example is taken from instructor's video

gambar 74
  
**What to achieve in logic synthesis?**

  Working digital logic circuit is: 
* Logically correct
* Electrically correct
* Timing met

To give more delay to the circuit to meet setup/hold time, add buffers. However, additional buffers to meet hold, will add additional area. 
  
How to decide for the correct implementation of the design?
-> By using constraints
  * Constraints are the guide to the synthesizer to pick the correct library cells which is most appropriate for the design
  * The illustrations are picked based on the needs
</details>
  
<details>
  <summary>Introduction to DC</summary>
 

### Introduction to Design Compiler (DC)

**What is DC?**
* Design Compiler (DC): Synthesis tool targeted for ASIC design flow from Synopsys.
* Features of DC: 
  -> Established as a premium synthesis tool across semiconductor industry.
  
  -> Interoperability with various backend tools from Synopsys.
  
  -> Has the ability to perform DFT Scan stitch.
  
  -> Can handle huge designs with extreme complexity and provide very good Quality of Results (QoR).
  
  
**Common Terminologies associated with DC**
* Synopsis Design Constraints (SDC): Industry standard that is used across Electronic Design Automation (EDA) implementation tools. These are the design constraints which are supplied to DC to enable appropriate optimization suitable for achieving the best implementation.
* .Lib: Design library whicb contains the standard cells.
*DB: Same as .lib but in a different format. DC understands libraries in .db format.
* DDC: Synopsys proprietary format for storing the design information. DC can write out and read in DDC.
* DESIGN: RTL files which has the behavioral model of the design.
  
**Synopsys Design Constraints (SDC) format**
* Design intent in terms of timing, power and area constraints.
* Supported by different EDA tools across semiconductor industry.
* SDC is based on Tool Command Language (TCL).
  
**DC Setup**
gambar 75
  
**Implementation flow of ASIC**
Steps in converting RTL to the physical database (GDS format).

  GAMBAR 76
  
DC Synthesis Flow
gambar 76
 
</details>
  
<details>
  <summary>Lab 1: Invoking DC basic setup</summary>
 

### Lab 1: Invoking DC basic setup

>> Invoke DC setup
> git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
> cd sky130RTLDesignAndSynthesisWorkshop/
> /p/hdk/pu_tu/prd/sams/mig76_wlw/setup/enter_p31 -cfg ip76p31r08hp7rev03 -ov ./


gambar 77/78

.lib file location
gambar 78

> gvim sky130_fd_sc_hd__tt_025C_1v80.lib
> :syn off

gambar 79
  
> csh
> dc_shell

gambar 79
  
> echo $target_library
> echo $link_library

gambar 79 bawah

>> exit dc_shell and open gvim
> gvim DC_WORKSHOP/verilog_files/lab1_flop_with_en.v

gambar 80
  
>> Invoke dc_shell 
> read_verilog DC_WORKSHOP/verilog_files/lab1_flop_with_en.v
> write -f verilog -out lab1_net.v
> sh gvim lab1_net.v

  gambar 81
  
> read_db DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.db
> write -f verilog -out lab1_net.v
> sh gvim lab1_net.v

gambar 82
  
> echo $link_library
> echo $target_library
> set target_library /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/sky130RTLDesignAndSynthesisWorkshop/DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.db
> set link_library {* /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/sky130RTLDesignAndSynthesisWorkshop/DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.db}
> link
> compile
> write -f verilog -out lab1_net.v
> sh gvim lab1_net.v
  
gambar 83 all

</details>
  
<details>
  <summary>Lab 2: Introduction to DDC GUI with design_vision</summary>
 

### Lab 2: Introduction to DDC GUI with design_vision

>> Invoke GUI format of DC
> csh
> design_vision

  gambar 84
  
> write -f ddc -out lab1.ddc
  
gambar 85 atas 
  
> design_vision > read_ddc lab1.ddc (loaded in one terminal)
  
> design_vision> read_verilog DC_WORKSHOP/verilog_files/lab1_flop_with_en.v (open in another terminal)
  
gambar 86
  
Schematic design output
  
gambar 87
</details>
  
<details>
  <summary>Lab 3: DC synopsys DC setup</summary>
 

### Lab 3: DC synopsys DC setup
  
>> Steps to invoke dc_shell everytime we need to reopen dc_shell
> cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/sky130RTLDesignAndSynthesisWorkshop/
> /p/hdk/pu_tu/prd/sams/mig76_wlw/setup/enter_p31 -cfg ip76p31r08hp7rev03 -ov ./
> csh
> dc_shell
> set target_library /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/sky130RTLDesignAndSynthesisWorkshop/DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.db
> set link_library {* target_library}
> echo $target_library
> echo $link_library

  gambar 88
  
>> Preconfigure at home directory and file name is .synopsys_dc.setup
> pwd -> to ensure it is at the home directory
> gvim .synopsys_dc.setup -> ensure the file name is as stated
>> Insert the contents inside gvim
> set target_library /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/sky130RTLDesignAndSynthesisWorkshop/DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.db
> set link_library {* target_library}
>> Save and exit gvim
> :wq

gambar 89 atas
  
>> Open dc_shell and display the output
> echo $target_library
> echo $link_library
  
gambar 89 bawah
  
</details>
  
<details>
  <summary>TCL Quick Refresher</summary>
 

### TCL Quick Refresher

Advanced synthesis and STA using DC - TCL quick refresher
  
**TCL: Quick refresher**
  
* set 
  
  -> Used for creating/assigning values and storing information in variables
  
  -> Example: set a 5 --> a = 5
  
  -> set a [expr $a + $b] --> a = a + b
  
Note: square brackets [] are used for nesting the commands in TCL 
  
Notes: Example is taken from instructor's video
  
gambar 90
  
**While loop**

Notes: Example is taken from instructor's video
  
gambar 91
  
**For loop**

Notes: Example is taken from instructor's video
  
gambar 92 

**Foreach in TCL**
  
Notes: Example is taken from instructor's video
  
gambar 93
  
**DC specific**
  
Notes: Example is taken from instructor's video
  
gambar 94
  
</details>
  
<details>
  <summary>Lab 4:  TCL scripting</summary>
 

### Lab 4:  TCL scripting
  
>> Invoke dc_shell and try simple command in tcl scripting
> set i 0
> echo $i
> incr i
> echo $i 

**For loop**
  
gambar 95
  
**While loop**
  
gambar 96
  
**Foreach loop**
  
gambar 97
  
