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
![day6lab1a](https://user-images.githubusercontent.com/118953917/208397228-54fb7390-6628-48b6-a0cb-cb1261a9d4c7.jpg)
![day6lab1b](https://user-images.githubusercontent.com/118953917/208397248-154bc6cb-def8-438f-abe3-aa0a65d91014.jpg)

  
**Synthesis**
* RTL to gate level translation
* The design is converted into gates and the connections are made between the gates
* This is given out as a file called netlist
  
**What is .lib?**
* Collection of logical modules
* Includes basic logic gates like AND, OR, NOT, etc.
* Different flavors of the same gate
  
gambar 70
  ![day6lab1c](https://user-images.githubusercontent.com/118953917/208397598-1439cc60-2aba-4b9f-bbad-657a1d1dc4da.jpg)
  
Why different flavours of gate is needed?
* Combinational delay in logic path determines the maximum speed of operation of digital logic circuit
* So, we need cells that work faster to make Tcombi small

gambar 71
  ![day6lab1d](https://user-images.githubusercontent.com/118953917/208397629-eb571fb2-3cab-4509-8fb6-dfd46b1be8a0.jpg)
  
Why we need slow cells?
* To ensure there are no "HOLD" issues at DFF_B, we need cells that work slowly
* Hence, we need cells that work fast to meet the required performance and we need cells that work slow to meet HOLD
* The collection forms the .lib
  
gambar 72
  ![day6lab1e](https://user-images.githubusercontent.com/118953917/208397662-fc00fd56-8b85-4cee-808d-f8d9ad62d630.jpg)
  
**Selection of Cells**
* Need to guide the synthesizer to select the flavour of the cells that is optimum for the implementation of logic circuit
* More use of faster cells in bad circuit in terms of Power and Area
* More use of slower cells in sluggish circuit where it may not meet the performance needed
* The guidance offered to the synthesizer -> Constraints
  
**Synthesis illustration**

The circuit on the right is created from RTL using the gates available in the .Lib and given out as netlist
  

  ![day6lab1f](https://user-images.githubusercontent.com/118953917/208397692-66e2ffac-28b6-4861-8724-557b6923503d.jpg)
  
**Logic synthesis basics**
  
Notes: Example is taken from instructor's video


  ![day6lab1g](https://user-images.githubusercontent.com/118953917/208397735-a291b233-b804-47a8-b6a8-6859f50f7981.jpg)
  
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

  ![day6lab1h](https://user-images.githubusercontent.com/118953917/208397835-39698526-7852-4c02-96c3-03faa67aac11.jpg)
  
**Implementation flow of ASIC**
Steps in converting RTL to the physical database (GDS format).


  ![day6lab1i](https://user-images.githubusercontent.com/118953917/208401949-378ad2bf-7629-4ffd-b903-a63c67810002.jpg)

  DC Synthesis Flow

  ![day6lab1j](https://user-images.githubusercontent.com/118953917/208399163-243a0652-8480-4e7f-b20b-e83201befc67.jpg)
 
</details>
  
<details>
  <summary>Lab 1: Invoking DC basic setup</summary>
 

### Lab 1: Invoking DC basic setup

>> Invoke DC setup
  
> git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
  
> cd sky130RTLDesignAndSynthesisWorkshop/
  
> /p/hdk/pu_tu/prd/sams/mig76_wlw/setup/enter_p31 -cfg ip76p31r08hp7rev03 -ov ./



 ![day6lab2a1](https://user-images.githubusercontent.com/118953917/208399987-8d9285f4-2de4-409c-9c63-a33ccfa342f5.jpg)

.lib file location

  ![day6lab2a2](https://user-images.githubusercontent.com/118953917/208400030-9671d0ab-f3c3-4f93-99b7-9a25bb7db368.jpg)

> gvim sky130_fd_sc_hd__tt_025C_1v80.lib
  
> :syn off


  ![day6lab2b](https://user-images.githubusercontent.com/118953917/208400087-79079ea8-cbb5-451b-ae25-dc85146735b1.jpg)
  
> csh
  
> dc_shell


  ![day6lab2c1](https://user-images.githubusercontent.com/118953917/208400490-1c52f66b-b17e-48a0-84f3-9dc717f10abc.jpg)

> echo $target_library
  
> echo $link_library


  ![day6lab2c2](https://user-images.githubusercontent.com/118953917/208400514-4699ec22-4aa8-409f-bf92-2020511f68fb.jpg)

>> exit dc_shell and open gvim
  
> gvim DC_WORKSHOP/verilog_files/lab1_flop_with_en.v


  ![day6lab2d](https://user-images.githubusercontent.com/118953917/208400571-ee4bc5a8-6833-4551-8764-cb26aa07333d.jpg)
  
>> Invoke dc_shell 
  
> read_verilog DC_WORKSHOP/verilog_files/lab1_flop_with_en.v
  
> write -f verilog -out lab1_net.v
  
> sh gvim lab1_net.v


  ![day6lab2e](https://user-images.githubusercontent.com/118953917/208400606-c8fb67eb-d67c-40ac-98ff-69b48633ab42.jpg)
  
> read_db DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.db
  
> write -f verilog -out lab1_net.v
  
> sh gvim lab1_net.v


  ![day6lab2f](https://user-images.githubusercontent.com/118953917/208400650-8263cd08-2928-49b7-98b4-ca451dad1ccf.jpg)
  
> echo $link_library
  
> echo $target_library
  
> set target_library /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/sky130RTLDesignAndSynthesisWorkshop/DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.db
  
> set link_library {* /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/sky130RTLDesignAndSynthesisWorkshop/DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.db}
  
> link
  
> compile
  
> write -f verilog -out lab1_net.v
  
> sh gvim lab1_net.v
  

  ![day6lab2g](https://user-images.githubusercontent.com/118953917/208400680-3ef83f6d-f0ca-4341-aae5-5b97c96487fe.jpg)

</details>
  
<details>
  <summary>Lab 2: Introduction to DDC GUI with design_vision</summary>
 

### Lab 2: Introduction to DDC GUI with design_vision

>> Invoke GUI format of DC
  
> csh
  
> design_vision


  ![day6lab3a](https://user-images.githubusercontent.com/118953917/208400716-0832952a-1418-497c-98c9-780c8210187a.jpg)
  
> write -f ddc -out lab1.ddc
  

  ![day6lab3b](https://user-images.githubusercontent.com/118953917/208400893-12bee07a-6420-4933-8c74-ad2ff7da256d.jpg)
  
> design_vision > read_ddc lab1.ddc (loaded in one terminal)
  
> design_vision> read_verilog DC_WORKSHOP/verilog_files/lab1_flop_with_en.v (open in another terminal)
  

  ![day6lab3c](https://user-images.githubusercontent.com/118953917/208400932-87d31281-6818-4700-88a5-3a0234332495.jpg)
  
Schematic design output
  

  ![day6lab3d](https://user-images.githubusercontent.com/118953917/208401027-aabec6f2-1273-4534-b92e-a23bbea0089e.jpg)

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


  ![day6lab4a](https://user-images.githubusercontent.com/118953917/208401082-41810d7b-dca9-4271-bbd2-1d4f27924cd7.jpg)
  
>> Preconfigure at home directory and file name is .synopsys_dc.setup
  
> pwd -> to ensure it is at the home directory
  
> gvim .synopsys_dc.setup -> ensure the file name is as stated
  
>> Insert the contents inside gvim
  
> set target_library /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/sky130RTLDesignAndSynthesisWorkshop/DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.db
  
> set link_library {* target_library}
  
>> Save and exit gvim
  
> :wq


  ![day6lab4b](https://user-images.githubusercontent.com/118953917/208401111-3ec2e644-451f-40ef-ba06-6b758b273c94.jpg)
  
>> Open dc_shell and display the output
  
> echo $target_library
  
> echo $link_library
  

  ![day6lab4c](https://user-images.githubusercontent.com/118953917/208401142-da2d1d55-e49c-42d3-9c55-411a36a237b4.jpg)

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
  

  ![day6lab5a](https://user-images.githubusercontent.com/118953917/208401199-4a32e760-56d2-41be-910f-deead81fe976.jpg)

**While loop**

Notes: Example is taken from instructor's video
  

  ![day6lab5b](https://user-images.githubusercontent.com/118953917/208401225-1e213365-e1d4-4e6f-b3e1-69f3ec30f403.jpg)
  
**For loop**

Notes: Example is taken from instructor's video
  

  ![day6lab5c](https://user-images.githubusercontent.com/118953917/208401247-12900240-0773-4f0a-a295-68fcff375432.jpg)

**Foreach in TCL**
  
Notes: Example is taken from instructor's video
  

  ![day6lab5d](https://user-images.githubusercontent.com/118953917/208401275-d94ab6f1-a9d6-451b-8d33-35f2d341cc3b.jpg)
  
**DC specific**
  
Notes: Example is taken from instructor's video
  

  ![day6lab5e](https://user-images.githubusercontent.com/118953917/208401299-0e0ed95c-59fa-4cc6-be77-454a942cb431.jpg)
  
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
  

  ![day6lab6a](https://user-images.githubusercontent.com/118953917/208401335-5e00a069-7a83-45bb-83f8-80c3ba67c2d2.jpg)
  
**While loop**
  

  ![day6lab6b](https://user-images.githubusercontent.com/118953917/208401355-bde4791c-60fc-4976-aacd-fe66af6c394f.jpg)
  
**Foreach loop**
  

  ![day6lab6c](https://user-images.githubusercontent.com/118953917/208401376-ae485660-2a5e-4e4b-be22-59fcddf41412.jpg)
  
**Foreach in collection**
  

  ![day6lab6d](https://user-images.githubusercontent.com/118953917/208401416-59907eba-3d11-46b1-aca1-46f989054374.jpg)
  

  ![day6lab6e](https://user-images.githubusercontent.com/118953917/208401453-67d6b045-9212-47b3-a8d2-65e6c5303748.jpg)
  
Another way to run the TCL script is by sourcing the gvim file using tcl scripting
> sh gvim myscript.tcl


  ![day6lab6f](https://user-images.githubusercontent.com/118953917/208401489-fb572ed7-8c29-445a-a6fa-82f513660a3d.jpg)
  
</details>
