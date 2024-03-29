## Day-27

### Topic: Introduction to crosstalk - glitch and delta delay

### Introduction to Crosstalk
<details>
  <summary>Introduction</summary>
  
### Introduction to crosstalk
  
**What happens when we go through a chip design cycle?**

* When we go through a design, there are three things that we try to achieve on a chip.
  + Power: focusing on the lowest power consumption.
  + Performance: focusing on the performance, process and speed of the device.
  + Area: preferable a smaller device

### What will be covered?
  
* Reasons for crosstalk
* Introduction to noise margin
* Crosstalk glitch example
* Factors affecting glitch height
* AC noise margin
* Timing window concepts
* Impact of crosstalk on setup and hold timing
* Techniques to reduce crosstalk
* Power supply noise
  
  
</details>


<details>
  <summary>High Routing Density</summary>
  
### Crosstalk Noise Reasons and Definition
  
**High routing density and large number of standard cells**
  
* 0.25 um and 0.1 um are the channel/gate length.

* Looking through 0.25 um and above process, there are quite some spaces and routes between each other.
  
* Quick way to reduce the size of the MOSFET is to reduce the channel length. When we reduce the channel length, the overall size of the MOSFET shrinks the overall size of the combinational logic, resulting the cell inside shrinks too. That way, we achieved a smaller size of the MOSFET.
  
* If smaller size has been achieved, resulting the cells inside shrank, the complete circuit accomodates in a smaller area. Therefore, we can have multiple instances of the circuits or similar kind of circuits which are getting made to get back into the area.
  
* For example, the circuit is used for sending and receiving messages. The circuit could have just instantiated in nine times. Some section can be sending and receiving messages, another section can be sending and receiving calls, some can be processing, some can reading other applications and so on. 
  
* As we can see, before reducing the MOSFET size, we only have one or two applications running in the same area, but after reducing the size, now we have nine applications running in the same area of the chip.
  
* However, there is issue in interference when we reduce the size. Basically, referring to 0.1 um and below process in the figure below, there is some amount of interference in their functioning that is happening between the two nets/wires that is being placed very close to each other when we reduce the size. This is the major reason in crosstalk.
  
* Initially, there are 20 number of standard cells. After reducing the size, the number of standard cell has increased 9 times where the standard cell has to be connected to each other and as a result of that, the number of routes has increased and the routing has becomes very close to each other. 
  
* Hence, we will started to see some failures in the design, where there was some functionality failure is happening which we can called it as crosstalk.
  
*Source: the figure was taken from lecture video in udemy course (https://www.udemy.com/course/vlsi-academy-crosstalk/learn/lecture/1614424#overview)*
  
![image](https://user-images.githubusercontent.com/118953917/219541836-b80e0fa7-52ce-45a3-898b-161ecf48c703.png)

</details>

<details>
  <summary>Dominant Lateral Capacitance</summary>
  
### Crosstalk Noise Reasons and Definition
  
**Increase in number of metal layers resulting in increase in lateral capacitance**
  
* Basically, there are 2 kinds of capacitance.
  + Interlayer capacitance: capacitors that is placed between 2 consecutive different layers.
  + Lateral capacitance: capacitors that is placed between 2 wires at the same level and metal layer.
  
* The second reason of increasing the crosstalk noise is increase in the lateral capacitance because it is increasing the metal layer.

![image](https://user-images.githubusercontent.com/118953917/219543759-59aad67d-c6f8-4d67-86ee-1182b9c4804c.png)
  
**Why increasing lateral capacitance making metal layer increasing too?**
  
* Breaking into several metal layers helps in reducing the resistance where the higher the area, resulting in lower resistance. That's why we are having a wider metal layer.
  
* The overlap area between metal 1 and metal 2 as shown in the figure below, is pretty huge, that leads into an increase of lower capacitance. That's why 0.25 um and above process, we say that the interlayer capacitance was dominant. 
  
![image](https://user-images.githubusercontent.com/118953917/219565374-c89b697d-bab2-4759-a627-b5930a383fb4.png)

* As we reduced the size of the MOSFET, it will increase the number of standard cells, resulting in increasing the number of connections. So, each cell needs to be connected to its edges of the standard cells, making the connection increased. As a result, the number of routes also got increased.
  
* Since the routes are very close to each other and it is difficult to accommodate the area of the MOSFET, we reduce the widthe of the metal. However, even when we reduce the width of the metal, the demand of routes of the area is too huge. Therefore, reducing it only won't help.
  
* So, we need to do the connections in different way (i.e. referring to the figure below) which is making the signal travelling in a straight line (only travelling across metal 1) without transferring the signal to metal 3 first.  This is happened because of the limited amount of resources/routing resources available in the area. In this case, the amount of area is very compact and we need to accommodate it where we have to connect signals at any cost.
  
![image](https://user-images.githubusercontent.com/118953917/219569277-28781416-165c-4714-bc6d-c17db5d329e4.png)

* Things that have been observed:
  + The width of the metal has reduced
  + The number of metal layers have increased
  
* Referring to the figure bwlow, now the issue regarding overlapping 2 consecutive area has been solved, but now we have issue in overlapping between 2 side walls of the metal layer at the RHS of the figure. So, there is a huge overlap area between these 2 side walls and that's the reason we see lateral capacitance dominant and the biggest disadvantage we find with the lateral capacitance is that they present all the same layer.
  
* Looking through the RHS of the image below, if they are present on the same layer and the signal which is passing through the left side net will immediately being coupled to the other right side net because they're very close to each other. So, any switching activity happening between the same layer will immediately affect the whole process.
  
![image](https://user-images.githubusercontent.com/118953917/219570032-d95aae1c-0f77-487d-83da-dc47820dd052.png)

</details>

<details>
  <summary>Introduction to noise margin</summary>
  
### Crosstalk Noise Reasons and Definition
  
**Lower supply voltage leading to lesser noise margin**
  
* In a basic inverter functioning, if we provide low-level input into an inverter, we will get high-level output and vice versa.
  
* Converting the concept into a graphical method, when Vin = low, Vout = high. whereas, when Vin = high, Vout = low.
  
* The behaviour of an inverter happens when the half of the voltage (Vdd/2), we will see the behavior of switch is happening.
  
* When the input is zero, the output is VDD. Then, we move the input from zero and keep increasing the input towards VDD. As gradually we increase the input voltage, the output voltage will start to decrease. And finally, the output voltage will be completely zero.
  
![image](https://user-images.githubusercontent.com/118953917/219574459-115da6cc-ce09-4f19-8e6f-49b9d4a869c5.png)

* The area of the slope (the difference of the output the input) ideally should be infinite.

![image](https://user-images.githubusercontent.com/118953917/219575275-2612b810-8dbf-46af-b571-a1ceffc52874.png)
  
* Practically, the curve won't be as smooth as in ideally. It might have some slopes since it has some delays due to capacitances and resistances while travelling from VDD to zero voltage. However, it won't be exactly achieve zero voltage due to practical scenarios of nmos and pmos, but for sure it will be somewhere around zero. 
  
* Input low voltage (VIL): the input voltage is from zero to some particular value (VIL), as well as maximum input voltage that will be recognised as a low input logic level.
  
* Output high voltage (VOH): the output voltage is from zero to some particular value (VOH), as well as nominal voltage corresponding to a high logic state.
  
* Input high voltage (VIH): any voltage at the input level which lies above VIH and VDD, the output is expected to be low/VOL.

* Output low voltage (VOL): the output at VIH.

![image](https://user-images.githubusercontent.com/118953917/219579883-a91f6c08-72de-45c6-bbfc-483c7ff29838.png)
  
</details>

<details>
  <summary>Noise Margin Summary</summary>
  
### Noise margin summary
  
* Anything that lies between VOL and VIL will be considered as logic 0.
  
* Any voltage that lies between VIL and VIH will be considered as undefined region.
  
* Undefined region -> the logic can either moved from logic 1 to logic 0 or from the interception point of (b) to logic 0. Undefined region is a danger case.
  
* Whenever the voltage lies between VIH and VOH, it will always being treated as 1V or logic 1.
  
* Therefore, we have to ensure that the voltage didn't enter in undefined region since it cannot be identified whether the voltage might be in logic 1 or not.
  
* That is the problem when we are having a large physical distance from the main power supply to the circuit.
  
* Noise margin defines the input voltage range and the output voltage. Basically it varies the input voltage.
  --> **Noise margin**: Any voltage in between the range of VOH and VIH will be detected as logic 1. It should be put under the inputs/outputs of the circuit.

* Any voltage level in NML range will be detected as logic 0.

* Noise could be easily eliminated or can be ignored at this margin.
  
*Source: Udemy learning website*
  
![image](https://user-images.githubusercontent.com/118953917/219953157-00f6b3c5-2728-4346-8112-c546254744ca.png)  
![image](https://user-images.githubusercontent.com/118953917/219952384-7bce91e1-b507-41d1-8706-82f6d5ea487c.png)
    
* Lower Supply Voltage leading to lesser noise margin.

* When the supply voltage is reduced, the noise margin will also be reduced.

* For example, referring to the figure below, anything below 200 mV on the LHS margin will be considered as low margin while on the RHS, the noise margin will be below 100 mV.
  
![image](https://user-images.githubusercontent.com/118953917/219953486-88c0dbd4-6321-4b8c-85f9-2f88e0b85201.png)

</details> 

### Introduction to Signal Integrity and Glitch
<details>
  <summary>Signal Integrity and Glitch</summary>
  
### Signal Integrity and Crosstalk
  
* Signal Integrity and Crosstalk are the Quality checks of the clock routes.
  
* **Signal integrity**: the ability of an electrical signal to carry information reliably and resist the effects of high-frequency electromagnetic interference from nearby signals.
  
* **Crosstalk**: the undesirable electrical interaction between two or more physically adjacent nets due to capacitive cross-coupling. It is a type of noise signal that corrupts the actual signal while transmission through the communication medium.
  
**Aggressor and Victim Nets**
  
* A net that receives undesirable cross-coupling effects from a nearby net is called a victim net.
  
* A net that causes these effects in a victim net is called an aggressor net.
  
### Crosstalk-Glitch
  
* When one net is switching, and another net is constant then switching signal may cause spikes on other net because of which coupling capacitance (Cc) occurs between two nets, this is called as crosstalk noise.
  
* Types of Glitches --> Rise, Fall, Overshoot, Undershoot
  
![image](https://user-images.githubusercontent.com/118953917/220038938-9c354627-8e3e-454a-8ee6-e855a5eaf6da.png)
  
</details>

<details>
  <summary>Performing analysis and report commands</summary>

### Performing Crosstalk Delay Analysis
  
* Enable PrimeTime SI --> ```set_app_varsi_enable_analysistrue```
  
* Back-annotate the design with cross-coupling capacitance information in a SPEF or GPD file --> ```read_parasitics-keep_capacitive_couplingfile_name.spf```

### Using check_timing
  
> Types to check specific to crosstalk analysis
```
no_driving_cell
ideal_clocks
partial_input_delay
unexpandable_clocks
```
  
### Timing reports
  
```
report_timing
-crosstalk_delta
report_si_bottleneck
report_delay_calculation –crosstalk
report_si_double_switching
report_noise
report_timing -transition_time-crosstalk_delta \ -input_pins-significant_digits 4   (Viewing the Crosstalk Analysis Report)
```
  
### Bottleneck Reports
  
```
report_si_bottleneck
report_bottleneck
delta_delay
delta_delay_ratio
total_victim_delay_bump
delay_bump_per_aggressor
  
report_si_bottleneck-cost_typedelta_delay\-slack_lesser_than 2.0    (To get a list of all the victim nets with a delay violation or within 2.0 time units of a violation, listed in order of delta delay)

report_delay_calculation –crosstalk
size_cell
set_coupling_separation
-include_clock_nets
minimum_active_aggressor

report_si_bottleneck-cost_type delta_delay \ -minimum_active_aggressors 3   (bottleneck command reports nets where three or more active aggressors are affecting the net)
```
  
### Crosstalk Net Delay Calculation
  
```
report_delay_calculation-crosstalk \ -from [get_pinsg1/Z] -to [get_pins g2/A]
```
  
### Reporting Crosstalk Settings
  
> To check crosstalk settings
```
report_si_delay_analysis
report_si_noise_analysis
report_si_aggressor_exclusion
```
  
</details>

### Lab
<details>
  <summary>Lab</summary>
  
### Lab
  
> In icc2_shell
```
source /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/top.tcl
update_timing
write_parasitics -format spef -output vsdbabysoc_spef
```
  
![image](https://user-images.githubusercontent.com/118953917/220655729-977b5f00-e99e-4400-9bba-8d60e4851ac5.png)
  
```
gzip -d /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/shell/write_data_dir/vsdbabysoc/vsdbabysoc.pt.v.gz
cp /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/shell/write_data_dir/vsdbabysoc/vsdbabysoc.pt.v /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/
```
  
> In pt_shell
```
set target_library "/nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/avsddac.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/avsdpll.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/sky130_fd_sc_hd__tt_025C_1v80.db"
set link_library [list /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/avsddac.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/avsdpll.db /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/sky130_fd_sc_hd__tt_025C_1v80.db]
read_verilog /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/vsdbabysoc.pt.v
link_design
current_design
```
  
![image](https://user-images.githubusercontent.com/118953917/220685160-8786848f-2343-4df4-9be1-1fcd8b5c8c47.png)
  
```
read_sdc /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/func1.sdc
set_app_var si_enable_analysis true
read_parasitics -keep_capacitive_coupling /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/vsdbabysoc_spef.temp1_25.spef
```
  
![image](https://user-images.githubusercontent.com/118953917/221361164-8254d2a7-3500-4b58-81cf-e53d3ac7121c.png)

```
check_timing
```
  
![image](https://user-images.githubusercontent.com/118953917/221361327-bc2f7e6d-18dc-4ef7-9ae9-5ec08c886360.png)

```
report_si_bottleneck              (Report of the nets that have the largest crosstalk effects)
report_bottleneck                 (Report of multiple min/max delay violation)
report_si_delay_analysis
report_si_aggressor_exclusion
report_si_noise_analysis
```
  
![image](https://user-images.githubusercontent.com/118953917/221361846-fb1eb9b4-a7fb-48e2-b102-aedad84a753f.png)
![image](https://user-images.githubusercontent.com/118953917/221361902-76fd9833-8b53-40a4-910b-0114168c59f4.png)
 
</details>

## Day-28

### Topic: Introduction to DRC/LVS 

### Introduction to SkyWater SKY130
<details>
  <summary>Theory: Introduction to SkyWater PDKs and opensource EDA tools</summary>
  
### Introduction to Skywater PDK
  
* SkyWater Open Source PDK is a joint project between Google and SkyWater Technology Foundry, where it provides a fully open source Process Design Kit (PDK), and its related resources.
  
* SkyWater open PDK public repository contains:
  + Documentation: https://skywater-pdk.readthedocs.io/en/main/
  + PDK Library and files: https://github.com/google/skywater-pdk
  + Community: https://invite.skywater.tools/
  
![image](https://user-images.githubusercontent.com/118953917/220237707-8bafcbff-c95a-456b-9fd4-7e666c1b034c.png)

* "130" in SKY130 stands for the feature size, which is the length of smallest transistor that can be manufactured in the process.
  
### Open-Source EDA Tools
  
* Open_PDKs is a Makefile based installer that takes files from the SkyWater PDKs and reformats them for a number of open source EDA tools.
  
* Tools that is supported by open_pdks:
  1. Magic
  2. Klayout
  3. Openlane
  4. Xschem
  5. Netgen
  6. Ngspice
  7. IVerilog
  8. qflow
  9. IRSIM
  10. xcircuit
  
* The libraries supported by open_pdks are:
  1. Digital standard cells i.e. sky130_fd_sc_hd
  2. Primitive devices/analog i.e. sky130_fd_pr
  3. I/O cells i.e. sky130_fd_io
  4. 3rd party libraries i.e. sky130_ml_xx_hd
  
* Open_PDKs uses a common installed file system structure, where SkyWater PDKs are under ```/usr/share/pdk/sky130A/``` directory.
  
* There are 2 subdirectories under the main SKY130 PDK's directory.
  + ```libs.tech```
  + ```libs.ref```
  
* ```libs.tech``` --> containing all subdirectories for the open source tool setup.
  
* ```libs.ref``` --> containing the reference libraries in various formats.
  
* ```project_root/``` --> project directory that is containing subdirectories for each tool or flow needed.
  
### Physical Verification and Design Flow
  
* Physical verification is perfomed to check whether we have a mask layout that matches what we think the circuit should be.
  
* There are 2 major steps in physical verification.
  + **Design Rule Checking (DRC)** --> to ensure that the layout matches all the rules provided by the foundy for the specific process.
  + **Layout Vs. Schematic (LVS)** --> to ensure that the layout netlist matches the schematic netlist.
 
</details>

<details>
  <summary>Lab: Tool installations and basic DRC/LVS design flow tools</summary>
  
### Opensource EDA Tools: Check Tool Installations
  
**Magic**
  
* Command ```magic``` in the command prompt to invoke magic interface. 
  
* A layout window and a console window that is used to run commands for layout and actions will be popped out.
  
* Tcl interpreter can be invoked in the terminal instead of seperate console window by using the option ```magic -noconsole```.
  
* Magic can also be run without graphics layout window using the option ```magic -dnull - noconsole```, and should be called as such when running from a script. 
  
* Command ```magic -dnull -noconsole filename.tcl``` is used to run magic in batch mode.
  
**Netgen**
  
* Command ```netgen``` in the terminal to invoke Netgen. It is completely command driven and has no graphics interface. The console window is a stock tcl interpreter as in Magic.
  
* Tcl interpreter can be invoked in the terminal instead of seperate console window by using the option ```netgen -noconsole```.
  
* Command ```netgen -batch source filename.tcl``` is used to run Netgen in batch mode.
  
* Netgen provides GUI window written in python that can be accessed using ```usr/local/lib/netgen/pyhton/lvs_manager.py```, though this interface hides many useful options that cannot be accessed with just this window itself.
  
**Xschem**
  
* Command ```xschem``` in the terminal to invoke Xschem. This should bring up a schematics window.
  
* Xschem has no seperate console window and uses native command line terminal for tcl commands, unlike Netgen and Magic.
  
* Command ```xschem --tcl filename.tcl -q``` is used to run Xschem in batch mode.
  
**Ngspice**
  
* Command ```ngspice``` in the terminal to invoke Ngspice in Linux.
  
* Ngspice has its own prompt and runs its own set of interpreter commands that aren't based on tcl. 
  
* Command ```ngspice -b``` is used to run Ngspice in batch mode.
  
### Creating Sky130 Device Layout In Magic
  
```
cd /home/nur.nazahah.mohd.amri/Desktop
mkdir inverter
cd inverter
mkdir xschem
mkdir mag
mkdir netgen
```
  
![image](https://user-images.githubusercontent.com/118953917/220499836-65254833-457d-4234-b894-340f8013d663.png)
  
```
cd xschem
ln -s /usr/share/pdk/sky130A/libs.tech/xschem/xschemrc
ln -s ln -s /usr/share/pdk/sky130A/libs.tech/ngspice/spinit .spiceinit
cd ../mag/
ln -s /usr/share/pdk/sky130A/libs.tech/magic/sky130A.magicrc .magicrc
cd ../netgen/
ln -s /usr/share/pdk/sky130A/libs.tech/netgen/sky130A_setup.tcl setup.tcl
```
  
**xscheme**

```
cd inverter/xschem/
xschem
```
  
![image](https://user-images.githubusercontent.com/118953917/220499889-19c52a2e-16d2-4bc5-9be5-a3c9e8f272e5.png)

* This brings up a display for xschem with a lot of example schematics, SKY130 devices are shown in xschem as below.
  
*Note: Examples can be accessed by clicking the relevant rectangle and pressing the "E" key on the keyboard. We can return to the menu by pressing "CTRL+E". The "F" key resizes the schematic to fit the window.*
  
![image](https://user-images.githubusercontent.com/118953917/220499949-67b0a991-0082-4f97-bf3e-77e2a148c0e2.png)
  
**magic**

```
cd ../mag/
magic
magic -d XR     (To invoke a cairo graphics package that uses 3D acceleration to get better rendering than the default graphics)
magic -d -OGL   (An OpenGL based graphics package)
```
  
* This brings up 2 magic windows, with the layout window displaying "Technology: sky130A", along with many colors and icons displaying the available layers in this technology, as shown below.
  
![image](https://user-images.githubusercontent.com/118953917/220500607-d3316a91-8339-4df7-9475-3768d9f360cf.png)
  
* Useful Magic Shortcuts:
  + Left and right mouse buttons --> to adjust the cursor box
  + Shift+Z --> to zoom out
  + Middle mouse button/P --> to select a layer (also known as painting)
  + Key E --> to erase whatever is present in the cursor box (can also be done by clicking the middle mouse button on an empty part of the layout)
  + Key V --> to view the entire layout
  + CTRL+P --> opens up the parameter options for the selected device
  + S key --> to select layers
  + Typing "what" command in the magic console --> gives information on the selected layer
  + ";" key --> to type commands in the magic console without moving between windows, until the Enter key is pressed
  + I key --> to select a device
  + M key --> to move the selected device

* To edit Devices drop down buttons: Click on Devices 1 -> nmos (MOSFET)
  
* Select nmos (MOSFET) under "Devices 1" and set the width to 2 um, length to 0.5 um and fingers to 3.
  
![image](https://user-images.githubusercontent.com/118953917/220506020-3aa752a5-9940-49a8-adad-a79f115b651c.png)
  
![image](https://user-images.githubusercontent.com/118953917/220509577-9870bcde-5ec6-4b83-9f44-74a042da6952.png)

* Changing the device type to sky130_fd_pr__nfet_g5v0d10v5
  
![image](https://user-images.githubusercontent.com/118953917/220510768-6080b79d-b78e-49ac-a7ed-1a9c9aaaec1b.png)

### Creating Simple Schematic In Xschem
  
```
cd ../xschem/
xschem
```
  
* Press "Insert" key to pop out Choose symbol window. Select the SkyWater library directory path to access SkyWater components and choose the fd_pr library. To create an inverter, a basic nfet and pfet are needed. Therefore, select nfet and pfet device from the insert window and place it anywhere in the schematic.
  
![image](https://user-images.githubusercontent.com/118953917/220518340-6c1f52c3-21a1-4bf7-8043-3f5bbba34cb4.png)
  
* As pins are not PDK specific, they can be found under the xschem library in the insert window. These are named as ipin.sym, opin.sym and iopin.sym. 
  
* Place the pins and use M key to move the components around on the schematic window. Use C key to copy the components and Del key to erase components. Make use of W key to insert wires between components and make connections. 
  
* Rename each pin to something sensible using the Q key to bring up the parameter window.
  
* Select the components by clicking on them and click Q key to bring up the parameter windows to configure the properties of the devices. 
  
* For **nfet**, change the length to 0.18 as the default value of 0.15 is restricted to sram devices only. Set the number of fingers to 3, and the width of each finger to 1.5. 
  
* Since we have 3 fingers now, the total width in the parameter window must be set to 3 times of the finger width, which is 4.5. 
  
* Similarly, for **pfet**, adjust the parameters to 3 fingers, width of 1 per finger, and a length of 0.18. We must specify the body to be connected to the Vdd pin as it is a 3 pin pfet.
  
![image](https://user-images.githubusercontent.com/118953917/220823402-5d940050-8723-4ffc-9ddb-5fe593bc2b9c.png)
  
* Save the design by clicking tab File --> save as --> inverter.sch
  
### Creating Symbol And Exporting Schematic In Xschem
  
* To functionally validate the schematic, testbench that is separated from the schematic must be created. 
  
 * Firstly, create a symbol for the schematic as the schematic will appear as a symbol in the testbench. To do this, click on the Symbol menu and select "Make symbol from schematic". Then, create a testbench schematic using new schematic option and insert the generated symbol from the local directory using the Insert key.
  
* Select new schematic in File tab and choose inverter.sch under home directory and paste it on the schematic window.
  
* The testbench will be very simple where we will generate a ramp input and observe the output response after connecting the power supplies. To do this, insert 2 voltage sources from the default xschem library, one for the input and one for the supply. Connect these and add a GND node to the supply connections. Create "ipins" and "opins" for the input and output signals to observe in Ngspice. 
  
* Supply voltage is set to 1.8 V. For the input voltage, we must set the supply to a piece-wise linear function to get ramp. PWL function has voltage and time values stated that the supply will start at 0v, then start to ramp up from 20 ns till it reaches its final value at 900 ns of 1.8 V. 
  
* Next, place two more statements for ngspice, but as these aren't specific to any component, they must be placed in text boxes. To place a text box, select the code_shown.sym component under the xschem library.
  
* The first text box will specify the location of the device models used in the device schematic, where it is using a .lib statement that selects a top level file that tells ngspice where to find all the models and also specifying a simulation corner for all the models. The first block specifying the typical corner with ```value = ".lib /usr/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt"```. 
  
* For the second block, it specifies;
```
value = ".control
tran 1n 1u
plot V(in) V(out)
.endc"
```
  
* This will tell ngspice to run a transient simulation for 1 ns and monitor voltages for the in and out pins. Therefore, a complete testbench schematic is shown as below, and save this as inverter_tb.sch
  
![image](https://user-images.githubusercontent.com/118953917/220539048-6f9c5a5c-10f7-45e0-be88-52af87624eb3.png)

* To generate the netlist, click on the Netlist button, then simulate it in Ngspice by clicking the Simulate button. 
  
* The waveform confirms that the schematic behaves as an inverter as shown below.
  
![image](https://user-images.githubusercontent.com/118953917/220541463-5897304d-d91e-4325-99ae-97bd59da2003.png)

* After verified the schematic, create a layout for it. To do this, go back to the inverter schematic. 
  
* Firstly, click on the Simulation menu and select "LVS netlist: Top Lvel is a .subckt" option. 
  
* Wait a few seconds and go back to the Simulation menu to check whether a tick mark appears beside the aforementioned option. This verifies if we have properly defined a sub circuit for creating a layout cell with pins in the layout. 
  
* Finally, generate a netlist for the schematic by clicking the Netlist button and exit Xschem.
  
### Importing Schematic To Layout And Inverter Layout Steps
  
```
cd ../mag/
magic -d XR
```  
  
* Import the schematic to the layout in Magic by running the magic, then click on File -> Import SPICE and then select the inverter.spice file from the xschem directory. If done correctly, the following layout has been opened up in magic.
  
![image](https://user-images.githubusercontent.com/118953917/220548501-d5759edd-b58c-4149-8490-f560e0906740.png)
![image](https://user-images.githubusercontent.com/118953917/220548959-0d23c844-8ca0-4112-a574-6a7e900f7516.png)

* Referring to the layout generated above, the schematic import does not know how to do analog placing and routing as it is very complicated. Therefore, We must place them in the best positions and wire them up manually. 
  
* Firstly, place the pfet device above the nfet and adjust the placement of the input, output and supply pins. Refer below figure.
  
![image](https://user-images.githubusercontent.com/118953917/220553775-6486d0f0-3144-48c1-815f-cae9f2dfb513.png)

* Next, set some parameters that are only adjustable in the layout which will make it more convenient to wire the whole layout up. 
  
* To pop out the parameter editing section, use S key and press I key to select the object, then use CTRL+P to open up the parameter options for the selected device.
  
* Set the "Top guard ring via coverage" to 100. This will put a local interconnect to metal1 via ta the top of the guard ring. Next, for "Source via coverage", put +40 and for "Drain via coverage", put -40. This will split the source drain contacts, making it easy to connect them with a wire. 
  
* For nfet, set the "Bottom guard ring via coverage" to 100, while the source and drain via coverages are set to +40 and -40, respectively, like the pfet.
  
* Start to paint the wires using metal1 layers by connecting the source of the pfet to Vdd and source of the nfet to Vss. Next, connect the drains of both mosfets to the output. Finally, connect the input to all the poly contacts of the gate. 
  
![image](https://user-images.githubusercontent.com/118953917/220818486-c900a932-d619-4b98-a12a-334083f115b4.png)
  
* Save the file and select the autowrite option. 
  
* Run the following commands in the magic console.
  
```
extract do local    (Ensuring that magic writes all results to the local directory)
extract all         (Performing the actual extraction)
ext2spice lvs       (Simulating and setting up the netlist to hierarchical spice output in ngspice format with no parasitic components)
ext2spice           (Generating the spice netlist)
```
  
![image](https://user-images.githubusercontent.com/118953917/220818458-c1c66460-eeb1-4730-8c2b-a3ac764171a0.png)
  
```
rm *.ext                                          (Clear any unwanted files -> .ext files are just intermediate results from the extraction)
/usr/share/pdk/bin/cleanup_unref.py -remove .     (Clean up extra .mag files -> files containing paramaterised cells that were created and saved but not used in the design)
netgen -batch lvs "../mag/inverter.spice inverter" "../xschem/inverter.spice inverter"    (Run LVS by entering the netgen subdirectory)
```
  
* Remember to always use the layout netlist first and schematic netlist second in the netgen command as in side by side, resulting the layout is on the left and the schematic is on the right. 
  
* Each netlist is represented by a pair of keywords in quotes, where the first is the location of the netlist file and the second is the name of the subcircuit to compare. 
  
* As we can see from the result below, there was an issue in the wiring and the netlists do not match. This is due to wiring errors in the layout.
  
![image](https://user-images.githubusercontent.com/118953917/220819365-90a4363d-2c28-4b7c-87a8-a38dc16681f8.png)
  
**Debugging errors in netlist, rerun and save layout**
  
```
extract do local
extract all
ext2spice lvs
ext2spice cthresh 0     (Tells magic to add all the parasitic capacitances to the spice netlist)
ext2spice
```

* Referring to the netlist file below, there are multiple lines beginning with C, which detail the parasitic capacitances.
  
```
vim inverter.spice 
```
  
![image](https://user-images.githubusercontent.com/118953917/220836829-6b35e6a9-19a8-41f0-b85a-1eba2ef39a99.png)

```
cp ../xschem/inverter_tb.spice .
vim inverter_tb.spice
```
  
* Modify the test bench netlist file.
  
![image](https://user-images.githubusercontent.com/118953917/220838344-940395c3-c800-4330-a236-78ee3ecc8be6.png)

```
/usr/share/pdk/bin/cleanup_unref.py -remove .
cp ../xschem/.spiceinit .
ngspice inverter_tb.spice
```
  
* The result is almost the same as in previous simulation in xschem.
  
![image](https://user-images.githubusercontent.com/118953917/220866522-c7d71cdd-8749-42a9-99f4-a9ccf70c3989.png)
  
</details>


### DRC/LVS Theory and labs
<details>
  <summary>Theory: Introduction to DRC and LVS</summary>
  
### Fundamentals of Physical Verification
  
* As chip gets denser, the scale of physical verification increases. 
  
* Chip designs can be hierarchical, while physical verification cannot. 
  
* Two primary aspects of physical verification are: 
  1. Design Rule Checks (DRC) --> Ensures that the design layout meets all the silicon foundry rules for mask making.
  2. Layout vs Schematic (LVS) --> Ensures that the design layout electrically matches the design, as implemented in schematic form or any form that electrically describes the design specifications. 
  
* Since the chips are designed from a single source (RTL design), the LVS is now checking the design through different flows where:
  1. Starting at the RTL source and working forwards.
  2. Starting at the finished layout and working backwards. This way the tools used cross check each other.
  
![image](https://user-images.githubusercontent.com/118953917/220867039-df1f63fd-aabb-4a69-81c8-9f93353faa72.png)

* Basically, physical verification must check if any manual intervention has broken something. 
  
* However, for errors, it is looking for how the tool got it wrong and how we can modify the setups to overcome the problem. 
  
* Increasing the number of tools used, increases the robustness of the physical verification process.
  
### Understanding GDS Format
  
* For some form of standardisation to describe integrated circuits, a standard file format is needed. 
  
* Some common file formats are:
  + Caltech Intermediate form (.cif)
  + GDSII stream format
  + Open Artwork System Interchange Standard (OASIS)
  
* GDSII format is an industry standard accross foundries for representing IC layouts. 
  
### Extraction Commands, Styles and Options In Magic

* Extraction process: The layout tool needs to generate a netlist independently by looking at the other than the mask geometry of the layout. 
  
* Extraction in Magic has two stage process, wherein magic generates an intermediate netlist format called the .ext, after it is converted to the required netlist format like spice.

![image](https://user-images.githubusercontent.com/118953917/220926384-dfe4bbcb-815d-40e0-9bb6-c2358abaeedb.png)

* All devices, instances, connections between cells, subcells, nets, as well as parasitics are present in the netlist. 
  
* The netlist can be fed into a simulator such as Ngspice, along with a schematic captured netlist to compare the results of the two.
  
* Eventhough magic can create a netlist for simulation, the tool don't know on how to actually simulate the netlist. 
  
* Thus, to simulate a netlist from a layout, we must provide all the missing information including the testbench netlist, along with the necessary stimuli for simulation. 
  
* As the layout editor knows nothing about the actual device models, we need to use include statements to add all device models used in the layout. 
  
* Subcircuit netlist is the generated netlist from the layout editor and must be included as well. 
  
* Finally, an analysis control block is needed to tell the simulator what kind of simulation to run as well as its simulation parameters.
  
* There are three extraction styles available in magic: 
  1. ngspice()
  2. ngspice(orig) 
  3. ngspice(si)
  
* Those styles can be selected using the commands below.
  
```
extract style ngspice()
extract style ngspice(orig)
extract style ngspice(si)
```
  
* Some extraction options in magic are as follows.
  
```
ext2spice lvs
ext2spice cthresh value
ext2spice scale on|off
ext2spice hierarchy on|off
ext2spice subcircuit top on|off
ext2spice global on|off
ext2spicemerge on|off
```
  
*Note: Magic also stores layer heights/thicknesses, and a three dimensional view of the layout can be rendered by magic's 3D engine using the menu button Option -> 3D Display.* 
  
### GDS Reading and Writing in Magic
  
* GDS files can be accessed in Magic with the ```gds``` command. 
  
* To read a GDS file in magic, use ```gds read file_name```. 
  
* Some important read options for gds files in magic are listed below.
  
```
gds readonly true|false  (Allows ceratin cells to be read-only, preventing magic from changing their gds descriptions in the final output gds file)
gds flatglob expression  (Flattens cells in question to be merged up into the hierarchy above them, preventing unnecessary hierarchy in the layout)
gds flatten true
gds noduplicates true    (Tells magic to ignore cell definitions in gds files that it already has in memory)
```
  
* GDS files can be written in magic using the command ```gds write file_name```, and some of its options are listed below.
  
```
gds library true      (Used to create gds library files with subcells with no concept of a top level layout)
gds addendum true     (Ignores read-only cell definitions when it generates an output)
gds merge true|false  (Turns rectangles and triangles present in the design into merged polygons for easier viewing)
```

### DRC Rules in Magic
  
* Magic implements an interactive DRC, wherein it shows DRC errors when we make them. 
  
* As this process is computationally expensive, magic uses 3 styles for running DRC, namely:
  1. drc(full) - complete checks (slow)
  2. drc(fast) - typical checks (fast)
  3. drc(routing) - metal checks (fastest)
  
### LVS Setup for Netgen
  
* Netgen is a tool used for running LVS checks. It knows nothing about layouts, and only knows about netlists and how to read and compare them.
  
* Netgen does not need to know anything about any components in the design, it juts needs to know wheter they match in the layout and schematic.
  
* LVS technology setup file tells the LVS tool what all the device names are, how they should or shouldn't be combined in series and parallel, whether any pins on the device are permutable (interchangeable), which properties are interesting to compare between netlists, which properties should be ignored, and whether any device must be ignored.
  
### XOR Verification
  
* This is a physical verification method used to compare 2 layouts. Here, an XOR operation is applied on the masks of the two layouts where both the masks either have nothing or share the same geometry, we see nothing, and only where one mask has something and the other mask has nothing, or vice versa, do we see something. This is useful in mask revisions.
  
* To run an XOR operation in Magic, we can use the following commands.
  
```
load layout1_name
flatten destination_name
load layout2_name
xor destination_name
```
  
</details>

<details>
  <summary>Lab: Labs for GDS read/write, extraction, DRC, LVS and XOR setup</summary>
  
### GDS Read
  
```
cd /home/nur.nazahah.mohd.amri/Desktop
mkdir lab2
cd lab2
mkdir mag
cd mag
cp /usr/share/pdk/sky130A/libs.tech/magic/sky130A.magicrc ./.magicrc
magic -d XR &
```
  
> In tkcon (Magic console)
```
cif listall istyle    (To view the possible styles)
cif list istyle       (To see the current style)
cif istyle xxx
gds read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/gds/sky130_fd_sc_hd.gds      (Read the GDS files from the PDK)
cellname top          (To see the available top level cells)
```
  
![image](https://user-images.githubusercontent.com/118953917/220935614-fc72ebd7-659d-4628-a898-d05da6d441a0.png)
  
* Since it is a library, the console lists all the subcells.
  
* The same thing can be accessed with the menu button Options -> Cell Manager -> sky130_fd_sc_hd__and2_1. We shall load a simple and2_1 cell as shown below.

![image](https://user-images.githubusercontent.com/118953917/220935709-3a014798-5960-41f6-81b2-7c7750fad832.png)

> In magic console
```
gds read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/gds/sky130_fd_sc_hd.gds
cif istyle()
```
  
* Referring to the below figure, the labels in the layout view are marked yellow, which means they are treated as regular text. 
  
```
cif istyle sky130(vendor)       (Change style)
gds read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/gds/sky130_fd_sc_hd.gds
```
  
* The current and2_1 layout will automatically be overwritten. 
  
* Here, the labels are colored blue, which means they are treated as ports. This shows that when dealing with vendor files, it is wise to use the vendor style.
  
![image](https://user-images.githubusercontent.com/118953917/220938107-ac0a9ccd-7acf-4798-b3e8-229311110327.png)
  
> In magic console
```
gds noduplicates true   (If don't want to automatically overwrite existing cells when reading from gds)
gds read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/gds/sky130_fd_sc_hd.gds
```

![image](https://user-images.githubusercontent.com/118953917/220940204-9633aa96-edf5-47ae-a4b6-ee978bec1b69.png)

### Ports
  
> In magic console
```
port index    (To inquire ports on a layout)
port first    (To find the index of the first port)
port 1 name
port 1 class
port 1 use
```
  
* Select a port and command as above. Note that we can only select one port at a time for this method.
  
![image](https://user-images.githubusercontent.com/118953917/220942541-c31db8e6-89cf-4a08-9c3e-d78a38dd6b1d.png)

```
ls /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/
cd /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/spice/
gvim sky130_fd_sc_hd.spice
```
  
![image](https://user-images.githubusercontent.com/118953917/220945910-98d17bf4-a64e-4123-ab2b-749c3320b5e6.png)
  
![image](https://user-images.githubusercontent.com/118953917/220945712-ebf92bca-28ab-403b-8cbe-afa62467bd8d.png)

* While the cell definition shows the first port to be port A, the gds read of the file in magic shows the first port as VPWR. 
  
* The port order mentioned in the definition came from the vendor and should be considered correct. However, port numbering is considered metadata and is not included in gds file. 
  
* One way to add metadata to the gds file opened in magic is to read its corresponding lef file. 
  
```
lef read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/lef/sky130_fd_sc_hd.lef    (To read the lef file)
port 1 name
port 1 class
port 1 use
```
  
* Here, the port order is not updated where the lef files do not contain port order metadata. However, port class and use information was imported. Unfortunately, port order is only captured in the spice files from a vendor, but magic has no spice read command as these files provide no layout information.
  
![image](https://user-images.githubusercontent.com/118953917/220948801-190f8292-7e85-499e-820e-f4cdae2b1aec.png)
  
```
readspice /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/spice/sky130_fd_sc_hd.spice   (To read port order from spice files - use custom .tcl script and call it in the magic console) 
port 1 name
port 1 class
port 1 use
```
  
* Load the cell layout again from the Cell Manager and inquire the same port 1 information to check.
  
* The port is already updated and the information has updated.
  
![image](https://user-images.githubusercontent.com/118953917/220952878-3f48d16d-368a-4778-88dc-33a5e4b2ba20.png)

### Abstract Views
  
* For abstraction, we cannot start with a cell in memory. Hence, we need to open a fresh Magic session and read the lef library and load the same and2_1 cell from the Cell Manager.
  
```
lef read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/lef/sky130_fd_sc_hd.lef
```
  
* If we check port information, we can see that port order metadata isn't present in the lef files.
  
![image](https://user-images.githubusercontent.com/118953917/220956130-8c847796-afb7-40b2-9d6e-f5eaac5bfd97.png)

* Select one port and perform below command.
  
```
port first
port 1 name
port 2 name
port 3 name
port 1 use
port 1 class
port 4 name
```
  
* Port order metadata isn't present in the lef files.
  
![image](https://user-images.githubusercontent.com/118953917/220958273-82f1f7bd-ea60-44ba-bd98-2c1ed6365209.png)

* Run the readspice script as before and load the cell again.
  
```
readspice /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/spice/sky130_fd_sc_hd.spice   --> load from cell manager
load test
getcell sky130_fd_sc_hd__and2_1
```

* Note: after load cell, make an empty box in empty space in magic. Then, command getcell.
  
![image](https://user-images.githubusercontent.com/118953917/221087159-f0b22c55-70b1-4b17-b769-624595503e4d.png)
  
* Click an empty space outside the cell, select the cell and command as below.
  
```
gds write test      (To write lef file to gds file)
quit
```
 
![image](https://user-images.githubusercontent.com/118953917/221087135-119a66cb-2605-4800-83e1-826163867001.png)

* Reinvoke magic and command as below.
  
```
gds read test     (To read gds file)
save test
quit
```

![image](https://user-images.githubusercontent.com/118953917/221088032-f1780516-1a96-4fa6-8337-487913a40e9c.png)
  
```
load test
path                (Viewing standard cell contents from the library path)
gds write test
```
  
![image](https://user-images.githubusercontent.com/118953917/221089189-239a4430-ffd3-46f8-8943-4020f283f396.png)

* Select the cell, hit the ">" key and perform command as below.
  
```
property
```
  
![image](https://user-images.githubusercontent.com/118953917/221089800-3452c845-5539-4ace-86bd-dd632789b41c.png)
  
* Highlight the cell by using key i and select the desired cell. Then, press ">" key and press scroller of the mouse to paint the selected layer using locali. 

```
cellname writeable sky130_fd_sc_hd__and2_1 true
gds write test
quit
```
  
![image](https://user-images.githubusercontent.com/118953917/221118636-c88e4b74-43ce-44ae-a20a-d15ecaa7357d.png)

```
gds read test
quit
```
```
gds readonly true
gds rescale false
gds read /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/gds/sky130_fd_sc_hd.gds
load sky130_fd_sc_hd__and2_1
property
```
  
![image](https://user-images.githubusercontent.com/118953917/221120672-80959213-dca1-4979-9c22-d029bbb8dc65.png)
![image](https://user-images.githubusercontent.com/118953917/221122008-e5a32ddd-2457-4a9e-8a65-429e45aa40b2.png)

### GDS Reading Option In Magic
  
```
load sky130_fd_sc_hd__and2_1
extract all
ext2spice lvs
ext2spice
```
  
![image](https://user-images.githubusercontent.com/118953917/221124067-48afe37e-e238-425d-a6e2-905378a68710.png)

```
cd /home/nur.nazahah.mohd.amri/Desktop/lab2/mag
gvim sky130_fd_sc_hd__and2_1.spice
```
  
![image](https://user-images.githubusercontent.com/118953917/221125269-b8cc7f78-c82c-4b50-a749-e73e8b982d8c.png)

```
ext2spice cthresh 0     (Include the parasitic capacitances)
ext2spice
```
  
![image](https://user-images.githubusercontent.com/118953917/221126118-63ab8d90-3859-4bdc-9344-d4e879f5441b.png)

```
cd /home/nur.nazahah.mohd.amri/Desktop/lab2/mag
gvim sky130_fd_sc_hd__and2_1.spice
```
  
* Generated netlist containing lines starting with C to denote the parasitic capacitances as shown below.
  
![image](https://user-images.githubusercontent.com/118953917/221126438-0c103791-5d49-4eab-a618-2683e32eca59.png)

```
ext2spice cthresh 0.01
ext2spice
```
  
![image](https://user-images.githubusercontent.com/118953917/221127906-d80cd9b2-8932-4664-b920-066ef3940ad2.png)
  
```
cd /home/nur.nazahah.mohd.amri/Desktop/lab2/mag
gvim sky130_fd_sc_hd__and2_1.spice
```

![image](https://user-images.githubusercontent.com/118953917/221128105-c954f806-c614-4b41-8cb0-6d26e39e87cd.png)

* Running a full R-C extraction
  
```
ext2sim labels on
ext2sim
extresist tolerance 10
extresist                 (Must select cell first before run the command)
```
  
* This shows the number of resistor nets found usable, and creates a .res.ext file which holds information to modify the existing .ext file for R parasitics.
  
![image](https://user-images.githubusercontent.com/118953917/221130496-2e90681f-c35b-4516-b5a7-54a87bbb8070.png)
  
```
ext2spice lvs
ext2spice cthresh 0.01
ext2spice extresist on
ext2spice
```
  
```
cd /home/nur.nazahah.mohd.amri/Desktop/lab2/mag
gvim sky130_fd_sc_hd__and2_1.spice
```
  
* Generated netlist is now containing both R and C parasitic components as shown below.
  
* While this method does work, it is extremely time consuming for the large circuits. For large circuits, it is better to let the router conduct this job as it already knows where all the wires are supposed to connect.
  
![image](https://user-images.githubusercontent.com/118953917/221132106-a7cf8a37-c920-4785-aa21-4418fa18e8c1.png)

### Setup For DRC
  
* To set up standard DRC, use the following commands to call a python script.

```
/usr/share/pdk/sky130A/libs.tech/magic/run_standard_drc.py /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/mag/sky130_fd_sc_hd__and2_1.mag
ls -al *.txt
```
  
![image](https://user-images.githubusercontent.com/118953917/221135334-4de470cf-02e6-4569-86ba-9c6988a2d1e2.png)

```
gvim sky130_fd_sc_hd__and2_1_drc.txt
```
  
![image](https://user-images.githubusercontent.com/118953917/221136091-cd39f2cc-fcac-4da3-ba28-19d5907fd030.png)

```
load sky130_fd_sc_hd__and2_1 
drc style
drc listall style
drc style drc(full)
drc check               (Select cell first then run command)
```
  
  * There are DRC errors in the vendor .mag file for the and2_1 subcell since the standard cell layouts do not have internal connections to the well and substrate to save room, and the layout depends on tap cells to make those connections.

* The reason we haven't seen these DRC errors earlier in Magic is because the DRC script runs a full DRC check, while the default DRC style in Magic was a fast DRC. This is shown below.
  
![image](https://user-images.githubusercontent.com/118953917/221138529-bc1e3827-5586-459a-8536-2ee89b5974ae.png)

```
drc why
drc find
```
  
![image](https://user-images.githubusercontent.com/118953917/221139526-911925eb-62f2-41c0-b735-421dd91bce82.png)

```
load test2
getcell sky130_fd_sc_hd__and2_1
getcell sky130_fd_sc_hd__tapvpwrvgnd_1      (Select an empty space and run the command)
```
  
![image](https://user-images.githubusercontent.com/118953917/221141961-1cbfded9-d476-4d93-a20c-a388648e4bf4.png)
  
* Add and align a tap cell in the existing layout, there are no more DRC errors in the top level.
  
* Use key I to select all, M to move.
  
```
save test3
```
  
![image](https://user-images.githubusercontent.com/118953917/221142740-12ae1b8a-a58c-42fa-9047-70c4ad376234.png)

### Setup For LVS
  
```
mkdir netgen
cd netgen
cp /usr/share/pdk/sky130A/libs.tech/netgen/sky130A_setup.tcl ./setup.tcl
cd ../mag/
magic -d XR sky130_fd_sc_hd__and2_1 &
```
  
```
ext2spice lvs
ext2spice 
quit
```
  
![image](https://user-images.githubusercontent.com/118953917/221147630-8593129e-d1aa-4acf-b8d2-65018827920f.png)
  
```
cd ../netgen/
netgen -batch lvs "../mag/sky130_fd_sc_hd__and2_1.spice sky130_fd_sc_hd__and2_1" "/usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/spice/sky130_fd_sc_hd.spice sky130_fd_sc_hd__and2_1"
```

![image](https://user-images.githubusercontent.com/118953917/221148331-7984985b-50e4-4983-b9bd-c215163bdd08.png)

### Setup For XOR
  
```
cd ../mag/
magic -d XR &
load sky130_fd_sc_hd__and2_1
save altered
load altered
```
  
![image](https://user-images.githubusercontent.com/118953917/221151613-553eb80d-9e0e-40bf-9542-96ab2985dad9.png)

```
erase li                           (Select cell first then run command)
flatten -nolabels xor_test
load sky130_fd_sc_hd__and2_1
xor -nolabels xor_test
load xor_test
quit
```

![image](https://user-images.githubusercontent.com/118953917/221152723-05e15105-903d-467c-b47e-6fb18c87e71d.png)

```
magic -d XR &
load test3
flatten -nolabels xor_test
xor -nolabels xor_test        (Select AND cell before run the command)
load xor_test
```
  
![image](https://user-images.githubusercontent.com/118953917/221194185-2bf924f1-5123-4358-bb42-1a58afd3beae.png)

</details>

### Front-end and back-end DRC
<details>
  <summary>Theory: Introduction to DRC rules</summary>
  
### Fundamentals of Design Rule Checking
  
* Every silicon manufacturing process has its own tolerances on the designs being manufactured. 
  
* These tolerances are dependant on the conditions and machines used in the fabrication environment, and are based on probabilities of expected failure/defects found in a manufactured batch, in parts per million. 
  
* Thus, each process has its own set of rules that need to be adhered to, and these rules are given on the geometry of the layout to prevent chip failure. 
  
* These are known as design rules, and the process of adhering to these rules is done by design rule checking.
  
### Back-end Metal Layer Rules
  
* Width Rule
* Spacing Rule
* Wide-Spacing Rule
* Notch Rule
* Minimum and Maximum Area Rules
* Minimum Hole Area Rule
* Contact Cut Rules

</details>

<details>
  <summary>Lab: Labs for all DRC rules</summary>
  
### Lab For Width Rule And Spacing Rule
  
```
cd /home/nur.nazahah.mohd.amri/Desktop
git clone https://github.com/RTimothyEdwards/vsd_drc_lab
cd vsd_drc_lab
./run_magic
```
  
![image](https://user-images.githubusercontent.com/118953917/221197881-2b2a1d8d-fb8d-4251-bedd-919494ba814c.png)

* In magic, click on File tab -> open -> select exercise1.mag
  
![image](https://user-images.githubusercontent.com/118953917/221198422-a5acc4b4-03ec-44db-a8f5-bfdd9dc9bb43.png)

* Select any object -> go to Drc tab -> DRC Report.
  
![image](https://user-images.githubusercontent.com/118953917/221199251-ec13c74a-b402-4c43-9a56-44fc7d825f78.png)

![image](https://user-images.githubusercontent.com/118953917/221199825-711c55bc-a487-4ac0-af79-3f40243fcc42.png)
  
```
box width 0.14um
paint m2
```
  
![image](https://user-images.githubusercontent.com/118953917/221201646-130b38e3-a181-4832-9082-a650860cb4de.png)

* DRC report.
  
* The white dots on each of the metal1 pieces indicate 1 error, meaning magic counts this spacing error as 2 DRC errors, and by moving either of the rectangles away, the DRC error can be fixed. 
  
* We can do this by selecting any of the rectangles, and using the numpad keys 4 and 6 to move them. Or using the command ```move e 0.14um``` which moves the selected layer to the east.
  
![image](https://user-images.githubusercontent.com/118953917/221202561-04791f35-a01c-454e-91ac-f61c49efc97c.png)

![image](https://user-images.githubusercontent.com/118953917/221203541-87aaf02b-07b6-48f1-bce7-6a6382590365.png)

### Lab For Wide Spacing Rule And Notch Rule
  
* If we select the cursor box over the area and run a DRC report, we can find 2 errors. 
  
* The first is a regular spacing error for the smaller rectangle, and the second shows a wide spacing error for the larger rectangle.
  
![image](https://user-images.githubusercontent.com/118953917/221204374-91ed856f-b04e-4005-bf06-a2074515b003.png)

* Fix can be done by moving either of the boxes by a distance of 0.4um away, as shown below.
  
```
move w 0.4um
```
  
![image](https://user-images.githubusercontent.com/118953917/221204803-0da1d0e5-7470-4bba-b681-6f69b181af33.png)

* If we run a DRC report, it shows up as a spacing error, since notch errors are generally the same as spacing errors for most processes.

![image](https://user-images.githubusercontent.com/118953917/221205173-0905a82d-b5b6-4a24-97b4-9271f67a9cb7.png)

* As this is the same layer, we cannot simply move this around. 
  
* To fix this, we put the cursor box over one half of the shape. Next we use the area select or A key to select just the top portion of the shape like shown below.

![image](https://user-images.githubusercontent.com/118953917/221206971-e85b7a15-7a83-4253-832f-58d8291b9951.png)

### Lab For Via Size, Multiple Vias, Via Overlap and Autogenerate Vias
  
* Load exercise2.mag and look at exercise 2a. 
  
* Run a DRC report, we see it is a simple via size error.
  
![image](https://user-images.githubusercontent.com/118953917/221211209-c4aa1cc4-6364-4f0c-be7d-54dd4b89c571.png)
  
* We can fix this easily by doing an area select and stretch operation twice. Once horizontally, and once vertically.

![image](https://user-images.githubusercontent.com/118953917/221211898-f742f51a-f251-4724-bb16-e73144de279a.png)

* For example 2b, we have a large via with an array of contact cuts. 
  
* We cannot see the contact cuts yet though, as Magic displays them as a single via for viewing ease. 
  
* To see the contact cuts, we can run the command ```cif see xxx``` to check the cif layer names.
  
![image](https://user-images.githubusercontent.com/118953917/221214193-f974d9cc-604c-40bb-b28d-8681e6bf57f2.png)
  
* Now let us explore example 2c. Here, we have an overlap error as shown below.

![image](https://user-images.githubusercontent.com/118953917/221218146-f2291362-ee35-44d8-9145-c0622ad00d83.png)

* To fix this, we can simply select the layer then use the box grow command as shown. Here, the c stands for center, which means grow around center. Next we paint in a layer of metal1 to fix the overlap error.
  
![image](https://user-images.githubusercontent.com/118953917/221218331-375be2aa-c1b3-4df4-bf5b-dddd923bff0c.png)
  
* To automatically generate a via in Magic without any box manipulation, we can do the following. Let's see example 2d. If we use the wiring tool by cycling through the tools with the space key, we can quickly draw wires. By clicking SHIFT+left MB we can move up a metal layer until we reach the top most metal 5 layer. Similarly, we can move down layers with the SHIFT+right MB until we reach the metal interconnect.
  
![image](https://user-images.githubusercontent.com/118953917/221216035-4ab378fd-0d00-4b85-b47e-fc1eeeba6c9a.png)

###  Lab For Minumum Area Rule And Minimum Hole Rule
  
* Load in example3.mag. Here, we have a layer of metal with the following DRC error.
  
![image](https://user-images.githubusercontent.com/118953917/221226474-cd1ff7ff-c25c-44f9-840e-f1eaf29ffa3c.png)

* To fix this, we can easily area select and stretch the layer to meet the requirement.
  
![image](https://user-images.githubusercontent.com/118953917/221226819-be6d062d-6042-442e-a486-2c0ceabe0706.png)

*Note: Wire routes always meet the minimum area rules when using the wiring tool. However, when jumping up by 2 or more layers, it is possible to violate this rule,  where there isn't enough metal 1 to fit the minimum area rule, along with other violations.*
  
![image](https://user-images.githubusercontent.com/118953917/221227089-f81e2134-14dd-40e9-ab55-7fa0ae0f8bf3.png)

* Next, example 3b. Here we have a minimum hole area violation, though Magic does not show it as one. 
  
* To see this DRC error, we need to run Magic in the full DRC mode. We can do this by clicking on the menu button DRC > DRC complete. Next we must tell Magic to update the DRC count, and then run a DRC report.
  
![image](https://user-images.githubusercontent.com/118953917/221227428-3f0335e7-2b94-4b7e-bc76-7ecb94f235c9.png)

### Lab For Wells And Deep N-Well
  
* exercise4.mag has wells and taps, so our DRC check needs to be set at full to check for these. 
  
* In example 4a, we have a basic well error, since the wells do not have taps. 
  
* The n-well shows an error as it is currently floating, though the "p-well" does not since this process doesn't actually consider p-wells unless they are in deep n-wells, and are instead counted as p-substrates.
  
![image](https://user-images.githubusercontent.com/118953917/221407708-041c63bb-3cf9-45bc-b766-397d62eaaf39.png)

* To fix this, we first paint a layer of n type material into the n well. The layer is called nsubstratendiff. Though this does not fix the DRC error.
  
![image](https://user-images.githubusercontent.com/118953917/221407794-eeb95971-e19a-401c-a3d0-bf2eee636403.png)

* n well must not be floating, so the tap should be connected to a layer of local interconnect. 
  
* Paint a layer of nsubstratencontact. While this gets rid of the n-well DRC error, it creates smaller errors like overlap and surround.
  
* Adjust the layers using what we have learnt till now by growing, stretching and adding in local interconnect, and we get no DRC errors.

![image](https://user-images.githubusercontent.com/118953917/221407841-f0778a51-b758-4dfe-9bac-a813b5c0beed.png)

![image](https://user-images.githubusercontent.com/118953917/221407900-8984bb2d-fc65-4752-b3ee-e0106c125b77.png)
  
###  Lab For Derived Layers
  
* Load exercise5.mag. These derived layers look like a transistor, and we can check to see what the overlapping layer is considered as by using the command what shown below.
  
![image](https://user-images.githubusercontent.com/118953917/221228252-a93cd3bc-7186-4ca7-8f18-e3b4c42af6c8.png)

* It is a nmoslvt layer. We can try to recreate this by painting in a layer of ndiff and poly and see what the overlapping layer becomes.

![image](https://user-images.githubusercontent.com/118953917/221228563-478e3cb3-4b2d-41d5-99c9-d0192e2aa2b4.png)
  
* As we can see, Magic considers this as an nmos, and not an nmoslvt as before. 
  
* This is based on the instructions specified in the tech file, so to get a layer of nmoslvt, we must specifically paint in a layer of nmoslvt. We have learnt earlier about Magic's implant layers and how they are used for autogeneration. Let us visualize these implant layers with the following commands.
  
![image](https://user-images.githubusercontent.com/118953917/221228842-0215fda8-ad9e-4c5f-ad66-2cd0280c456e.png)

* Similarly, we can see the implant lvt layer on the nmoslvt using the command below. This is what differentiates the nmoslvt from the nmos we just created.
  
![image](https://user-images.githubusercontent.com/118953917/221229028-802987c7-5522-4eee-9ce9-f79325d5a1fd.png)

* Here we can check the layers with the ```what``` command as before.
  
![image](https://user-images.githubusercontent.com/118953917/221229289-6b3926c3-7f59-4455-9ca9-2042d00443f2.png)
  
### Lab For Paramterized And PDK Devices
  
* Load exercise6.mag. Here, we have a parameterised device, which is a device create automatically using magic, and have fixed layouts in the PDKs. 
  
* It is a simple nmos (MOSFET) device created from the Devices 1 menu with default parameters. While this is a Magic generated device, it still shows up as having DRC errors as follows.
  
![image](https://user-images.githubusercontent.com/118953917/221396864-0068e3c2-77ca-41c7-9011-097a44cd1a95.png)

* Th error is a metal minimum area rule error, and magic could avoid it by adding more metal to the layer, but it does not since these contacts will be routed to, and have metal connected to it anyway. We can go ahead and fix this by painting in some metal1 over the contacts, and we see no DRC errors.
  
![image](https://user-images.githubusercontent.com/118953917/221397378-167b5bca-bc17-4a63-b6ba-44f4da96d499.png)

* Another example, we have an esdfet designed to survive high voltages, and it purposefully breaks design rules. For example, it has a transistor gate at an angle, which is not allowed.
  
![image](https://user-images.githubusercontent.com/118953917/221397508-e6e81b5d-eacd-429f-891b-ed6bb5562b3e.png)

* The only way in Magic to ignore rule errors for a device that is known good, is to abstract it. Though abstract views are in a separate lef directory, but that is not what is needed here. 
  
* All library views are abstract in a way, because they contain pointers to gds files, and thus what is shown is no more than Magic's best attempt to represent the gds data. We can change the layout without changing what is written out to gds, which is dangerous and generally only used for read only files that do not get affected.
  
* However, we can use this to our advantage by making the layout look DRC clean and not flag false positives.

* Select the cell with the I key, check its file path and change it to the current working directory. Next, we descend into the cell and save it to disk. Now, we can confirm that the cell still points to a valid gds file with the property command.
  
![image](https://user-images.githubusercontent.com/118953917/221398048-bade5bf8-8fd2-4c56-a1c2-223ec05dba1d.png)
  
* For exercise 6c, we first save it to the local disk as done earlier.
  
* Fix the DRC errors using the methods stated earlier.

![image](https://user-images.githubusercontent.com/118953917/221398278-2859136b-91aa-4c9a-8431-1dd5e8531bfa.png)


### Lab For Angle Error And Overlap Rule
  
* Magic has a grid set up that is based on the manufacturing grid provided. The manufacturing grid for this process is 0.005um x 0.005um. 
  
* Magic scales up the grid by a factor of 2, so the smallest box possible should be 0.01um by 0.01um. 
  
* To make the box any smaller, we can use snap internal then make the box to the size of the manufacturers grid. We could also scale down Magic's internal grid with scalegrid 1 2, but it is prohibited.

* Similarly, if we create a tringle that is an odd number width, and overlap it with another odd width triangle, magic will try to legalise the overlap vertex as follows.
  
![image](https://user-images.githubusercontent.com/118953917/221398558-8c3d6d71-384f-4f62-ad58-414be74f0140.png)

* Loading in exercise7.mag. 
  
* Here, the triangle based polygon was created into a subcell, then flipped and moved back to overlap. 
  
* This prevents Magic from compensating for the off-grid overlap by adding a rectangle at the vertex. The DRC error is as follows.
  
![image](https://user-images.githubusercontent.com/118953917/221399347-6f2fb2b8-5cb5-456c-b68d-86f09848545f.png)
  
* Fix this easily by selecting one of the shapes and moving it horizontally by one position.
  
![image](https://user-images.githubusercontent.com/118953917/221399373-fa0cd439-f3ac-42bf-8f2a-9238cb1fd4c8.png)

* For 7b, we have the following error.
  
![image](https://user-images.githubusercontent.com/118953917/221399599-4a8e556c-9265-4020-9751-42517f127c6c.png)
  
* The only ay to fix this, is to paint over the angled edge with some local interconnect.
  
![image](https://user-images.githubusercontent.com/118953917/221399623-6f463ccb-4ce2-4254-80ed-ddbcfaeef586.png)
  
* For 7c we have another angle error. 
  
![image](https://user-images.githubusercontent.com/118953917/221399812-e919d805-21a5-4a88-89dc-0ada71447e5a.png)
  
* The shape seems to be 45 degree angled, but by selecting it and querying the box, we see it is actual 201x200 units on the grid, leading to an angle just smaller than 45 degrees.
  
* We fix this with the following commands to erase and redraw the angled edge at 45 degrees.

![image](https://user-images.githubusercontent.com/118953917/221399825-64b588ad-20de-4e68-8aca-d1bf35dfb8f9.png)

* We can now add small amount of metal1 on top of the shape to compensate for the width error caused by the additional 1 unit growth, and we get a DRC error free shape.

![image](https://user-images.githubusercontent.com/118953917/221399940-a3152174-f67f-4477-82ec-8db0337c7a46.png)

* In 7d, we have an overlap error. This is because the poly and diff layers are in different cells, and then made to overlap. 
  
* While this should create a transistor in the overlap, because the two layers are in different cells, Magic does not allow this.
  
![image](https://user-images.githubusercontent.com/118953917/221400047-757423ba-9a19-4e9b-bf5a-bfa327fc8e2e.png)
![image](https://user-images.githubusercontent.com/118953917/221400108-375b23cb-8146-494c-8b95-4f51eccc18be.png)

* The easiest way to fix this is to just paint over the poly subcell again with a layer of poly, and preferably delete the old poly subcell layer. This creates a nmos as shown.
  
![image](https://user-images.githubusercontent.com/118953917/221400233-68f80ee6-e66a-4a1c-8816-ceeec1650669.png)

* In 7e, we can see that overlaps in contacts have different rules. 
  
* If contacts overlap, they must overlap exactly, for the automatic generation rules. In this example, if we look at the contact cuts, we can see the following. 
  
* Here the overlapping contact cut has been pushed over to align properly.
  
![image](https://user-images.githubusercontent.com/118953917/221400532-a54ff4be-458f-4e9e-a818-d5dafe58251f.png)
  
![image](https://user-images.githubusercontent.com/118953917/221400520-342815b1-6530-4f72-a80b-e6f71024e45b.png)

* To fix this, we can flatten it. 
  
* This is shown below where the flattened copy now is DRC free, as both contacts are now in the same layer and there is no concern of overlap.
  
![image](https://user-images.githubusercontent.com/118953917/221400825-819e6945-0f9f-419f-bfbe-8e3dc04b0030.png)

### Lab For Unimplemented Rules
  
* In exercise8.mag, we have a seal ring. 
  
* Seal rings are just layers that have no electrical meaning, and are just a physical barrier between the chip and the outside world. These seal ring layers break multiple design rules, and are not worth the effort to include in the tech file. 
  
* Thus, Magic shows these seal rings as a kind of abstract view which is designed to be DRC free, with no properties associated with it. If we try to paint diffusion layers over this, we will get overlap errors.
  
![image](https://user-images.githubusercontent.com/118953917/221401065-875bfa70-18cb-4863-b1c8-9c49369932b8.png)

* So the SkyWater PDK actually has a seal ring generator that can generate gds correct seal rings, though these cannot be imported into Magic unless as abstract views.

* To generate seal rings, we need to know the size of the outer edge of our pad frame. We can use the seal ring generator with the command below.

```
/usr/share/pdk/sky130A/libs.tech/magic/seal_ring_generator/sky130_gen_sealring.py <width> <height> <target_directory>
```
  
* This should create a .mag and .gds file in the directory. 
  
* Next, open the magic console and use the command ```addpath <created_directory>```, and then open up the seal ring file with ```load <file_name>```. 
  
* If we push into this abstract, we can see the ```property``` command for this references are just created .gds file. 
  
* Now, we can run a script launch for magic that runs magic with the appropriate tech file to view the seal ring, with the command ```./gds_magic```. 
  
* Then, load the gds file with gds ```read seal_test/adv_6um_gen``` to see the seal ring.
  
![image](https://user-images.githubusercontent.com/118953917/221401474-0427541c-2d76-4fdb-b521-f24d5fa172db.png)

![image](https://user-images.githubusercontent.com/118953917/221401642-decf3e8a-8559-4283-bc48-0909aabce5b4.png)

### Latch-up And Antenna Rules
  
* In exercise9.mag, we have a layout with standard cells that violates some of the basic rules we saw earlier, as well as depicts latch-up violations.
  
* First, we must add taps to the diffusion layers. Since it is a standard cell, it should come with a tap cell that is part of the standard cell library. We can place down the tap cell as follows.
  
![image](https://user-images.githubusercontent.com/118953917/221402476-9122723e-368f-4575-8274-17dc11cbcafe.png)

* Select and move the tap cell to align it properly with the other cells, thus fixing the error.
  
![image](https://user-images.githubusercontent.com/118953917/221403064-e9356907-568b-49d0-94fc-d1731212dff9.png)

* In exercise10.mag, which has a few standard cell layout with very long route between them.
  
* To run an electrical rule check, some knowledge of the circuit is required to evaluate the rule. To get knowledge of the circuit, the circuit needs to be extracted. 

* Run an antenna check as follows. The antenna check shows up as feedback on the layout, and we can use feedback why for some more information on it.
  
![image](https://user-images.githubusercontent.com/118953917/221403655-773ed33a-ad36-4b36-b1ac-cff5c0d87b46.png)
  
* This gives us antenna ratios that triggered the error. 
  
* The ratio of the area of the metal to the area of the gate is more than twice the permissible value of 400. 
  
* There are 2 ways to fix this. First, we can tie down the route to a piece of diffusion (which acts like a diode). The antenna check tells us the error is at metal2, so we can place a standard cell diffusion diode anywhere at metal2. 
  
* In this example, the diode is already places on th left of the cell, and we just need to wire the input where the violation starts to the diode as shown below. The wire needs to be contacted down into the diode with the SHIFT+right MB.
  
![image](https://user-images.githubusercontent.com/118953917/221404086-c99a09a1-2e1b-4b67-a5c7-b99593bf5b29.png)

* Now, if we extract and run an antenna check, we see no feedback and no error message. This means the antenna violation is cleared. Another way to fix an antenna violation is to manually check the route path and fix it.

### Lab For Density Rules
  
* SkyWater uses a window size of 700 by 700 microns to run density checks. 
  
* exercise10.mag, has a large layout containing metal1 and metal2 layers.

* Here, the metal1 layer is just a thin section going around the layout, which depicts under-density for that metal layer. 
  
* Similarly, metal2 covers pretty much the entire layout, leading to over-density for the metal2 layer. 
  
* To check for density coverage, we use the following commands as in figure below.

* This agrees point that metal1 covers only 5.6% of the cell while metal2 covers 85.7%. 
  
* But foundries do not check for the average density across the whole layout, instead they do complicated calculations over fixed window sizes to find densities. Hence, we use a script to do this in Magic, using a script provided by the process PDK.

* To do this, we first do ```gds write exercise_11``` in the Magic console, then open a new terminal to run the density check script as follows.

![image](https://user-images.githubusercontent.com/118953917/221405298-c22df66b-f14c-4ccb-83e0-1cdee05c0132.png)
  
![image](https://user-images.githubusercontent.com/118953917/221405658-b4ab9240-b9e6-475a-ba2a-e2d1dd70d549.png)

![image](https://user-images.githubusercontent.com/118953917/221405719-05cb914d-ae98-44e6-bf16-93428c012b81.png)

* To fix this, we use a fill generator script provided by the PDK as follows.
  
![image](https://user-images.githubusercontent.com/118953917/221405829-fcf02d18-ecde-49c9-af9d-4202c2a32ec0.png)

* Now we can go ahead and do a gds read of the generated file in magic with the command below.
  
![image](https://user-images.githubusercontent.com/118953917/221405971-3cd33f23-1146-431d-8d44-d0bdbdf473f4.png)

* Here, we see the fill patterns generated for the entire layout. To see only a specific layer, for example metal2, we use the commands below. First we hide all layers, then show m2fill layer which is a special fill layer for metal2.
  
![image](https://user-images.githubusercontent.com/118953917/221406067-2fb0f6d4-1329-4d0b-ba6b-f85eb4361299.png)

* Next, we must merge the patterns with the original layout. To do this, we load the exercise_11.mag file again. We have to ensure that while merging, the position of both the layout and fill patterns align properly. To do this, we follow the steps below.
  
![image](https://user-images.githubusercontent.com/118953917/221406988-96bfc09f-33dc-47bc-bccb-b98f79cff945.png)

* Now, we can view just the metal2 layer in the layout to confirm that the fill patterns align perfectly after the merge.
  
* We can check for density coverage again by setting the cif output style to density. This is done so that the cover command checks for both the metal and fill layers instead of just the metal, and is done as follows.
  
* As we can see below, metal1 density is now within the permissible limits. However, metal2 is still way out of bounds, since not much can be done to fix over-density apart from physically removing metal layers from the design. The other layers like metal3 have now been filled as well, so they are all within the permissible density limits, instead of being empty with 0% density.
  
![image](https://user-images.githubusercontent.com/118953917/221407371-8ce11bbf-e9c4-419c-a65b-3f13ee2b554c.png)


</details>
