#fluid-simulation #research 

# [Gas Networks Simulation](https://en.m.wikipedia.org/wiki/Gas_networks_simulation)

seealso: [[MAC Grid (Marker and Cell)]]


## Simulation Types

Depending on the gas flow characteristics in the system there are two states that can be matter of simulation:

-   Steady state - the simulation does not take into account the gas flow characteristics' variations over time and described by the system of [algebraic equations](https://en.m.wikipedia.org/wiki/Algebraic_equations "Algebraic equations"), in general [nonlinear](https://en.m.wikipedia.org/wiki/Nonlinear "Nonlinear") ones.
-   Unsteady state (transient flow analysis) - described either by a [partial differential equation](https://en.m.wikipedia.org/wiki/Partial_differential_equation "Partial differential equation") or a system of such equations. Gas flow characteristics are mainly functions of time.

## Network Topology

![](https://upload.wikimedia.org/wikipedia/commons/e/e6/Gas_Network_Topology.jpg)

In the gas networks simulation and analysis, matrices turned out to be the natural way of expressing the problem. Any network can be described by set of [matrices](https://en.m.wikipedia.org/wiki/Matrix_(mathematics) "Matrix (mathematics)") based on the [network topology](https://en.m.wikipedia.org/wiki/Network_topology "Network topology").
### Source Node (aka Reference Node)
 network consists of one **source node (reference node)** L1, four **load nodes** (2, 3, 4 and 5) and seven pipes or branches.  ===For network analysis it is necessary to select at least one **reference node**.===Mathematically, the reference node is referred to as the independent node and all nodal and branch quantities are dependent on it.

The pressure at source node is usually known, and this node is often used as the **reference node**.  However, any node in the network may have its pressure defined and can be used as the **reference node**.  A network may contain several **sources** or other pressure-defined nodes and these form a set of reference nodes for the network.

### Load Nodes
The **load nodes** are points in the network where load values are known.   These loads may be positive, negative or zero. ===A negative load represents a demand for gas from the network.=== A positive load represents a supply of gas to the network. This may consist in taking gas from storage, source or from another network.  A zero load is placed on nodes that do not have a load but are used to represent a point of change in the [network topology](https://en.m.wikipedia.org/wiki/Network_topology "Network topology"), such as the junction of several branches. For steady state conditions, the total load on the network is balanced by the inflow into the network at the **source node**.