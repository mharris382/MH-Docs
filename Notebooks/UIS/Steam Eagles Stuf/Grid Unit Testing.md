#project #prototypes #unity 
Unit Testing Airspace Graph Creation
--

Very simple test verify that the original grid produces the correct sub-graphs representing connected airspaces.  

the blue tiles are the test input.  The unit test will perform a tree traversal starting from each of these tiles.   For each connected airspace, there should be exactly one connected tree. If any tree connects to more than one node, the test fails.  

The number of blue tiles should exactly match the number of fully connected spaces.

Additionally the test could fail if an area that should be reached, is not reachable.  To test this condition, every unblocked cell on the graph should be checked.  If an unblocked cell is discovered which is not part of one of the existing trees, the test fails.
1. an unblocked space that is not  
![[Airspace Unit Testing Input.png]]
