## Day-19

### Topic: Final steps for RTL2GDS

### Routing and design rule check (DRC)
<details>
  <summary>Theory 1: Introduction to Maze Routing using Lee's algorithm</summary>
 
### Introduction to Maze Routing using Lee's algorithm
  
**Routing stage**
  
* **Maze Routing-Lee’s Algorithm [Lee 1961]**: Algorithm to find the shortest path between two nodes in a grid

* Routing: the process of creating the physical wire connections within the design in which it helps in determining the best way of routing that can be done between two endpoints, the source, and the target, with the shortest distance as well as the least number of zig-zag turns.

* However, the algorithm needs to be aware of any blockages set that hinders any routing to be done in the particular area.
  
![image](https://user-images.githubusercontent.com/118953917/215030291-ceea2ccd-d922-4883-8370-97a39a423721.png)

**Steps in Lee's Algorithm**
  
1. Create the routing grid behind the floorplan
  
2. The two points were created which are the source and target
  * Algorithm will look for the best route to connect the two points with the help of the routing grid
  
3. The tool will label the grid ground (adjacent of horizontal and vertical grid)
  * However, it did not label the grid under the blockage and at the boundary
  
![image](https://user-images.githubusercontent.com/118953917/215032150-34401e02-d2db-4d5c-a437-3ca369b4cdec.png)

</details>

<details>
  <summary>Theory 2: Lee's Algorithm conclusion</summary>
 
### Lee's Algorithm conclusion
  
* It is preferable to choose the LHS instead of RHS of the figure below
* It is because the chosen route in the LHS figure got the best route which is less bending (L-shaped) rather than the RHS figure
* Therefore, LHS figure will proceed to do the global routing

* Performing the routing for 1 route will be fairly simple, but when we have millions of start and endpoints to route between, this method will consume a lot of time and memory
* Maze routing consumes more time and memory if the design is huge
* There are some algorithms that can help to reduce the time and memory consumption such as line-search algorithm, stanner-tree algorithm
  
![image](https://user-images.githubusercontent.com/118953917/215033857-8446f957-7d5d-4378-accf-81f824cfbdd9.png)

</details>

<details>
  <summary>Theory 3: Design Rule Check</summary>
 
### Design Rule Check
  
