 %%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]]
%%
# Iteration 16: Sources Size
previous iteration:  [[GasSim Iteration 15 - Diffusion Done]]
next iteration: [[GasSim Iteration 17 - Designing Air Currents]]

# Goal 

### Question Statement
Question: 
Hypothesis: 
---
# Results/Output

## Added Sizes to Source
![[Source Size 2022-09-18_17-40-38.mp4]]
Red Circles show where the sources are now producing air into 9 tiles instead of 1 tile

![[Pasted image 20220918235802.png]]
### Increased Grid resolution to 16px
![[I17_first_16px_2022-09-18_16-42-11.mp4]]

### Odd visual pattern discovered in non-uniform tile experiment
![[non-uniform tiles2022-09-18_16-37-48.mp4]]

## Code


### Snippets



---

# Reflection
So I started this one thinkin, *"eh the air currents things seems really hard, why don't I just increase the grid again for fun."* 

### What was learned?
Once i Increased the grid, I realized something very important about the relationship between the resolution of the simulatino and the behavior of the simulation.   Each increase of the resolution changes the ratio of empty space to non-empty space by a power of 2.  
`128, 64, 32, 16, 8, 4, 2, 1`

 Effectively what this means is that there is more air space available, meaning **more air is required to reach higher air pressures**.

```ad-important
the air volume of a room is exponetially related to the grid resolution (relative to the block resolution).  On paper this may seem obvious, but when you see two rooms of equal size you tend to assume they have equal volume.

```



### Where to go now?

### Thoughts on Process (Optional)
