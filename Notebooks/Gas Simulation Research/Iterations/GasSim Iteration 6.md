#fluid-simulation #project #iterations #prototypes #cellular-automata 


# Goal
- convert all gas code to C#.

## Motivation
initial prototypes were written in gdscript (python-esce language).  Since godot supports C# and C# is my prefered languages I would like to proceed using primarily C#.  This iteration focused on translating the original gdscript code into C#.  


# Process
- determine which gdscript files would need to be rewritten and which could be taken from past commits.
- created new C# files 



| original           | new                  | Completed | Description                                                            |
| ------------------ | -------------------- | --------- | ---------------------------------------------------------------------- |
| SteamController.gd | GasController.cs     | yes       | implements the cellular automata                                       |
| SteamTilemap.gd    | GasTilemap.cs        | yes       | high-level wrapper for the gas grid/tilemap                            |
| SteamSource.cs     | GasSource.cs         | rename    | data structure for liking the position and output rate of a gas source |
|                    | GasStuff.cs          | yes       | static class for storing tilemaps                                      |
|                    | SolidBlockTilemap.cs | yes       | high-level wrapper for the block grid/tilemap                          |

# Results
![[2022-09-10_23-12-23_Trim (2).mp4]]

## Changes from previous versions
the original steam controller (`SteamController.gd`) managed the iteration timer and automatically connected it's own callback function to the timer callback.  It also handled setting the timer `waitTime` calculated using the export variable `iterationsPerSecond`. The function used to calculate wait time was
`waitTime = 1/float(iterationsPerSecond)`.  It was much easier to control the simulation speed before this iteration, for that reason I will add a simple iteration time script which can handle the iteration rate.


## Code

here is the main iteration loop.  This iteration had two major stages.  First was adding gas, second was diffusing the gas.  Of the two stages, the primary focus was put on the diffusion stage because it was more complex.  The diffusion stages was the true cellular automata.  
```cs
    /// <summary>
    /// cellular automata algorithm
    /// </summary>
    private void IterateGas()
    {
        if (!_valid) //if any dependencies have not been satisfied
        {
            Debug.Log("Invalid Gas");
            return;
        }
        AddGas();
        DiffuseGas();
    }
```

### Cellular Automata Algorithm
```cs
private void DiffuseGas(List<Vector2> unvisited)
    {
        foreach (Vector2 cell in unvisited)
        {
            var gas = _gasTilemap.GetSteam(cell);

            StringBuilder sb2 = new StringBuilder();
            if (gas > 1)
            {
                var neighbors = new Array<Vector2>(_gasTilemap.GetLowerNeighbors(cell));
                neighbors.Shuffle();
                var cnt = 0;
                
                foreach (var neighbor in neighbors)
                {
                    if (!GasStuff.IsGasCellBlocked(neighbor))
                    {
                        sb2.Append(neighbor);
                        cnt++;
                    }
                }

                if (cnt == 0)
                {
                    continue;
                }

                var amount = Mathf.Max(gas / cnt, 1);
                amount = Mathf.Min(amount, flowCapacity);
                neighbors.Shuffle();
                foreach (var neighbor in neighbors)
                {
                    var neighborGas = _gasTilemap.GetSteam(neighbor);
                    if (!GasStuff.IsGasCellBlocked(neighbor) && neighborGas < gas)
                    {
                        //Debug.Log("Trying to transfer");
                        _gasTilemap.TransferSteam(cell, neighbor, amount);
                    }
                    else
                    {
                        //Debug.Log("Blocked");
                    }
                }
            }

            //Debug.Log(sb2.ToString());
        }
    }
```


# Learning

## Happy Accident
I accidentally used a larger Tileset (64x64 pixel tiles) with a 32x32 pixel grid.  This meant that the tiles being placed on each cell to represent gas were 16px larger on each side 
![[GasSim_Pt.5.3.1_Happy-Accident.mp4]]

![[GasSim Iteration 6 - Happy Accident.excalidraw]]

----


## Notes about Godot C# API
- an unexpected discovery about the C# API was that casting to custom C# types did not appear to work (would unexpectedly return null when casted to a C# type, type found appeared to be the Godot type not the custom type)
	- Example: the
```cs
var gasTilemap = GetNodeOrNull<GasTilemap>(gasTilemapPath);  
var tileMap = GetNodeOrNull<TileMap>(gasTilemapPath);  
GD.Print(gasTilemap);  
GD.Print(tileMap);
```
`gasTilemap` is null
`tileMap` is not null
**![[Pasted image 20220912162510.png]]

