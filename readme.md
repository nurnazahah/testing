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
  
**Selecting a substrate**
  
* Doping: the process of adding impurities to intrinsic semiconductors to alter their properties
