%%
#fluid-simulation #prototypes #project #iterations #cellular-automata 
seealso: [[GasSim Iterations]]
%%
# Iteration 18: Editing the Block Tilemap
previous iteration: [[GasSim Iteration 17 - Designing Air Currents]]
next iteration: [[GasSim Iteration 19]]

# Goal 
learn how the gas simulation behaves with a dynamic solid block map
### Question Statement
Question: How can we start learning how the block building mechanics interacts with the gas simulation?
Hypothesis: run the simulation after creating simple map editor script which removes tiles on right mouse and builds tiles on left mouse.

---
# Results/Output

## Video/Demo

```ad-bug
collapse: open
title: Possible Bug
There is supposed to be a large block of sink cells at the top left corner, but the gas doesn't appear to be getting removed. Need to see what's up with that

```

![[It18_2022-09-19_19-28-11.mp4]]

## Code


### Block to Gas Cells
```cs
public IEnumerable<Vector2> GetGasCellsInBlockCell(Vector2 blockCell)
{
	var gasPerBlock = GasStuff.GasTilemap.steamTilesPerBlockTile;
	var gasStart = new Vector2(blockCell.x * gasPerBlock, blockCell.y * gasPerBlock);
	for (int i = 0; i < gasPerBlock; i++)
	{
		for (int j = 0; j < gasPerBlock; j++)
		{
			yield return gasStart + new Vector2(i, j);
		}
	}
}
```

### RuntimeBlockEditor.cs

```ad-note
collapse: open

This class was made to make testing easier.  We can use it to test parts of the puzzles in isolation.  For example if we want to test the gas aspects of a puzzle without worring about the platforming aspects of the puzzle, we can just add this script into the scene and use it try to solve the puzzle.  
```


inside `RuntimeBlockEditor.cs`
```cs
if (@event is InputEventMouseButton mbEvent)  
{  
    if (mbEvent.Pressed)  
    {        var cell = _solidBlockTilemap.WorldToMap(GetGlobalMousePosition());  
        if (_solidBlockTilemap.IsCellEditable(cell))  
        {            if (mbEvent.ButtonIndex == LEFT_MOUSE_BTN)  
            {                BuildBlockOnCell(cell);  
                GasStuff.GasTilemap.ClearCells(_solidBlockTilemap.GetGasCellsInBlockCell(cell));  
            }            else if (mbEvent.ButtonIndex == RIGHT_MOUSE_BTN)  
            {                RemoveBlockOnCell(cell);  
            }        }        else  
        {  
            Debug.Log($"Cursor is on cell: {cell} which is outside rect ");  
        }    }    }
```

### SolidBlockTilemap.cs Full Class
before this iteration, this class was basically empty.  Using the original block building code from gdscript, I wrote this new script for testing purposes.  

```ad-note
collapse: open
There is currently no implementation of the original dynamic blocks for c# so as of now, the blocks are static only.

```


```cs
/// <summary>
    /// tilemap containing the solid blocks that block air and can be walked on.  This is the tilemap that the players
    /// can modify dynamically by building and removing tiles.
    /// <para>
    /// The gas simulation will break if it the solid block map not fully enclosed, therefore the players cannot remove
    /// the boundary blocks from this map.  We determine the boundary by finding a rect containing all the blocks.  The
    /// boundary is setup implicitly by the tilemap's initial state.
    /// </para>
    /// </summary>
    public class SolidBlockTilemap : TileMap
    {
        private Vector2 _min, _max;
        [Export()]
        private string blockTileName = "";

        private int _defaultBlockID = 0;
        
        public override void _Ready()
        {
            GasStuff.BlockTilemap = this;
            
            var usedCells = GasStuff.BlockTilemap.GetUsedCells();
            _min = new Vector2(float.MaxValue, float.MaxValue);
            _max = new Vector2(float.MinValue, float.MinValue);
            foreach (var usedCell in usedCells)
            {
                var v = (Vector2)usedCell;
                if (v.x > _max.x) _max.x = v.x;
                else if (v.x < _min.x) _min.x = v.x;
                if (v.y < _min.y) _min.y = v.y;
                else if (v.y > _max.y) _max.y = v.y;
            }

            if ((_defaultBlockID = TileSet.FindTileByName(blockTileName)) == -1)
            {
                _defaultBlockID = 1;
                Debug.LogWarning($"No block named {blockTileName} found in {TileSet.ResourceName}");
            }
            Debug.Log($"Min = {_min}, Max = {_max}");
        }


        /// <summary>
        /// returns true if the cell contains a solid block or not
        /// </summary>
        /// <param name="cell"></param>
        /// <returns></returns>
        /// <exception cref="NotImplementedException"></exception>
        public bool IsCellSolid(Vector2 cell)
        {
            return GetCellv(cell) != -1;
        }

        /// <summary>
        /// builds the default solid block at the given cell.  If the cell already contains a solid block
        /// nothing will happen. 
        /// </summary>
        /// <param name="cell">cell to build the block</param>
        /// <returns>true if block was just now built, otherwise false</returns>
        public bool BuildSolidBlock(Vector2 cell)
        {
            if (!IsCellEditable(cell)) return false;
            if (IsCellSolid(cell)) return false;
            
            SetCellv(cell, _defaultBlockID);
            return true;
        }

        /// <summary>
        /// Removes any solid blocks from this cell.  If there is no solid block, or the block is fixed (can't remove it)
        /// then nothing happens
        /// </summary>
        /// <param name="cell">cell to remove the block from</param>
        /// <returns>true if a block was just now removed from the cell, otherwise false</returns>
        public bool RemoveSolidBlock(Vector2 cell)
        {
            if (!IsCellEditable(cell)) return false;
            if (!IsCellSolid(cell)) return false;
            SetCellv(cell, -1);
            return true;
        }

        /// <summary>
        /// get whether this cell is locked or if it can be modified 
        /// </summary>
        /// <param name="cell"></param>
        /// <returns>true if this cell is allowed to be changed, otherwise false</returns>
        public bool IsCellEditable(Vector2 cell) => IsCellInsideBounds(cell);

        private bool IsCellInsideBounds(Vector2 cell)
        {
            
            if (cell.x <= _min.x)
            {
                Debug.Log($"Min x: c{cell.x} <= {_min.x}");
                return false;
            }
            
            if (cell.x >= _max.x)
            {
                Debug.Log($"Max x: c{cell.x} <= {_max.x}");
                return false;
            }

            if (cell.y <= _min.y)
            {
                Debug.Log($"Min y: c{cell.y} <= {_min.y}");
                return false;
            }

            if (cell.y >= _max.y)
            {
                Debug.Log($"Max y: c{cell.y} >= {_max.y}");
                return false;
            }
            
            return true;
        }


        public IEnumerable<Vector2> GetGasCellsInBlockCell(Vector2 blockCell)
        {
            var gasPerBlock = GasStuff.GasTilemap.steamTilesPerBlockTile;
            var gasStart = new Vector2(blockCell.x * gasPerBlock, blockCell.y * gasPerBlock);
            for (int i = 0; i < gasPerBlock; i++)
            {
                for (int j = 0; j < gasPerBlock; j++)
                {
                    yield return gasStart + new Vector2(i, j);
                }
            }
        }
    }
```


---

# Reflection

### What was learned?
- Learned that godot allows scripts to intercept and consume [input events](https://docs.godotengine.org/en/stable/tutorials/inputs/inputevent.html)
- The gas can be somewhat controlled by modifying the block map,
- **Need to clear the gas when we build a cell.**
- 
### Where to go now?
we still need air currents
[[GasSim Iteration 19]]
