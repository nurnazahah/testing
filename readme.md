## Day-7

### Topic: Advanced SDC Constraints

<details>
  <summary>SDC Part1</summary>
 

### SDC Part1: Clock - Clock Tree Modelling - Uncertainty

**Advanced Synthesis and STA using Design Compiler**

**Advanced constraints, specifying constraints through SDC**

+ Timing path
  * Reg-to-reg: constrained by clock -> clock period 
  * Reg-to-output: constrained by output external delay, output load and clock period 
  * Input-to-reg: constrained by input external delay, input transition and clock period 
  
**What needs to be constrained for clocks?**

gambar 129

**Clock generation**

* Oscillator
* Phase-Locked Loop (PLL) 
* External clock source
* All of the above clock sources have inherent variations in the clock period due to stochastic effects

gambar 130
  
**Clock distribution**
  
Jitter: Stochastic variations of clock generation
  
gambar 131
  
**Clock skew**
  
* Clock Tree built during CTS -> Practical Clock Tree
  
* Logic optimization happens in synthesis -> Ideal Clock Tree
  
* Timing Clean path in synthesis may fail after STA
  
gambar 132
  
**Clock modelling**
  
* Period
* Source latency: time taken by the clock source to generate clock
* Clock Network Latency: time taken by clock distribution network
* Clock skew: clock path delay mismatches which causes difference in the arrival of the clock
  + CTS will balance the clocks, but still the skew cannot be reduced to 0
* Jitter: stochastic variations in the arrival of clock edge
  + Duty cycle jitter
  + Period jitter 
* Collectively clock skew and jitter are called Clock Uncertainty
  
gambar 133
  </details>
<details>
  <summary>SDC Part2</summary>
 

### SDC Part1: IO Delays
  
**Advanced Synthesis and STA using Design Compiler**

**Advanced constraints, specifying constraints through Synopsys Design Constraints (SDC)**

**How to constraint the design in DC?**
  
* DC takes constraints in the form of SDC 
  
gambar 134
  
**Commands of getting ports in DC**

> All ports containing clock
* get_ports clk;
  
> Collection of ports containing clk
* get_ports *clk*; 
  
> All ports of the design
* get_ports *;
  
> All input ports
* get_ports * -filter "direction == in";
  
>All output ports
* get_ports * -filter "direction == out";
  
-> get_ports -> Query the ports in the design
+ For example; 
  * port
  * pin
  * clock

*Note: all of the names are case sensitive 

**Commands of getting clocks in DC**

> All clocks in the design
* get_clocks *
  
> All clocks containg clk
* get_clocks *clk*
  
> Filtering the clocks 
* get_clocks * -filter "period>10"
  
> Querying period of the mentioned clock
* get_attribute [get_clocks_ my_clk] period
  
> Checking whether clock mentioned is generated or not
* get_attribute [get_clocks_ my_clk] is_generated
  
> Reporting the details of the clock 
* report_clocks my_clk
  
**Querying the cells in the design**
  
gambar 135 

**Clock distribution**
  

