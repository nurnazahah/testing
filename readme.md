## Day-17

### Topic: Design and characterise one library cell using Layout tool and spice simulator

### Labs for CMOS inverter ngspice simulations
<details>
  <summary>Lab 1:  IO placer revision</summary>
 
### Continue previous lab session (Day 16)
  
> In terminal
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/configuration
vim README.md
vim floorplan.tcl
```
> In openLANE
```
set ::env(FP_IO_MODE) 2
run_floorplan
```
  
![image](https://user-images.githubusercontent.com/118953917/213461720-580d1387-3c86-4491-ab15-2eb11e06c7a3.png)
  
```
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-01_14-09/results/floorplan
magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
  
![image](https://user-images.githubusercontent.com/118953917/213464391-330846dc-721c-4027-97c9-3e3632d4b345.png)

</details>

<details>
  <summary>Theory 1:  SPICE deck creation for CMOS inverter</summary>
 
### SPICE deck creation for CMOS inverter
  
**What is SPICE deck?**
  
* Component connectivity
* Defining component values
* Identifying the nodes
* Naming the nodes
  
![image](https://user-images.githubusercontent.com/118953917/213471549-629a7714-ffd7-4440-b422-43a05f5b131a.png)
  
**Writing SPICE deck**
  
![image](https://user-images.githubusercontent.com/118953917/213471621-710c2e2b-e994-43b0-a095-4547f54bfc17.png)

</details>

<details>
  <summary>Theory 2:  SPICE simulation lab for CMOS inverter</summary>
 
### SPICE simulation lab for CMOS inverter
  
![image](https://user-images.githubusercontent.com/118953917/213475389-6c5d85a3-e150-4348-9bd8-9a51b57c3208.png)

![image](https://user-images.githubusercontent.com/118953917/213478263-a4d6c4eb-8289-426e-a046-653c4c728d08.png)
  
![image](https://user-images.githubusercontent.com/118953917/213477736-bb9326c4-96f7-403d-be5c-df0412e91974.png)

**Overview of the model file**
  
![image](https://user-images.githubusercontent.com/118953917/213483508-dfe6602e-9d46-4b16-964b-2d06064a9b6e.png)

**Steps in executing the circuit**
  
1. Change directory to the desired path, source the circuit, and execute the circuit
2. Set the plot and choose plot that is currently presenting in the design (dc1)
3. Display the vectors 
4. Plot the graph and observe the waveform (out vs in)

![image](https://user-images.githubusercontent.com/118953917/213486523-2280c11c-2426-4e06-ac90-303f6b833351.png)

**Output waveform obtained**
  
* Voltage transfer characteristic
* The waveform is kind of shifted towards the left 
  
![image](https://user-images.githubusercontent.com/118953917/213487543-8db2eebc-b3a9-4e46-bd25-683595dcbc7b.png)

**Shifting the value of nmos width and ratio of pmos transistor**
  
* The width of pmos has increased for about 2.5 times
  
![image](https://user-images.githubusercontent.com/118953917/213490254-cf4ee7d1-996f-466c-abe6-0feab9f451f1.png)

**Generated output waveform**
  
* The output is now has been centred
  
![image](https://user-images.githubusercontent.com/118953917/213490384-71909e03-0399-45d7-833e-10d79eb7f7e1.png)

</details>

<details>
  <summary>Theory 3:  Switching Threshold Vm</summary>
 
### Switching Threshold Vm
  
**Comparison of the output waveforms with switching voltages**
  
**Static behaviour evaluation**
  
* CMOS is a robust design, where the shape of both waveforms are the same eventhough they have different sizes
* Referring to the output waveform shown below, the graph on the RHS is not shifted when the voltage shifting is applied due to the pmos width which is larger than the design on the LHS where the widths of nmos and pmos are the same
* This happened is because the cmos circuit constructed is more robust
* One of the parameters that defines the cmos inverter robustness is the switching threshold, Vm, which is the voltage when Vin = Vout
  
![image](https://user-images.githubusercontent.com/118953917/213602461-40136fc9-f951-45a8-8164-9107a541aeaa.png)

* At interception, pmos and nmos are in saturation region (power ON)
* There is high possibility of having leakage current (power to ground)
* When the vin = vout, gate voltage = drain voltage, so the vgs will be very little compared to the threshold voltage
* At the point where vgs = vds, the currens idsn and idsp will be the same, but it's just flowing in the opposite directions

![image](https://user-images.githubusercontent.com/118953917/213603451-2c206e94-81c4-43c9-a4f7-e5a759c4b7c5.png)

</details>

<details>
  <summary>Theory 4:  Static and dynamic simulation of CMOS inverter</summary>
 
### Static and dynamic simulation of CMOS inverter
  
* Dynamic simulations usually being used to analyse the rise and fall delay as well as verifying how it varies with the switching threshold
* To construct a dynamic simulation, pulse wave is inserted as an input and the simulation command will be a transient analysis
  
![image](https://user-images.githubusercontent.com/118953917/213609513-3b328a27-183d-4aaa-afec-2b2d73905252.png)

**Setting the pulse wave and transient analysis**
  
![image](https://user-images.githubusercontent.com/118953917/213609592-d3faea15-b0cf-4317-890f-e1a7a6088700.png)

* The rise delay calculation is based on the 50% point, the difference in time between when the falling input crosses 50% and the point where rising output crosses 50%
* Vice versa for the fall delay calculation
  
![image](https://user-images.githubusercontent.com/118953917/213609830-9eeebf73-6172-4e3c-9fcd-487897930636.png)

![image](https://user-images.githubusercontent.com/118953917/213609852-06ebcda9-f143-4e29-a6bb-63a537b15e19.png)

![image](https://user-images.githubusercontent.com/118953917/213609871-28bd45bc-ba18-497b-afdc-6da319163b01.png)

</details>

<details>
  <summary>Lab 2:  Lab steps to git clone vsdstdcelldesign</summary>
 
### Lab steps to git clone vsdstdcelldesign

```
cd ~/Desktop/work/tools/openlane_working_dir/openlane
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
cd vsdstdcelldesign/
```
  
```
cd ~/Desktop/work/tools/openlane_working_dir$ cd pdks/sky130A/libs.tech/magic/
cp sky130A.tech /home/nur.nazahah.mohd.amri/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
magic -T sky130A.tech sky130_inv.mag
```
  
![image](https://user-images.githubusercontent.com/118953917/213611879-6ee86bc8-385a-47aa-9dd3-c964043f2bcc.png)

</details>

### Inception of Layout & CMOS fabrication process
<details>
  <summary>Theory 1:  Create Active regions</summary>
 
### Create Active regions
  
**16-mask CMOS process**
  
1. Selecting a substrate
2. Creating active region for transistors
3. N-well and P-well formation
4. Formation of gate
5. Lightly doped drain (LDD) formation
6. Source and drain formation
7. Local interconnect formation
8. Higher level metal formation
  
**Selecting a substrate**
  
* Doping: the process of adding impurities to intrinsic semiconductors to alter their properties
* Common type of substrate is P-type substrate and it is always be used in smartphone, and etc.
* The doping level is used to fabricate nmos and pmos as well as it will be maintaining low 
  
![image](https://user-images.githubusercontent.com/118953917/213614279-acf4cd99-123b-45de-9b82-55ee21ebe551.png)

**Creating active region for transistors**
  
* Creating active region of transistors by creating pockets
  
1. Create isolation between each pocket in insulator (by growing the silicon dioxide)
2. Add in/deposit a layer of silicon nitrate onto a silicon dioxide
3. Define the region to create the pocket by depositing a layer of photoresist (film to do the process that will define all the regions)
4. Photoresist process will create a mask in fabrication and masking process will take place, where photoresist process will protect the layout from the chemical reaction if UV light is disposed
5. The exposed region which is not getting protected from the photoresist will be washed out 
  
![image](https://user-images.githubusercontent.com/118953917/213616666-7260dcbe-2125-4fde-9e3a-cb17adb0a3d2.png)

6. Mask is then removed and if there is chemical reaction or any kind of deposition/etching process, only the exposed region will be affected
7. Silicon nitrate will be etched, only the area of underneath the protection layer will be protected while the remaining area will be etched off 

![image](https://user-images.githubusercontent.com/118953917/213619651-496b1f88-1ee2-4bd9-982c-325342d002e0.png)
  
8. Photoresist will be removed since the silicon nitrate will act as a protection layer to grow the oxides of the other area
9. Place the cell into a completely oxidation furnace (containing a very high temperature) to grow the oxides of the other layers 
* From the figure, the protection layer protects the underneath region to grow while it helps the exposed region to grow in the oxidation furnace
10. The exposed region has grown while the protected layer remain the same
  * Grown process is called LOCOS (Local Oxidation of Silicon)
  * Grown area is called as Bird's beak
  
![image](https://user-images.githubusercontent.com/118953917/213619681-a1b89a85-85c9-4b06-b48a-c307d3346fc5.png)
  
11. Etched out/remove the Silicon nitrate using phosphoric acid
  * This is how we get the isolation region and the ways in protecting those two different transistors from communicating to each other 
  
![image](https://user-images.githubusercontent.com/118953917/213619715-bfedb329-5afb-4527-a0c0-092872825bb4.png)

</details>

<details>
  <summary>Theory 2:  Formation of N-well and P-well</summary>
 
### Formation of N-well and P-well
  
**N-well and P-well formation steps**
  
* N-well will be used for pmos application while P-well will be used for nmos application
* We need to protect one area while fabricating the other area
  
1. Deposit a layer of photoresist and define which layer should we protect
  * Use Mask2 to protect the desired area while we fabricating the other area
  
![image](https://user-images.githubusercontent.com/118953917/213631953-0a866e31-c03b-4a0d-bc9c-ceb91742ef31.png)
  
**Overview of Mask 2 (Top view)**
  
![image](https://user-images.githubusercontent.com/118953917/213631914-97ed5958-4bc0-4393-8a21-a5a53b76d286.png)

2. The photoresist will be exposed by the UV light, in which only the exposed area will be reacted by the UV light while the protected area would remain the same and did not react to the UV light
3. The exposed area will then be washed out from the substrate
  
![image](https://user-images.githubusercontent.com/118953917/213633496-b1bfc5ca-1ea7-443d-a8b8-33cd3e4bf1cd.png)

4. The mask is then be removed and create a p-well by adding Boron (p-type material) and it will be diffused in the substrate through the oxide layer
  * This process is using ion implantation process with a very high energy of around 200keV
  * High energy is needed, so that Boron can pass through the substrate
  * This will produce P-well 
  
![image](https://user-images.githubusercontent.com/118953917/213635240-c5ae2dde-75ba-42c0-add6-ea60e8b7ea73.png)

* The formation of N-well will undergoing the same process as P-well, but N-well uses Phosphorus (n-type material) to implant/diffuse inside the substrate with around 400keV
* Energy used in Phosphorus is quite high since Phosphorus is heavier than Boron
* After all the steps are done, N-well and P-well have been formed

![image](https://user-images.githubusercontent.com/118953917/213637152-6a4cb8fc-217f-4b0f-8234-5e5d442c3775.png)

5. Once the complete N-well and P-well have been formed, we need to diffuse the well so that it will occupies at least the half of the substrate area. Therefore, we have a clear room available for the pmos and nmos fabrication.
6. Place the complete substrate with N-well and P-well into a high temperature furnace (Driving furnace) for about 100°C for 4-6 hours
  * That will form a drive in/diffuse Boron and Phosphorus into the substrate, forming a clear well
  * N-well creates pmos transistor while P-well creates nmos transistor
  
* Explaination above refers to the ways on creating the pockets as mentioned in the previous session
  
![image](https://user-images.githubusercontent.com/118953917/213650065-57864b9b-ac52-4849-bc9c-83bc6da3f789.png)

</details>

<details>
  <summary>Theory 3:  Formation of gate terminal</summary>
 
### Formation of gate terminal
  
* Formation of gate terminal is quite important since it will control the threshold voltage as well as turning on voltage for transistors

**Quick refreshing on Circuit design and SPICE simulation**
  
* Fabrication is closely related to threshold voltage in which threshold voltage would decides the internal voltage of the gate, as well as deciding the functions of the gate
  
![image](https://user-images.githubusercontent.com/118953917/213654491-7b5181cb-ffd6-47d8-ab71-d1c5ba2d75d4.png)

**Formation of gate terminal steps**
  
1. Control and maintain the doping concentrations of the substrate by adding Mask4 
2. The rest of the process is still the same as discussed before. Hence, it will be repeated steps for the rest of the process.
  * Doping process using Boron with low energy required which is only around 60eV where it is not high enough since we only need the surface 
  * Similar process needs to be done fro pmos too
  
![image](https://user-images.githubusercontent.com/118953917/213704270-986aa838-8a7c-4d58-929d-ba8efa764a1d.png)

3. Add Mask5 on P-well and repeat the same steps for pmos. We also need to control the threshold voltage of pmos and undergo the rest of the process for pmos
4. Arsenic/Phosphorus can be used in diffusing into the substrate with maintaining the low energy 
  
![image](https://user-images.githubusercontent.com/118953917/213706127-010c6cc9-687d-4e86-bbea-1092aea41b19.png)

5. Fix the oxide since there is a lot of damages from implantation process. Fixing process can be undergo by removing the oxide using hydrofluoric (HF) acid
  * Once we fixed that, the oxide will re-grown and giving a high quality oxide to the substrate
6. Deposit a polysilicon layer using deposition technique and it will grow a thick polysilicon where the gate area is supposed to have a low resistance
7. Since the gate has a low resistance, we need to dope it with some more impurities i.e. Phosphorus & Arsenic and diffuse it on a complete polysilicon layer to have a low sheet resistance, as well as low gate resistance
8. Use Mask6 as a gate of polysilicon mask 
  
![image](https://user-images.githubusercontent.com/118953917/213713052-5491a8d7-9385-4fda-b7ff-780df015fcef.png)
  
**Top view of Mask6**
  
![image](https://user-images.githubusercontent.com/118953917/213713554-091b184e-3a83-4c7a-86e7-7db2c7af790d.png)

9. Remove Mask6 and the remaining areas of the polysilicon which is outside of the photoresist can be etched out of it
10. Polysilicon gate is now formed
  
![image](https://user-images.githubusercontent.com/118953917/213714902-42fc4015-c534-436e-a4be-dcc2b189f679.png)
  
11. Remove the photoresist and gate has been formed
  
![image](https://user-images.githubusercontent.com/118953917/213715020-4cda3c87-8341-42dd-be25-35c9b79c5db4.png)

</details>

<details>
  <summary>Theory 4:  Lightly doped drain (LDD) formation</summary>
 
### Lightly doped drain (LDD) formation
  
**Why theere are two dopping profiles?**

* Hot electron effect
  + Electric field, E=V/d
  + High energy carriers will break Si-Si bonds (more electron and hole -> cannot control doping profile well)
  + Crosses 3.2eV barrier between Si conduction band SiO2 conduction band, it will enter oxide layer and may causing a liberty issue
  
* Short channel effect
  + For short channels, drain field will penetrates channel (making the gate difficults to control the current- source and drain)
  
**Ways to generate LDD structure**
  
1. Based on the created gate from the previous session, the step started by adding Mask7 to protect the desired area and the rest steps of photolithography is remain the same 
2. However, to fabricate the nmos at P-well, we need to do the ion implantation process by doping it with Phosphorus (N-type material) 
  * N- -> lightly doped channel is formed
  
![image](https://user-images.githubusercontent.com/118953917/213725083-cd59e33f-ebce-4b90-9e1c-6fdc30af7adf.png)

3. Creating Mask8 and repeat the rest of the steps 
4. Ion implantation takes place in the next process by doping Boron 
5. Create some spaces around gate to protect further source and drain formation
  
![image](https://user-images.githubusercontent.com/118953917/213726675-d2af85ef-22c8-4712-b01f-2768556b914a.png)

6. Undergo plasma anisotropic etching 
  * The etching will create a side-wall spacers (green colour at both LHS & RHS of the gate in the figure)

![image](https://user-images.githubusercontent.com/118953917/213728268-5c93239c-ead4-42b3-83f9-68139b844841.png)

</details> 

<details>
  <summary>Theory 5:  Source and drain formation</summary>
 
### Source and drain formation

1. Add a thin layer of screen oxide to avoid channel link entering the substrate 
2. Add Mask9 and repeat the same steps of photolithography 
3. Expose the structure with Arsenic with 75keV amount of energy 
  * It will obtain N+ N- P
  
![image](https://user-images.githubusercontent.com/118953917/213731093-b1c96fd3-34dd-4301-b7bc-294b720cb732.png)

* Do the same way as pmos as well
4. Add Mask10 and do the process og photolithography 
5. Do ion implantation process using Boron with 50oeV of energy 
  * It will obtain P+ P- N
6. Place into high temperature furnace to penetrate more into N-well and P-well and the process will do the high temperature annealing
  * Source for pmos and nmos are now obtained

![image](https://user-images.githubusercontent.com/118953917/213733511-9497f7ad-9979-480b-9962-8ffaab50609c.png)

</details>

<details>
  <summary>Theory 6: Local interconnect formation</summary>
 
###  Local interconnect formation
  

