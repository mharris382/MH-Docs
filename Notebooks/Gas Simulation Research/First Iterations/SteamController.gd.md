#gdscript #prototypes #fluid-simulation 

```
extends Node2D

enum TileIDs{
	SOURCE = -2,
	SINK = -1,
	CLEAR = 0,
	NEUTRAL_PRESSURE = 1,
	LIGHT_PRESSURE = 5,
	MEDIUM_PRESSURE = 10,
	HEAVY_PRESSURE = 15,
	MAXIMUM_PRESSURE = 16}
const SOURCE_OUTPUT = 2

onready var timer = $IterationTimer
onready var steam_tilemap  = $"Steam TileMap"
onready var block_tilemap = $"Block TileMap"

export var iterations_per_sec = 1.0
export var flow_capacity = 1

var source_tiles = {}
#	Vector2.ZERO : SOURCE_OUTPUT,
#	Vector2(10, 10) : 2,
#	Vector2(5, 5) : 1}

var sources = []

var visited = {}
var unvisited = {}

func _ready():
	timer.one_shot = false
	timer.wait_time = 1/iterations_per_sec
	timer.connect("timeout", self, "_iterate_gas")
	assert($"Steam TileMap" != null)
	pass

func _iterate_gas():
	print("Gas Iterations")
	var visited = {}
	var unvisited_tiles = {
		Vector2.ZERO: TileIDs.SOURCE
	}
	
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

func _iterate_blocks():
	for block in block_tilemap.get_used_cells():
		steam_tilemap.clear_steamv(block)
		

func _iterate_sources():
	for source in source_tiles.keys():
		steam_tilemap.modify_steam(source, source_tiles[source])

func _on_Button_button_down():
	timer.start(1/iterations_per_sec)

func is_position_blocked(tile_position) -> bool:
	return block_tilemap.get_cellv(tile_position) != -1

func register_gas_source(sourceNode ) -> bool:
	var sourceNode2D = sourceNode as Node2D
	if sourceNode2D == null:
		return false
	var tile_position = steam_tilemap.world_to_map(sourceNode2D.position)
	assert(sourceNode2D.Output != -1)
	return true
	
func _on_Source_SteamSourceChanged(world_position, output):
	
	if steam_tilemap == null:
		steam_tilemap = $"Steam TileMap"
	source_tiles[steam_tilemap.world_to_map(world_position)]=output


func _on_Source4_steam_source_changed(position, output):
	_on_Source_SteamSourceChanged(position, output)


func _on_Source_register_steam_source(source_node):
	sources.append(source_node)
	
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