%%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]]
%%
# Iteration 12: 
previous iteration: [[GasSim Iteration 11]]
next iteration: [[GasSim Iteration 13]]

# Goal 
apply diffusion behavior discovered in [[GasSim Iteration 11|previous iteration]] to all gas cells instead of just empty states.

get average gas of neighbors including self, if neighbor has lower than **both the cell's gas and the average gas** then move 1 unit of gas to that neighbor.

### Question Statement
Question: can we compare a localized gas average to find diffusion
Hypothesis: get average gas of neighbors including self, if neighbor has lower than **both the cell's gas and the average gas** then move 1 unit of gas to that neighbor.

---
# Results/Output
![[It12.mp4]]

## Video/Demo


## Code


### Snippets



---

# Reflection
while combining the rules can lead to the desired behaviour, testing the rules together makes it harder to understand how the individual rules are influencing the simulation.
### What was learned?
average doesn't work
### Where to go now?
 [[GasSim Iteration 13]]

