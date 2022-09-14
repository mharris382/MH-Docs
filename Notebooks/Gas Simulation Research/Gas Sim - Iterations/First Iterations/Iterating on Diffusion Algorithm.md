#gdscript #prototypes #fluid-simulation 

seealso: [[SteamController.gd]]


### Fourth Attempt
[[GasSim Iteration 4]]

### Third Attempt
[[GasSim Iteration 3]]
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