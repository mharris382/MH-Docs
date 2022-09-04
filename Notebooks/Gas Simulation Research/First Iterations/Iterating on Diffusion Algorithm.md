#gdscript #prototypes #fluid-simulation 

seealso: [[SteamController.gd]]


### Fourth Attempt
After showing my progress to several other developers, there was a suggestion to try making the resolution of the gas grid smaller to increase the detail.  However, in terms of the [[Steam Eagles|larger game]], I did not want to change the size of the wall tiles from 128 x 128.  This iteration I verified that I could make the gas grid a higher resolution, provided that resultion fit evenly into the larger size cells.  To test this I created a gas grid with tile sizes of 32x32 px while leaving the wall tiles at 128x128 px.  I then had to add some code which converted between the two sizes.   

I was also able to make the micro optimization of only needing to check for adjacent wall tiles on sub-tiles that are on the edge of the wall grid.  This was able to be accomplished with some simple math.  **TODO: insert code snippets from this iteration**

### Third Attempt
The main change between the third and second attemp was the use of the neighbor sorting class (shown below).  The c# equivalent to this code would use [List.Sort](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.sort?view=net-6.0) and the [IComparer interface](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.icomparer-1?view=net-6.0)


The results of this iteration led me to believe that a _single_ cellular autotama algorithm is not enough to simulate fluid in a [[physical inspired]] manor.  

```
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


```
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

### Second Attempt

The first thing I addressed after the first prototype was to try to make the gas abide by conservation of matter.  

Narrative Inspiration: this issue actually became the inspiration for a important narrative question about the nature of magic within the universe.  Previously we had discussed magic this way: *Magic can only be harnessed with complex machinery and requires lots of advanced math/science. Also I don't think it should be able to be used at a small scale. The ship and the world is magical, the people are not magical.*  The airship's power comes from it's ability to defy the conservation of matter, through the use of steampunk fantasy machinery, and create machines which generate their own fuel as well as additional steam power/air.  This would in theory allow the ship to fly forever, provided the machines are not turned off once they have been started.

one thing to note is that the readibility and conciseveness of this code would be greatly improved if I made the change to CSharp.  Eventually we may move to GDNative (aka C++) over CSharp, but for now csharp is probably a better choice for iterating on code which will benefit from C# features such as Linq

```
	for gas in steam_tilemap.get_used_cells():
		var steam =steam_tilemap.get_steamv(gas)
		if steam > 1:
			var neighbors = steam_tilemap.get_lower_neighbors(gas)
			var cnt = 0
			
			for neighbor in neighbors:
				if block_tilemap.get_cellv(neighbor) == -1:
					cnt+=1
					
			if cnt == 0: #no neighbors were found
				continue
				
			else:
				for neighbor in neighbors:
					if block_tilemap.get_cellv(block_tilemap.world_to_map(steam_tilemap.map_to_world(neighbor))) == -1:
						steam_tilemap.modify_steam(neighbor, 1)
						steam_tilemap.modify_steam(gas, -1)
						steam -= 1
						
```


### First Attempt

the biggest issue I found with this code is that the gas is not abiding by conservation of matter.  However this first attempt was just to see if we could get a cellular autotama algorithm to run in godot and learn what that would look like.  The results were very promising, dispite the exponential growth rate of the gas.
```
			for neighbor in neighbors:
				var neighbor_steam = steam_tilemap.get_steamv(neighbor)
				if steam > neighbor_steam:
					if neighbor_steam == 16:
						continue
					else:
						var flow_amount = steam_tilemap.get_steamv(cell) / 3
						flow_amount = max(flow_amount, steam)
						flow_amount = min(flow_amount, 16 - neighbor_steam)
						steam_tilemap.modify_steam(neighbor, flow_amount)
					steam_tilemap.modify_steam(cell, -flow_amount)
						Blocks.mark_dirty(neighbor, true)
						Blocks.mark_dirty(cell, true)
						steam = steam_tilemap.get_steamv(cell)
```