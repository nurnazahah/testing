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
  
> rvmyth_avsddac.v
```
cd /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/sky130RTLDesignAndSynthesisWorkshop
/p/hdk/pu_tu/prd/sams/mig76_wlw/setup/enter_p31 -cfg ip76p31r08hp7rev03 -ov ./
csh
