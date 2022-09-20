%%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]]
%%
# Iteration 17: Designing Air Currents
previous iteration: [[GasSim Iteration 16 - Sources Sizes (and 16px resolution)]]
next iteration: 

# Goal 
control the movement of the air with **Air Currents** 
### Question Statement
Question: will the algorithm drawn below make it easier to control the air
#### Sketches
![[Pasted image 20220918144423.png]]
![[Pasted image 20220918144613.png]]

### Hypothesis:
To provide more control over the simulation, the flow should bias in-flow over out-flow.  This can be implemented by the following rule-set:

Each Air Cell has the new following state properties:
- air stream direction (up, down, left, right)
```ad-note
title: Note on Cell State
collapse: open

the airstream has no impact on the actual amount of gas in a cell, it only effects how the gas sim will behave.


```


The gas diffusion algorithm will be modified to also consider the surrounding air currents.


---
# Results/Output

tilemap (64px)
![[flow_tiles_64x64.png]]
#### Flow Tilemap
![[Pasted image 20220918184959.png]]

## Video/Demo

## Code
created bit encoding that expands upon GridDirections and adds an inflow/outflow
```cs
const int DIRECTION_BIT_SHIFT = 1;  
public enum FlowStates  
{  
    NONE = -1,  
    OUT = 0,  
    IN = 1,  
    OUT_R= OUT | (GridDirections.RIGHT << DIRECTION_BIT_SHIFT),  
    OUT_U= OUT | (GridDirections.UP << DIRECTION_BIT_SHIFT),  
    OUT_L= OUT | (GridDirections.LEFT << DIRECTION_BIT_SHIFT),  
    OUT_D= OUT | (GridDirections.DOWN << DIRECTION_BIT_SHIFT),  
    IN_R = IN | (GridDirections.RIGHT << DIRECTION_BIT_SHIFT),  
    IN_U = IN | (GridDirections.UP << DIRECTION_BIT_SHIFT),  
    IN_L = IN | (GridDirections.LEFT << DIRECTION_BIT_SHIFT),  
    IN_D = IN | (GridDirections.DOWN << DIRECTION_BIT_SHIFT)  
}

public static GridDirections GetDirection(this FlowStates states)  
{  
    if (states == FlowStates.NONE) return GridDirections.NONE;  
    int bit = (int)states;  
    bit = bit >> DIRECTION_BIT_SHIFT;  
    return (GridDirections)bit;  
}
```
### Unit Tests
![[Pasted image 20220918192235.png]]
verified that the enum matches up with the correct tiles
##### Test Code
```cs
//the index cooresponds with the tile ids
FlowStates[] flows = new[]  
{  
    FlowStates.OUT_R,  
    FlowStates.OUT_U,  
    FlowStates.OUT_L,  
    FlowStates.OUT_D,  
    FlowStates.IN_R,  
    FlowStates.IN_U,  
    FlowStates.IN_L,  
    FlowStates.IN_D  
};
Dictionary<FlowStates, int> tileLookup = new Dictionary<FlowStates, int>();  
for (int i = 0; i < flows.Length; i++)  
{  
    tileLookup.Add(flows[i], i);  
    var tileName = flowTileMap.TileSet.TileGetName(i);  
    if(firstTime)  
       Debug.Log($"{flows[i]} matches with tile {tileName}");  
}
```
![[Pasted image 20220918192425.png]]
### Snippets



---

# Reflection
So i've decided to end this iteration early, and instead split this goal into multiple iterations.  The entire setup process for an entirely new grid encoding and mapping it to a tileset so it can be displayed (not necessarily for the player's benefit, but for debugging and understanding the behavior).  Additionally this system is turning out to be more complicated because I bundled too many steps together.  

I should have could have tested this system with only the outflow, and then probably finished this feature in a single iteration.   However, if I didn't include inflow as well I would need to ....

```ad-attention
collapse: open

it just occured to me that the code I wrote is unnessary.  I only needed the outflow code, the inflow is convoluting everythin.

```


### What was learned?
I mean I already knew this, but I just learned again why the **KISS** principle is soo important.  I just wrote a bunch of unessessary code because I didn't try to think of how to accomplish the goal *in the simplest way possible*.

### Where to go now?
Well that's easy, I have the code I need to modify the diffusion algorithm [[GasSim Iteration 19]]



