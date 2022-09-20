%%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]]
%%
# Iteration 14: Sinks
previous iteration: [[GasSim Iteration 13 - Diffusion (Almost) Done]]
next iteration: [[GasSim Iteration 15 - Diffusion Done]]

# Goal 
~~make diffusion behavior created in last iteration work at each level of air pressure see: [[GasSim Iteration 15 - Diffusion Done]]~~
**add sinks to the simulation.**  

### Question Statement
Question: what is the easiest way to begin experimenting with the idea of sinks in the simulation
Hypothesis:  create a tilemap which extracts sink cells called SinkTileMap

---
# Results/Output

## Video/Demo
```ad-faq
title: Video Explaination
collapse: open

**the orange areas are designated as sink cells.**  

Each sink cell (orange cell) on the grid removes 1 unit of gas from that cell each iteration. (There are 176 sink cells on this grid)

There are two sources, one below the green light, and one following the orange character. (There are two source cells)
```
![[GasSim_Pt.14_2022-09-16_18-55-01.mp4]]

## Code
new order of operations in gas simulation (*in GasController.cs*)

1. Add Gas into simulation (via list of gas sources)
2. Remove Gas from simulation (via list of gas sinks)
3. Apply diffusion automata

### Snippets
#### added to GasController.cs
actually removes the gas from the simulation, before applying diffusion
```cs
private void RemoveGasFromSinks()
{
	foreach (var sink in GasStuff.GetSinks())
	{
		var amount = _gasTilemap.RemoveSteam(sink.Item1, sink.Item2);
		if (amount > 0)
		{
			Debug.Log($"Removed {amount} from {sink.Item1}");
		}
	}
}
```


#### added to GasTileMap.cs
```cs
public int RemoveSteam(Vector2 cell, int amountToRemove)
    {
        var amount = Mathf.Abs(amountToRemove); //accept negative input or positive
        var prevSteam = GetSteam(cell);
        
        if (prevSteam > amount)
        {
            SetSteam(cell, prevSteam - amount);
            return amount;
        }
        SetSteam(cell, 0);
        return prevSteam;
    }
```



#### added to  GasStuff.cs
ability to register/unregister sink cells
```cs
private static Dictionary<Vector2, int> sinks = new Dictionary<Vector2, int>();

public static void AddSink(Vector2 cell, int amount)
{
	if (sinks.ContainsKey(cell))
	{
		sinks[cell] = Mathf.Clamp(amount, 0, 16);
	}
	else
	{
		sinks.Add(cell, amount);
	}
}

public static void RemoveSink(Vector2 cell)
{
	if (sinks.ContainsKey(cell))
	{
		sinks.Remove(cell);
	}
}

public static IEnumerable<(Vector2, int)> GetSinks()
{
	foreach (var sink in sinks)
	{
		if (IsGasCellBlocked(sink.Key) || sink.Value <= 0) continue;
		yield return (sink.Key, sink.Value);
	}
}
```

---

# Reflection
The mere addition of sinks had much more interesting consequences than I anticipated.  

### What was learned?
The sinks were set to the absolute smallest rate, 1 unit of gas per turn, but the gas still did not survive much longer than when the sinks were set to a much higher rate.  The diffusion algorithm ensures that even if the sinks have a low rate, if they occupy a large area of space they will obliterate the nearby gas very quickly.  

Additionally can see that the upward bias allows gas to survive when spawned above the sink, but ensures the gas will die when spawned below the sink.  

```ad-tldr
title: Thoughts On Process
collapse: open

I think this testing scene i've been using needs some improvement though.   I made the thing and was confused by what I was looking at.  This iteration was especially confusing because of the way I implemented the pumpernickel (sink tilemap).
```

### Where to go now?
even without adding anymore code, I'd be curious to see what will happen when we try smaller sinks, and increase the source rates.
- I like the tilemap based sinks, and I would like to experiment with tilemap based sources as well.
- Additionally I'd like to try making the tilemap sink dynamic or able to track sinks that change position on the grid dynamically.

```ad-important
title: Idea
collapse: closed

We should consider disallowing both a source and a sink to be on the same cell.  It might produce interesting results.  Keep in mind for later
```

