## Day-13

### Topic: Post-synthesis simulation

<details>
  <summary>Theory</summary>
 
### Theory

**Things to do in pre-synthesis**

Modelling and simulating IP cores in VSDBabySoC (for checking its functionality) 
  
**Refreshing on synthesizable and non-synthesizable constructs in verilog**
  
![image](https://user-images.githubusercontent.com/118953917/210950834-5d98bf62-278a-4bb3-a5c3-4d84781364d0.png)
  
**Why do pre-synthesis? Why cannot just do post-synthesis?**
  
* Pre-synthesis simulation is done according to the logic that we have designed for and written -> only functionality
* Post synthesis simulation/GLS (gate level simulation) is done after synthesis considering each gate delays into account, also reports the violations in both functionality and timing
* This will show the mismatches that are likely to get due to the wrong usage of operators and inference of latches
* i.e. using ‘X’ (simulator/synthesizer terms) as ‘Unknown’/‘Don’t care’
  
**Gate Level Simulation (GLS)**
  
* 'gate level' refers to netlist view of a circuit, usually produced by logic synthesis
* RTL simulation is pre-synthesis while GLS is post-synthesis
* Netlist view is a complete connection list consisting of gates and IP models with full functional and timing behavior
* RTL simulation is zero delay environment and events, generally occurs on the active clock edge. Whereas GLS can be zero delay too, but it is more often used in unit delay or full timing mode
  
*Note: Gate level simulation is used to boost the confidence regarding implementation of a design and can help verify dynamic circuit behaviour, which cannot be verified accurately by static methods. It is a significant step in the verification process.*
  
**Tools used to synthesize the netlist**
  
* Using synopsys’s DC shell (Design Compiler)
* DC RTL synthesis solution enables users to meet today's design challenges with concurrent optimization of timing, area, power and test
  
**Why post-synthesis simulation is needed?**
  
* To ensure each and every gate delays are taken into account
* Pre-synthesis in VCS and DVE that has done from the previous labs would be compared 
* To observe the output generated
* Note that post-simulation output should be matched with pre-simulation output

</details>

<details>
  <summary>Lab: Convert .lib to .db using lc_shell</summary>
 
### Converting .lib file to .db file

>> Picking one file 
  
**avsddac.lib**

> Change directory to the desired path
```
cd /nfs/site/disks/zsc11_mip_xmphy_0021/users/nazahah/partition/training
```

> Entering cheetah environment and invoking lc_shell
```
set block = ddrphy
setenv block ddrphy
#source /nfs/site/disks/zsc11_mip_xmphy_0002/proj_root_ulc/setup/setup_pv.csh $block
/p/hdk/bin/cth_psetup -proj ip/2209sp3 -cfg IP78P3H180O12_22WW47 -cfg_ov /nfs/site/disks/ipg_da_00001/da/cth_overrides/sde_2209sp3_converged.cth -ward $PWD -verbose -x '$SETUP_R2G -force'
  
lc_shell
```

> Read and write .lib files
```
cd /nfs/site/disks/zsc11_mip_xmphy_0021/users/nazahah/partition/training/sky130RTLDesignAndSynthesisWorkshop/lib
read_lib avsddac_ori.lib                                                                                  (Error reading .lib file -> need to do modification)
```

**avsddac.lib error debugging**
  
![image](https://user-images.githubusercontent.com/118953917/211332488-46a6a719-600d-46bb-b5a8-9d38105b4c57.png)

```
cd /nfs/site/disks/zsc11_mip_xmphy_0021/users/nazahah/partition/training/sky130RTLDesignAndSynthesisWorkshop/lib    (Modify the .lib file)
read_lib avsddac.lib        
write_lib avsddac -format db -output avsddac.db
exit                                                                                                                (Exit lc_shell)     
tkdiff avsddac_ori.lib avsddac.lib                                                                                  (Looking the differences for before and after modification) 
```
  
**Modification of avsddac.lib**
  
![image](https://user-images.githubusercontent.com/118953917/211332631-8b55d664-693d-469f-bec2-dd93b687507e.png)
  
> Moving converted .db file from cheetah environment (santa clara zsc11) to png site 
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files                 (Penang site)
rsync -rv rsync.zsc11.intel.com:/nfs/site/disks/zsc11_mip_xmphy_0021/users/nazahah/partition/training/sky130RTLDesignAndSynthesisWorkshop/lib/avsddac.db .    ("." stands for current directory)
```

**avsdpll.lib**
  
```
/nfs/site/disks/zsc11_mip_xmphy_0021/users/nazahah/partition/training/VSDBabySoC/src/lib
lc_shell
read_lib avsdpll.lib          
```
  
**avsdpll.lib error debugging**
  
![image](https://user-images.githubusercontent.com/118953917/211332719-f4e6fa28-f94e-4307-812d-e1daae9472c9.png)
  
**Modification of avsdpll.lib**
  
![image](https://user-images.githubusercontent.com/118953917/211332887-2b569f6f-b19a-4c8a-900f-e1e593be1ce7.png)
  
```
read_lib avsdpll.lib 
write_lib avsdpll -format db -output avsdpll.db
exit    
tkdiff avsdpll_ori.lib avsdpll.lib 
```
  
> Move the converted .db files to the training path (png site)
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files                 (Penang site)
rsync -rv rsync.zsc11.intel.com:/nfs/site/disks/zsc11_mip_xmphy_0021/users/nazahah/partition/training/VSDBabySoC/src/lib/avsdpll.db .  
```
</details>

<details>
  <summary>Lab: Synthesis</summary>
 
### Synthesizing the code
  
**using avsddac.db**
  
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/sky130RTLDesignAndSynthesisWorkshop
/p/hdk/pu_tu/prd/sams/mig76_wlw/setup/enter_p31 -cfg ip76p31r08hp7rev03 -ov ./
csh
dc_shell
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/dac/rvmyth_avsddac_interface/iverilog/Pre-synthesis

sh gvim .synopsys_dc.setup
set target_library {/nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/sky130_fd_sc_hd__tt_025C_1v80.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsddac.db}
set link_library {* nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/sky130_fd_sc_hd__tt_025C_1v80.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsddac.db}
```
  
**rvmyth_avsddac.v**
  
```
read_verilog rvmyth_avsddac.v
```

**rvmyth_avsddac.v Error debugging**
  
![image](https://user-images.githubusercontent.com/118953917/211706699-6cf5e5b5-fdf0-4afc-96c1-46c63a2bfeba.png)
  
**Modification of rvmyth_avsddac.v**
  
![image](https://user-images.githubusercontent.com/118953917/211706748-ff43e106-22da-4235-9131-28da491d21c1.png)
  
```
echo $target_library
echo $link_library
read_file {mod_rvmyth_avsddac.v mod_avsddac.v mod_mythcore_test.v clk_gate.v} -autoread -format verilog -top rvmyth_avsddac 
compile
write -f verilog -out mod_rvmyth_avsddac_net.v
```

![image](https://user-images.githubusercontent.com/118953917/211706800-c6baa5f9-1108-4c87-a4df-759d4f9f6704.png)
  
**Netlist of rvmyth_avsddac**
  
![image](https://user-images.githubusercontent.com/118953917/211706853-85f2f560-8021-4f5e-94f8-0d337bc430a9.png)
  

> Checking the design 
```
current_design
check_design
check_timing
report_constraint
report_timing
```
  
![image](https://user-images.githubusercontent.com/118953917/211706893-82f070cd-87ad-4179-9672-f925523f76df.png)
  

**using avsdpll.db**
  
```
sh gvim .synopsys_dc.setup
set target_library {/nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/sky130_fd_sc_hd__tt_025C_1v80.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsdpll.db}
set link_library {* nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/sky130_fd_sc_hd__tt_025C_1v80.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsdpll.db}
```

**avsd_pll_lv8.v**
  
```
echo $target_library
echo $link_library
read_verilog rvmyth_avsdpll.v
``` 

**avsd_pll_lv8.v Error debugging**

![image](https://user-images.githubusercontent.com/118953917/211706945-3c9a2fc7-8879-4996-b2d6-c9dff6286477.png)
  
**Modification of avsd_pll_lv8.v**
  
![image](https://user-images.githubusercontent.com/118953917/211707005-4cf034bd-75da-414e-8b76-446435867f5b.png)
  
```
echo $target_library
echo $link_library
read_file {mod_avsd_pll_1v8.v mod_mythcore_test.v clk_gate.v mod_rvmyth_pll.v } -autoread -format verilog -top rvmyth_pll_interface
compile
write -f verilog -out avsd_pll_1v8_net.v
```

![image](https://user-images.githubusercontent.com/118953917/211707052-5eea3774-75f6-4f1a-9a59-2e0e7d6d1c33.png)
  
**Netlist of avsd_pll_lv8.v**
  
![image](https://user-images.githubusercontent.com/118953917/211707102-80af24c0-0e64-4ad3-b929-781e3ccb2e43.png)
  
  
  
### VSDBabySoC
  
```
sh gvim .synopsys_dc.setup
set target_library {/nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/sky130_fd_sc_hd__tt_025C_1v80.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsdpll.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsddac.db}
set link_library {* nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/sky130_fd_sc_hd__tt_025C_1v80.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsdpll.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsddac.db}
```
  
```
echo $target_library
echo $link_library
read_file {mod_mythcore_test.v mod_avsd_pll_1v8.v mod_avsddac.v clk_gate.v mod_vsdbabysoc.v} -autoread -format verilog -top vsdbabysoc
compile
write -f verilog -out vsdbabysoc_net1.v
``` 
  
![image](https://user-images.githubusercontent.com/118953917/211965920-d89fad34-d839-436e-b114-35a916b7c300.png)

**Netlist of VSDBabySoC**
  
![image](https://user-images.githubusercontent.com/118953917/211966018-18e67c13-b0ba-48db-9f4c-dce2e935e507.png)
  
</details>

<details>
  <summary>Lab: Post-synthesize and comparisons</summary>
 
### Post-synthesizing simulations 
  
**rvmyth_avsddac.v**
  
```
vcs mod_rvmyth_avsddac_net.v rvmyth_avsddac_TB.v
  
```
</details> 

## Day-14

### Topic: Synopsys DC and Timing Analysis

<details>
  <summary>Theory</summary>
 
### Recap 
  
**What is synthesis?**
  
* RTL to gate level translation
* The design is converted into gates and the connections are made between the gates
* This is given out as a file called netlist
  
**What is .lib?**
  
* Collection of logical modules
* Includes basic logic gates like AND, OR, NOT, etc.
* Different flavors of the same gate
  
**Why different flavours of gate is needed?**
  
* Combinational delay in logic path determines the maximum speed of operation of digital logic circuit
* So, we need cells that work faster to make Tcombi small
  
**What to achieve in logic synthesis?**
  
Working digital logic circuit is:

* Logically correct
* Electrically correct
* Timing met
  
*Note: To give more delay to the circuit to meet setup/hold time, we need to add buffers. However, additional buffers to meet hold, will add additional area.*
  
**How to decide for the correct implementation of the design?**
  
By using constraints
  + Constraints are the guide to the synthesizer to pick the correct library cells which is most appropriate for the design
  + The illustrations are picked based on the needs
  
**What is CTS?**
  
* Clock Tree Synthesis: a technique for distributing the clock equally among all sequential parts of VLSI design
* Purpose: to reduce skew and delay. CTS provide the placement data as well as the clock tree limitations as input.
  
*Source: https://www.google.com/search?q=cts+in+vlsi&rlz=1C1GCEA_enMY1023MY1023&oq=cts+in+vlsi&aqs=chrome..69i57j0i512j0i22i30l8.2308j0j7&sourceid=chrome&ie=UTF-8*
  
**PVT stands for?**
  
* Process, Voltage, Temperature
  
**Corners of PVT**
  
* Integrated circuits are designed in such a way that they can function in a wide variety of temperatures and voltages, rather than a single temperature and voltage
* In order to make our chip to work after fabrication in all possible conditions, we simulate it at different corners of process, voltage, and temperature
* These conditions are called corners. All these three parameters directly affect the cell delay

**Understanding PVT**
  
* **Process** -> there are millions of transistors on a single chip as we are going to lower nodes and all the transistors in a chip cannot have the same properties. Process variation is the deviation in parameters of the transistor during the fabrication
* **Voltage** -> as we are going to lower nodes, the supply voltage for a chip is also going to less. If the chip is operating at 1.2V, there are chances that at certain instances of time this voltage may vary
* **Temperature** -> when a chip is operating, the temperature can vary throughout the chip. This is due to the power dissipation in the MOS-transistors
  
**PVT Graphs**
  
*Source: https://onedrive.live.com/?authkey=%21APiDwU9SDyOxl5w&cid=E0E9B5EEF85B162E&id=E0E9B5EEF85B162E%2199590&parId=E0E9B5EEF85B162E%2197363&o=OneUp*
  
![image](https://user-images.githubusercontent.com/118953917/211721237-90fa20cc-d0ce-4e88-bf63-46a673c4e1ed.png)

  
</details> 

<details>
  <summary>Task and labs</summary>
 
### Task
  
**Steps to read PVT corners of each timing libs provided**
 
### Converting .lib to .db using lc_shell
  
```
cd /nfs/site/disks/zsc11_mip_xmphy_0021/users/nazahah/partition/training
git clone https://github.com/Geetima2021/vsdpcvrd.git
/nfs/site/disks/zsc11_mip_xmphy_0021/users/nazahah/partition/training/vsdpcvrd/resources/timing_libs
lc_shell
read_lib sky130_fd_sc_hd__ff_100C_1v65.lib > 100C_1v65.rpt                         (Error reading lib file -> remove the line in lib with those errors)
```
  
**Error debugging -> grepping errors from .rpt file**
  
![image](https://user-images.githubusercontent.com/118953917/211825198-f8a621b1-d89e-49c7-9ddb-ca2610a95d4b.png)

**Modification of .lib file**
  
![image](https://user-images.githubusercontent.com/118953917/211825404-41ac67e1-fd19-48be-8488-3c676300ca5b.png)

```
read_lib sky130_fd_sc_hd__ff_100C_1v65.lib
write_lib sky130_fd_sc_hd__ff_100C_1v65 -format db -output sky130_fd_sc_hd__ff_100C_1v65.db
```
  
![image](https://user-images.githubusercontent.com/118953917/211831483-dcf4b51a-3a58-4ada-a452-7ad59b264736.png)

> Moving converted .db file from cheetah environment (santa clara zsc11) to png site 
```
/nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d14/timing_libs                (Penang site)
rsync -rv rsync.zsc11.intel.com:/nfs/site/disks/zsc11_mip_xmphy_0021/users/nazahah/partition/training/vsdpcvrd/resources/timing_libs/sky130_fd_sc_hd__ff_100C_1v65.db .    ("." stands for current directory)
```
  
### Synthesizing and optimizing
  
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d14/timing_libs
sh gvim cons.sdc
```
  
**Setting the constraints**
  
gambar 6
  
```
sh gvim .synopsys_dc.setup
 
set target_library {/nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d14/timing_libs/sky130_fd_sc_hd__ff_100C_1v65.db}
set link_library {* /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d14/timing_libs/sky130_fd_sc_hd__ff_100C_1v65.db} 
```
  
![image](https://user-images.githubusercontent.com/118953917/211971204-3eb09759-db4b-4f5e-9594-f0179be48d4e.png)


```
dc_shell
echo $target_library
echo $link_library
read_file {mod_mythcore_test.v mod_avsd_pll_1v8.v mod_avsddac.v clk_gate.v mod_vsdbabysoc.v} -autoread -format verilog -top vsdbabysoc
link
read_sdc cons.sdc
compile_ultra
report_qor
```

