## Day-27

### Topic: Introduction to crosstalk - glitch and delta delay

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
  

  
