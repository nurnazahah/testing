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
  <summary>Lab</summary>
 
### Task

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

### Synthesizing the code
  
**avsddac.lib**
  
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/sky130RTLDesignAndSynthesisWorkshop
/p/hdk/pu_tu/prd/sams/mig76_wlw/setup/enter_p31 -cfg ip76p31r08hp7rev03 -ov ./
csh
dc_shell
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/dac/rvmyth_avsddac_interface/iverilog/Pre-synthesis

sh gvim .synopsys_dc.setup
set target_library {/nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/sky130_fd_sc_hd__tt_025C_1v80.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsddac.db}
set link_library {* nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/sky130_fd_sc_hd__tt_025C_1v80.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsddac.db}

read_verilog avsddac.v
write -f verilog -out avsddac_net.v
read_db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsddac.db
compile
write -f verilog -out avsddac_net.v
```

**avsddac.v error debugging**
  
![image](https://user-images.githubusercontent.com/118953917/211333113-9aea82b7-6a63-470b-8de2-59f51b8168c5.png)
  
**Modification of avsddac.v**
  
![image](https://user-images.githubusercontent.com/118953917/211333191-3acccfef-1795-43ff-8637-7f0eb7c146b3.png)
  
![image](https://user-images.githubusercontent.com/118953917/211333284-76799ede-283a-40c9-814e-5a1aef1456c2.png)
  
**avsddac_net.v**
 
![image](https://user-images.githubusercontent.com/118953917/211334130-9929b441-7e68-4842-bb0a-fbd22e5c326d.png)
  
> DAC modelling
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files
vcs avsddac_net.v avsddac_tb_test.v
```
  
**avsddac_net.v & avsddac_tb_test.v error debugging**
  
![image](https://user-images.githubusercontent.com/118953917/211337707-fb49726a-eecc-496e-90ec-af8083e38070.png)


  
  
  
  
  
  
  
  
  
  
**avsdpll.lib**
  
```
/nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/

sh gvim .synopsys_dc.setup
set target_library {/nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/sky130_fd_sc_hd__tt_025C_1v80.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsdpll.db}
set link_library {* nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/sky130_fd_sc_hd__tt_025C_1v80.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsdpll.db}
  
read_verilog avsdpll.v
write -f verilog -out avsdpll_net.v
read_db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/db_files/avsdpll.db
compile
write -f verilog -out avsdpll_net.v
```
  
**avsdpll.v error debugging**
  
![image](https://user-images.githubusercontent.com/118953917/211333391-eba38513-9600-4d2c-8e76-e0e56c8d6b32.png)
  
**Modification of avsdpll.v**

