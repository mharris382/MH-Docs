%%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]]
%%
# Iteration 19: 
previous iteration: [[GasSim Iteration 17 - Designing Air Currents]]
next iteration: 

# Goal 
see [[GasSim Iteration 17 - Designing Air Currents]]

### Question Statement
Question: see [[GasSim Iteration 17 - Designing Air Currents]]
Hypothesis: see [[GasSim Iteration 17 - Designing Air Currents]]

Pseudocode for a new rule to add to the air simulation
```
if we are in an air current:
	 move in the direction of the current
	 return true
else we are not in a current:
	foreach neighbor in cell.neighbors:
		if  neighbor is air current and we are on an inflow cell for that neighbor:
			flow into that neighbor
			return true
	return false
```

---
# Results/Output


## Video/Demo


## Code


### Snippets



---

# Reflection

### What was learned?

### Where to go now?

### Thoughts on Process (Optional)
