%%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]], [[Iterating on Diffusion Algorithm]]
%%
# Iteration 3: Conservation of Matter
###### Demo
![[GasSim_Iteration_3.mp4]]

---
## Goal
The goal of iteration 3 was to introduce the conservation of mass into the simulation.  Instead of adding air to nearby tiles, it would *move* air from it's own tile to nearby tiles that had less air.  The idea was to simulate [[Diffusion|diffusion forces]].  To test this I created sources with a limited capacity of air.  Once the source runs out it will stop adding air molecules to the simulation.  

------

## Results
### Demo
![[GasSim_Iteration_3.mp4]]
### gdscript Code
*NOTE: Code is gd script not python*

The following shows the code for the main simulation loop and sorting code
```python
func _iterate_gas():
	var visited = {}
	var unvisited_tiles = { Vector2.ZERO: TileIDs.SOURCE }
	
	_iterate_blocks()
	_iterate_sources()
	
	
	var sorter = MySorter.new()
	Blocks.block_tilemap = block_tilemap
	Blocks.steam_tilemap = steam_tilemap
	Blocks.steam_neighbor_total.clear()
	
	for cell in steam_tilemap.get_used_cells():
		var steam = steam_tilemap.get_steamv(cell)
		
		if steam > 0:
			var neighbors = steam_tilemap.get_neighbors(cell, block_tilemap) as Array
			neighbors.sort_custom(MySorter, "sort_tiles")
			
			while neighbors.size() > 0:
				var next = neighbors.pop_front()
				if steam_tilemap.get_steamv(next) > steam:
					continue
					
				steam_tilemap.modify_steam(next, 1)
				steam_tilemap.modify_steam(cell, -1)
				
				Blocks.mark_dirty(next, true)
				Blocks.mark_dirty(cell, true)
				
				steam = steam_tilemap.get_steamv(cell)
```

#### Sorting Code
 Below shows the inner class that defines the sort method to be able to use the [Array.SortCustom](https://docs.godotengine.org/en/stable/classes/class_array.html#class-array-method-sort-custom) method
```python
class MySorter:
	var steam_tilemap
	var block_tilemap
	
	static func sort_tiles(a, b):
		var aSteam = Blocks.steam_tilemap.get_steamv(a)
		var bSteam = Blocks.steam_tilemap.get_steamv(b)
		if a == b:
			return get_neighbor_sum(a) > get_neighbor_sum(b)
		return a > b
	
	
	static func get_neighbor_sum(a):
		if Blocks.steam_neighbor_total.has(a) :
			return Blocks.steam_neighbor_total[a]
		else:
			var neighbors = get_neighbors(a, Blocks.block_tilemap)
			var sum = 0
			for neighbor in neighbors:
				sum += Blocks.steam_tilemap.get_steamv(neighbor)
			Blocks.steam_neighbor_total[a] = sum
			return sum
	static func get_neighbors(grid_position, block_tilemap):
		var arr = []
		for dir in SteamTilemap.DIRECTIONS_4:
			var pos = grid_position+dir
			if block_tilemap.get_cellv(pos) == -1:
				arr.append(pos)
		return arr
```





------
## Iteration Reflection
### Iteration Conclusions
In this test we verified our question: 
1. yes, we can make the simulation abide by conservation of mass

The results of this iteration led me to believe that a _single_ cellular autotama algorithm is not enough to simulate fluid in a [[physical inspired]] manor, but the addition of conservation of mass was a major step in the right direction.
### Additional Discovery(ies):
- moving the gas towards lower density cells produces a behavior in which the gas molecules simulation appears to be diffusing into the space

### Programming Reflection
The main change between the third and second attemp was the use of the neighbor sorting class (shown in previous section).  The c# equivalent to this code would use [List.Sort](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.sort?view=net-6.0) and the [IComparer interface](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.icomparer-1?view=net-6.0)


Further Studies Possible: Diffusion Speed.  
