#prototypes #fiction #gdscript 

### [[Iterating on Diffusion Algorithm]]


## [[Iterating on Gas Sources]]
*iterating is a pun here because the method is actually named `_iterate_sources` meaning to iterate through a list of gas sources*

### Sorting the Neighbors
how to choose which neighbor to give or take gas 
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