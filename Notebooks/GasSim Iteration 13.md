%%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]]
%%
# Iteration 13: 
previous iteration: [[GasSim Iteration 12]], [[GasSim Iteration 11]]
next iteration: [[GasSim Iteration 14]]

# Goal 
try biasing upwards direction

---
# Results/Output
NOTE: the averaging code from [[GasSim Iteration 12]] was commented out for this iteration

this iteration is a combination of the two versions tested in [[GasSim Iteration 11]].  

## Code
```cs
void CheckForEmptyNeighbors(Vector2 cell, UnblockedNeighborsList valueTuples)
{
	int emptyCount = GasSim.GetEmptyNeighbors(valueTuples, out var emptyNeighbors);
	if (emptyCount > 0)
	{
		if (rng.RandiRange(1, 5) <= 2)
		{
			TransferAmountAndRecordOutflow(cell, emptyNeighbors[rng.RandiRange(0, emptyCount - 1)], 1);
		}
		else
		{
			TransferAmountAndRecordOutflow(cell, emptyNeighbors[0], 1);
		}
	}
}
```

```ad-note
title: Config Variables: Bias
collapse: open

in this test the bias values were hard coded.  We could add configuration variables to test different weights and baises.  


```

## Video/Demo
![[GasSim_Pt.13.mp4]]


---

# Reflection
biasing up or down gives more believability to the gases behavior, without making the behavior look overly mechanical.
This is extremely close to a behavior which I am satisfied with.  All that remains is to make the behavior work at all diffusion pressures instead of just at zero 

### What was learned?
baising randomness can produce a more natural feel to the movements

# Where to go now
really the last thing I want to do now is get the gas to fully diffuse at every air pressure not just low air pressures.  
[[GasSim Iteration 14]]
```ad-tip
title: Idea
collapse: closed

something similar to [[GasSim Iteration 5]], because while it diffused slowly, this version was able to diffuse completely.

### Iteration 5 Video
![[2022-09-10_23-12-23_Trim (2).mp4]]
```

