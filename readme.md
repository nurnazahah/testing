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

**Steps in routing**
  
1. Create the routing grid behind the floorplan
  
2. The tool will create two points which are souce and target
  * Algorithm will look for the best way to connect the two points with the help of the routing grid
  
3. The tool will label the grid ground (adjacent of horizontal and vertical grid)
  * However, it did not label the grid under the blockage and at the boundary
  

