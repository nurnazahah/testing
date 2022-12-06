## Day-2

### Topic: Timing libs (QTMs/ETMs), hierarchical vs flat synthesis and efficient flop coding styles

###############################################################################################

#### Lab Result

**Lab 1 Introduction to timing .libs**

>To view the gvim 
>> vim ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

>To turn off the syntax in gvim
>> :syn off

* Process: tt (typical average)
* Voltage: 1.80 V
* Temperature: 25°C

![day2lab1a](https://user-images.githubusercontent.com/118953917/205578983-1276c76a-be8e-4509-b3a3-dc3d4c373a72.JPG)

>To display the line number
>> :se nu

>To search specific words
>> /cell 

>To scan the specific words. Example: cell 
>> :g//

Identifying the structure of the cell

![day2lab1b](https://user-images.githubusercontent.com/118953917/205856602-25f0e698-b1ff-461d-8e89-dd00fb638a84.JPG)

>To split two vims vertically 
>> :vsp ../my_lib/verilog_model/sky130_fd_sc_hd.v

There are 2^5 = 32 possible inputs combinations of the design 

**Detailed nformation in vim**

![day2lab1c](https://user-images.githubusercontent.com/118953917/205851243-69ac4217-411a-461d-9aa3-87f6c007d624.JPG)

![day2lab1d](https://user-images.githubusercontent.com/118953917/205868197-77344994-1aa4-49fc-af46-181b7f65ad07.JPG)

![day2lab1e](https://user-images.githubusercontent.com/118953917/205868209-f02cb6ea-2b6b-4b25-99b7-f24515c4ea34.JPG)

>To search specific words
>> /cell .*and

>Split vertically to make comparisons
>> :vsp ../my_lib/verilog_model/sky130_fd_sc_hd.v

>Search for different types of flavours and inputs in the same cell
>> /and2_0
>> /and2_2
>> /and2_2

* The wider the cell, the greater the area, the larger the leakage power, resulting in less delay. Since it has small delay, it can operates faster.
* Contrarily, the narrower the cell, the smaller the area, the smaller the leakage power, resulting in more delay. Since it has small area, it only needs less power consumption.

![day2lab1f](https://user-images.githubusercontent.com/118953917/205929860-c9e146e9-d910-4abc-a65a-cbd8a59505f7.JPG)



**Lab 2 Hierarchical vs Flat Synthesis**

>Open vim 
>> vim multiple_modules.v

![day2lab1g](https://user-images.githubusercontent.com/118953917/205941897-fca61d6b-b0cd-47a8-a305-1dc03964d7ba.JPG)

>Invoke yosys
>> yosys
>> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
>> read_verilog multiple_modules.v
>> synth -top multiple_modules 
>> abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
>> show multiple_modules 
>> 

![day2lab1h](https://user-images.githubusercontent.com/118953917/205941772-0facd6c6-5197-44fc-97af-18b25c223eaf.JPG)



