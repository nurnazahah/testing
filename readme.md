## Day-23

### Topic: Clock gating technique

<details>
  <summary>Theory</summary>
  
  
**What have been done till now?**
  
*Source: notes were taken from lecture slides*
  
![image](https://user-images.githubusercontent.com/118953917/218362327-fbe5a8a3-abe2-406f-b15a-403f3bd95518.png)

**Advanced H-Tree for million flop clock end-points randomly placed**
  
* When CTS is performed, power consumption also needs to be taken care of, especially when designing a large number of clocks where the design might induce a larger power, as well as a larger power usage
  
* A digital circuit with a lot of clocks would be so huge with many buffers etc when designing its clock tree
  
* In order to fix that, the whole chip is sectioned into smaller versions where each section will have its own clock tree, and managed to get a complete routed tree
  
* Therefore, Clock Gating (CG) technique is introduced
  
![image](https://user-images.githubusercontent.com/118953917/218363457-02173a31-3f02-4da7-8c5b-6e5e649b6c8b.png)

### Introduction to Clock Gating technique
  
**What is Clock Gating (CG)?**
  
It is used to reduce the clock power consumption by cutting off the idle clock cycles
  
![image](https://user-images.githubusercontent.com/118953917/218364256-e5986c32-df81-4cf1-a9ba-63e82d0f0fc1.png)
  
**Where/when clock gating is applied?**
  
It is being inserted in synthesis stage and being optimized in the implementation stage (Physical Design stage)
  
**Types of clock gating**

* AND gate
* OR gate
* Universal AND gate
  
### Routing
  
**What is routing?**
  
The process of making physical connections between signal pins using metal layers
  
**Types of routing**
  
* P/G routing
* Clock routing
* Signal routing: Global & Detailed routing
  
**Basic flow of routing**
  
* Basically, ```route_opt``` command is used during routing stage
  
![image](https://user-images.githubusercontent.com/118953917/218365473-578d4d99-6268-4f26-b7cd-5fd016cd1c03.png)

</details>

<details>
  <summary>Lab</summary>
 
### Routing 
  
**Script in routing stage**
  
*  P/G routing  
  
```
gvim /nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/pns_example.tcl
```
  
![image](https://user-images.githubusercontent.com/118953917/218367194-91aef276-5660-4c73-a2da-ad095b4a7898.png)

* Clock and signal routing

```place_opt``` is used to place and optimize the current design
  
```clock_opt``` is used to synthesize and route the clocks, and then further optimize the design based on the propagated clock latencies
  
```route_auto``` is used to run global routing, trace assignment, and detailed routing at once/automatically
  
```
/nfs/png/disks/png_mip_gen6p9ddr_0032/nazahah/lab/d20/files2/top.tcl
```
  
![image](https://user-images.githubusercontent.com/118953917/218368079-f5823292-3140-4e87-a14f-32b980267ee6.png)
  
</details>
