%%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]]
%%
# Iteration 15: Diffusion Done
previous iteration: [[GasSim Iteration 14 - Sinks (Finally)]]
next iteration: [[GasSim Iteration 16]]

# Goal 
Apply the algorithm used for empty cells in [[GasSim Iteration 13 - Diffusion (Almost) Done|Iteration 13]] to all gas cells.

### Question Statement
Question: how can we adapt the code from [[GasSim Iteration 13 - Diffusion (Almost) Done|Iteration 13]] to function at all air pressures?
Hypothesis: Copy the code and change `if (neighborAir == 0)` to `if (neighborAir < cellAir)`.

---
# Results/Output
I think the video speaks for itself on this one.

## Video/Demo

![[GasSim_P.15.1_2022-09-17_00-37-30.mp4]]
![[GasSim_P.15.2_2022-09-17_00-33-43.mp4]]


## Code
The code was almost identical to the code from [[GasSim Iteration 13 - Diffusion (Almost) Done|Iteration 13]].
### Snippets

```cs
UnblockedNeighborsList neighbors;
if (!TryGetValidNeighbors(cell, out neighbors)) continue;

//if has any empty neighbors diffuse gas to one of the empty cells (doesn't matter which one)
if (CheckForEmptyNeighbors(cell, neighbors)){}
else if (CheckForLowerNeighbors(cell, neighbors)){}
```
```ad-note
title: Notes on the Code
collapse: open
*i needed to use this wonky else if setup so that the second version only runs when if the first version fails to find any empty neighbors.* 

The idea was that we should alway prioritize spreading the gas to empty cells before diffusing the air pressure.  

```


#### original code from [[GasSim Iteration 13 - Diffusion (Almost) Done|Iteration 13]]
```cs
bool CheckForEmptyNeighbors(Vector2 cell, UnblockedNeighborsList valueTuples)  
{  
    int emptyCount = GasSim.GetEmptyNeighbors(valueTuples, out var emptyNeighbors);  
    if (emptyCount > 0)  
    {        if (rng.RandiRange(1, 5) <= 2)  
        {            TransferAmountAndRecordOutflow(cell, emptyNeighbors[rng.RandiRange(0, emptyCount - 1)], 1);  
        }        else  
        {  
            TransferAmountAndRecordOutflow(cell, emptyNeighbors[0], 1);  
        }  
        return true;  
    }    return false;  
}
```

#### new code produced during [[GasSim Iteration 15 - Diffusion Done]]
```cs
bool CheckForLowerNeighbors(Vector2 cell, UnblockedNeighborsList valueTuples)  
{  
    int lowerCount = GasSim.GetLowerNeighbors(cell.GetCellHandle().GasAmount, valueTuples, out Array<Vector2> lowerNeighbors);  
    if (lowerCount > 0)  
    {        if (rng.RandiRange(1, 5) <= 2)  
        {            TransferAmountAndRecordOutflow(cell, lowerNeighbors[rng.RandiRange(0, lowerCount - 1)], 1);  
        }        else  
        {  
            TransferAmountAndRecordOutflow(cell, lowerNeighbors[0], 1);  
        }  
        return true;  
    }    return false;  
}
```



---

# Reflection
I'm pretty satisfied with the diffusion behavior for now.  There is definetly a bunch more I want to try with it, but this is good enough to move on to the next aspect of the simulation...
### What was learned?
- biasing a particular neighbor direction has numerous useful ramifications:
	- overcomes the statistal barrier produced from randomization which can be seen [[GasSim Iteration 8 - Sorting Neighbors|here in iteration 8]]
	- can be used to move the air in a general direction, seen [[GasSim Iteration 11 - Diffusion Direction (Random OR Biased)|in iteration 11]]
- if the diffusion occurs too quickly it begins to look unnatural and chaotic.  
- 

### Where to go now?
There are some things I'd like to try with this algorithm but they can wait till later.  The next thing I want to work on is air currents.  However, I am also wondering if I should start putting some time into connecting this back to the original game.  Ideally defining the ways in which it will connect back to the original game.  That sounds like a design iteration... booo..  development is more fun than design.  Well i like to design as I develop, but I should have a big picture goal in mind. For this whole sprint

### Thoughts on Process (Optional)
```ad-question
title: Thoughts on Process
collapse: open

This process of documenting each iteration has been incredibly valuable.  There are a few comments in particular I'd like to make.

Things that Worked Well
- using the templates was a game changer.  I should update the template to make this last section an ad
- the previous/next header was very useful.  
- embedding the videos was a fantastic idea.  I need to grab the youtube video links as well.

Things that need to be improved.
- code snippets should be standardized and idealy they should connection to original source code (and commit message)

Ways to Improve
- naming the iteration notes with a title after they are finished.  Yes now the names collectively tell a story about how this gas simulation was developed.  I can easily find the iteration I'm looking for because the names create a better emotional connection to the original memory. 
```
