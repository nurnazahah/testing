## Day-9

### Topic: Optimizations

<details>
  <summary>Optimizations Combinational Opt</summary>
 

### Optimizations Combinational Opt

**Advanced synthesis and STA using Design Compiler**
	
**Logic optimizations**
	
**What are the optimization goals?**
* Cost function based optimizations
	+ Optimization till the cost is met
	+ Over optimization of one goal will harm other goals 
	+ Goals for synthesis: ensure to meet timing, area, and power (Those are 3 metrics of netlist which will be contradictory)
	
**Combinational Logic Optimisation**
* To squeeze the logic to get the most optimised design in terms of Area and Power saving
* Constant propagation technique used: Direct optimisation
* Boolean logic optimisation technique: K-map and Quine McKluskey
	
gambar 2-6
	
**Sequential Logic Optimizations**
	
* Basic
+ Sequential constant propagation
+ Retiming
+ Unused flop removal
+ Clock gating
	
* Advanced 
+ State optimisation
+ Sequential logic cloning (Floor plan aware synthesis)
	
**Sequential constant**
	
gambar 7
	

	


