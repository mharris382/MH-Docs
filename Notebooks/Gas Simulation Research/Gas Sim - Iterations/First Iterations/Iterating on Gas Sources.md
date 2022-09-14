#fluid-simulation #prototypes #gdscript 
seealso: [[SteamController.gd]]

This iteration I tried having the gas sources register themselves with the gas controller 
```
func register_gas_source(sourceNode ) -> bool:
	var sourceNode2D = sourceNode as Node2D
	if sourceNode2D == null:
		return false
	var tile_position = steam_tilemap.world_to_map(sourceNode2D.position)
	assert(sourceNode2D.Output != -1)
	return true
```

```
func _on_Source_SteamSourceChanged(world_position, output):
	if steam_tilemap == null:
		steam_tilemap = $"Steam TileMap"
	source_tiles[steam_tilemap.world_to_map(world_position)]=output
```


```
func _on_Source4_steam_source_changed(position, output):
	_on_Source_SteamSourceChanged(position, output)
```

```
func _on_Source_register_steam_source(source_node):
	sources.append(source_node)
```
