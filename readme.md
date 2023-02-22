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
  
**xscheme

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

* 
  


  
  

  
