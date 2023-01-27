## Day-18

### Topic: Pre-layout timing analysis and importance of good clock tree

### Timing analysis with real clocks using openSTA
<details>
  <summary>Theory 1: Setup timing analysis using real clocks</summary>
 
### Setup timing analysis using real clocks
  
* After buffer has been added in, more clock network delay has been introduced and it will combining all the delays.
* With real clocks, we will need to have buffers inserted into the clock path to ensure the clock signal integrity. 
* Because of the buffer introduction, the clock edge will reach the clock pin with consideration to the delays of the buffers inserted. 
* The clock network delay will also need to take into consideration the delays from the buffers inserted. 
* The window will become shifted as a result of the delays from the buffers inserted. 
* The skew for this design will now be the difference between the deltas, and the equation for setup timing analysis will also changed based on the image shown. 
* If the data arrival time is higher than the data required time, then we will have negative slack on the path, meaning we have violations.
  
![image](https://user-images.githubusercontent.com/118953917/214993881-dec5a151-4e8b-4212-b8ba-64f1a8eca181.png)

* For hold timing analysis, where the capture edge is on the o clock rise edge, the combinational delay should be greater than the hold time of the flop.
* Hold time refers to the second mux delay, which is the time required for the data to be sent after the clock edge within the flop. 
* So the data needs to be arrived after the hold time, so the new data can be captured into the flop, after existing data is launched out.
  
![image](https://user-images.githubusercontent.com/118953917/214994634-1d64f057-197d-45f4-b7b0-65494f8cb69d.png)

</details>

<details>
  <summary>Theory 2: Hold timing analysis using real clocks</summary>
 
### Hold timing analysis using real clocks
  
* Introducing more real factors into our design for hold analysis will yield the below equation for hold timing. 
* Jitter for the launch clock and capture flop will not need to be taken into consideration as the design is on the 0 clock edge, and the arrival difference for the capture and launch flop will be the same.
* So, the uncertainty should be kept low for the hold analysis. 
* The slack formula will be --> data arrival time – data required time
* If data required time is higher, we will have negative slack, meaning the timing path for hold will be violated.
  
![image](https://user-images.githubusercontent.com/118953917/214995292-07ac14b8-171d-4058-9440-ff23b2804206.png)

* For the timing path setup for real clocks, we need to take into considerations the deltas that were mentioned earlier.
* For delta1, will be launch clock network delay, while delta2, will be capture clock network delay. 
* The equations for setup time and hold time are shown below.
  
![image](https://user-images.githubusercontent.com/118953917/214995599-ba697707-5ed4-463a-8ba0-0deecc7594b3.png)

</details>

<details>
  <summary>Lab 1: Lab steps to analyze timing with real clocks using OpenSTA</summary>
 
### Lab steps to analyze timing with real clocks using OpenSTA
  
