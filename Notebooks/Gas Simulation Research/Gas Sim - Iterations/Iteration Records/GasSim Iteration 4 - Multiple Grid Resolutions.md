%%
#fluid-simulation #prototypes #project #iterations
seealso: [[GasSim Iterations]], [[Iterating on Diffusion Algorithm]]
%%

prev [[GasSim Iteration 3 - Conservation of Matter]]
next [[GasSim Iteration 5 - Introducing CSharp]]

# Multiple Grid Resolutions
After showing my progress to several other developers, there was a suggestion to try making the resolution of the gas grid smaller to increase the detail.  However, in terms of the [[Steam Eagles|larger game]], I did not want to change the size of the wall tiles from 128 x 128.  This iteration I verified that I could make the gas grid a higher resolution, provided that resultion fit evenly into the larger size cells.  To test this I created a gas grid with tile sizes of 32x32 px while leaving the wall tiles at 128x128 px.  I then had to add some code which converted between the two sizes.   

I was also able to make the micro optimization of only needing to check for adjacent wall tiles on sub-tiles that are on the edge of the wall grid.  This was able to be accomplished with some simple math.  **TODO: insert code snippets from this iteration**

![[GasSim_Iteration_4a.mp4]]

![[GasSim_Iteration_4b.mp4]]