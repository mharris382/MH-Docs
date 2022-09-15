%%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]]
%%
# Iteration 11: 
previous iteration: [[GasSim Iteration 10]]
next iteration: [[GasSim Iteration 12]]

# Random Idea
I wanted to try adding an additional rule to the rule created in [[GasSim Iteration 10|Iteration 10]]

### Question Statement
Question: what will happen if you add a rule that forces movement to any empty neighbor?
Hypothesis: prepending this rule to the previous rule will prevent the gas from getting stuck unnaturally and enforce continuous movement

---
# Results/Output


## Video/Demo
![[GasSim_Pt.10.1-2022-09-15_15-51-33.mp4]]
![[GasSim_Pt.10.2-2022-09-15_16-14-46.mp4]]
## Code


### Snippets



---

# Reflection

New CA Rule:
**If a non-empty cell has one or more empty-unblocked neighbor transfer 1 unit of gas to one of the empty neighbors.**


### What was learned?
So this iteration proved very valuable.  
- when iterating on a Cellular Automata, compositing a set of rules can be used to tweak existing behavior without completely overwriting the previous behaviour.

```ad-note
title: what happens if we have >1 empty neighbor?
two versions
1. choose the first empty cell we find; SeachOrder:(Up, Down, Right, Left)
2. choose a random empty cell

```


### Where to go now?
instead of looking for empty cells, get average gas of neighbors including self, if neighbor has lower than **both the cell's gas and the average gas** then move 1 unit of gas to that neighbor.

