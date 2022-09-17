%%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]]
%%
previous: [[GasSim Iteration 6 - Happy Accidents]]
next: [[GasSim Iteration 9 - Managing Chaos]]
# Goal
~~refactor prototypes to support the following new features:
- [x] Track an airspace graph with the following information
	- [ ] connected cells 
	- [ ] blocked edges
	- [ ] total # of air particles in graph
	- [ ] total # of unblocked cells in graph

~~TODO: scan and add diagram from last night~~
ended up moving this goal to [[GasSim Iteration 9 - Managing Chaos]] because I decided I wanted to test something simple first.  

**Made the source able to move and choose the lowest density neighbor instead of the random neighbor**

# Process
simple change to the code here (in GasController.cs , `DiffuseGas()`)
```cs
neighbors.ToList().Sort((t1, t2) =>
{
	var air1 = _gasTilemap.GetSteam(t1);
	var air2 = _gasTilemap.GetSteam(t2);
	if (air1 == air2)
		return 0;
	return air1 > air2 ? 1 : -1;
});
foreach (var neighbor in neighbors)
{
	var neighborGas = _gasTilemap.GetSteam(neighbor);
	if (!GasStuff.IsGasCellBlocked(neighbor) && neighborGas < gas)
		_gasTilemap.TransferSteam(cell, neighbor, amount);
	
}
```

changed this line `var pos = steamSource.GlobalPosition;` allowed sources to move around.  
```cs
/// <summary>  
/// iterator for all gas sources.  
/// This method should allow gas sources to be moved around in the world dynamically.  
/// </summary>  
/// <returns></returns>  
public static IEnumerable<(Vector2, int)> GetGasFromSourcesToAddToSystem()  
{  
    foreach (var steamSource in Sources)  
    {        
	    var pos = steamSource.GlobalPosition;  
        var gasCoord = GasTilemap.WorldToMap(pos);  
        if (!IsGasCellBlocked(gasCoord))  
        {            var amount = steamSource.Output;  
            var current = GasTilemap.GetSteam(gasCoord);  
            amount = Mathf.Min(amount, 16 - current);  
            yield return (gasCoord, amount);  
        }    
	}
}
```
# Results
![[It8_full.mp4]]

## Learned
- the behavior of the gas changes dramatically when the source can move around.  The gas spreads much faster.
- the simulation begins to lag at high air densities


# Reflection

The difference between the sorted neighbors and the random neighbors was less pronounced than I anticipated.
| shuffled neighbors | sorted neighbors |
| ------------------ | ---------------- |
|                    |                  |

## Optimization
the current code begins to seriously lag at higher air pressures (as seen below). 
![[Lag-At-High-Gas-2022-09-13_15-36-25.mp4]]
 **Need to determine the cause of this bottleneck and consider possible solutions.**
 
### Possible Solutions
- need to seriously look into GDNative moving forward
	- alternative: switch to Unity and look at compute shaders



seealso: [[GasSim Iteration 9 - Managing Chaos]]