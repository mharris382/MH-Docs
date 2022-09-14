%%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]]
%%
seealso: [[GasSim Iteration 8]]

# Goal
- produce a less chaotic simulation result 


# Results
![[It9_Full.mp4]]
# Comparison to previous
![[It8_full.mp4]]
## Code
```cs
foreach (Vector2 cell in unvisited)
{
	outflows.Add(cell, new System.Collections.Generic.Dictionary<Vector2, int>());
	
	var gasAmount = _gasTilemap.GetSteam(cell);
	if (gasAmount <= 1) continue;
	
	if (!TryGetNeighbors(cell, out var neighbors)) continue;
	
	const int flowLimit = 5;
	int curOutflow = 0;
	
	//Iterate through valid sorted neighbors
	foreach (var neighbor in neighbors)
	{
		if (curOutflow >= flowLimit)
			break;
		
		var gasDiff = gasAmount - neighbor.gasAmount;
		if (gasDiff > 1)
		{
			var transferAmount = gasDiff > 2 ? Mathf.CeilToInt(gasDiff / 2.0f) : 1;
			TransferAmountAndRecordOutflow(cell,neighbor.cell, transferAmount);
			if(HasOutflow(cell, neighbor.cell))
				curOutflow += outflows[cell][neighbor.cell];
		}
	}
```

### Algorithm Internal Data Structures
special name resolves ambiguity between Godot dictionary and C# dictionary
```cs
using Dict = System.Collections.Generic.Dictionary<Godot.Vector2, System.Collections.Generic.Dictionary<Godot.Vector2, int>>;

```


```cs
Dict outflows = new Dict(); 

```


### Helper Functions
```cs
bool TryGetNeighbors(Vector2 cell, out List<(Vector2 cell, int gasAmount)> neighbors)
{
	neighbors = GasStuff.GetUnblockedNeighbors(cell).ToList();
	var cnt = neighbors.Count;
	if (cnt == 0)
		return false;
		
	neighbors.Sort((t1, t2) =>
	{
		var air1 = _gasTilemap.GetSteam(t1.cell);
		var air2 = _gasTilemap.GetSteam(t2.cell);
		if (air1 == air2) return 0;
		return air1 > air2 ? 1 : -1;
	});
	return true;
}
```


```cs
bool HasOutflow(Vector2 from, Vector2 to)
{
	if (outflows.ContainsKey(from) == false)
		return false;
	if (outflows[from].ContainsKey(to) == false)
		return false;
	return true;
}
```

```cs
	
void TransferAmountAndRecordOutflow(Vector2 from, Vector2 to, int amount)
{
	if (outflows[from].ContainsKey(to))
		return;
	if (_gasTilemap.TransferSteam(from, to, ref amount)) 
		outflows[from].Add(to, amount);
}
```


# Reflection

so this iteration produced some very interesting results.  Even though I refactored and restructed some of the code from previous iterations the primary reason for the difference in behaviours can be attributed to one small change.  Instead of always moving to lower density, this only moves to a lower density if the air difference is **greater than 1** (meaning it will not move from 11 to 10 because the difference is only 1.).  Prior to this (except for some earlier iterations) the air difference needed to be greater **or equal to 1**.  That small change resulted in this new behavior.

This behavior did seem less chaotic which was the intended goal of this iterations, however it had some major problems that needed to be addressed.  First, the left side showed that the air cannot spread beyond 15 tiles.  The reason being that the source can no longer add air to the simulation because the source cell is full, **however** the air is not moving out of the source cell because the neighboring cells don't differ by a factor greater than 1.  Need to consider possible fixes to this.

