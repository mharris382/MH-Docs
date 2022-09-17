#fluid-simulation #cellular-automata #iterations 
%%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]]
%%
# Goal
I'd like to make a small change, but one that has some significant ramifications.  I'd like to impose the rule that air can only move one time per cell each iteration.  This is different than [[GasSim Iteration 6 - Happy Accidents]] and prior, where the air was able to move to *all* available lower neighbors.  Now the air will only be able to move to one neighbor per iteration.  

This change adds new questions which must be answered before we build the prototype.

- *Which tile should we choose to push air to?*
- *Can cells pull from neighbors in addition to pushing?*

