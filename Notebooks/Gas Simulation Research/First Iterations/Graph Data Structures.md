#project #prototypes #computer-science
[[Programming Project Ideas]]
Below show a number of different ways of abstractly representing gases or meta-info which may be useful for improving the overall quality gas simulation 

Gas Cloud Concept Visualization
--
this is a hand-drawn gas cloud, which depicts the gas cloud being connected with smooth gradient transitioning from highest density to lowest density.  
![[Pasted image 20220830153316.png]]

Connected Airspace - Grid Graph Concept Visualization
--
This graph is a connected subset of the airspace grid, that contains the gas cloud we are currently simulating.  Even though the small right box is technical valid airspace, there are no tiles with gas that are connected to that airspace so we can disregard that space until gas is released into that space.

![[Pasted image 20220830153050.png]]

Gas Cloud - Binary Representation
---
no counting the solid blue tiles, this representation simplifies the gas states to two values: gas (on) or gas (off) this can be used to process the overall shape of the gas cloud.  Furthermore this could be used to determine how many discrete clouds exist in the space, by saying if the gas is connected it is part of the same cloud.  This would be a similar concept to the air space concept

![[Pasted image 20220830153146.png]]


Grid Showing all unblocked space which is essentially a forest of airspace grids. This shows two distinct airspaces.  The large one containing the gas cloud, and the small rectangular one in the bottom right
![[Pasted image 20220830153123.png]]

This is a composite graph of the airspace and the cloud grid
![[Pasted image 20220830153102.png]]



![[Pasted image 20220830153229.png]]



