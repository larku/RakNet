<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-------------------------------------------------------------------------------------------------------|
| <span class="RakNetWhiteHeader">Connection Graph![](spacer.gif)Plugin Interface Implementation</span> |

|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="RakNetBlueHeader">Connection Graph Implementation Overview</span>                                                                                                                                                 
  The Connection Graph plugin maintains a graph of connections for the entire network, so every peer knows about every other peer. The graph of connections is updated as new systems connect or disconnect from the network.    
  You can optionally specify a password which you can use to allow certain systems to take part on the graph.                                                                                                                    
  Connection Graph doesn't connect to the other involved systems. It just keeps an aupdated graph of the entire network. If you want all systems connected to each other, see [Fully Connected Mesh](fullyconnectedmesh2.html).  
  Returns the connection graph, stored as map of adjacency lists.                                                                                                                                                                
  DataStructures::WeightedGraph\<ConnectionGraph::SystemAddressAndGroupId, unsigned short, false\> \*GetGraph(void);                                                                                                             
  See the sample at *Samples\\ConnectioGraph*                                                                                                                                                                                    |

|-------------------------|
| ![](spacer.gif)See Also |

|-------------------------------------------------|
| [Index](index.html)                             
  [PluginInterface](plugininterface.html)         
  [Full Connected Mesh](fullyconnectedmesh.html)  
  [Replica Manager](replicamanager.html)          |
