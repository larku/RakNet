![Oculus VR, Inc.](RakNetLogo.jpg)
|--------------------------------|
| ![](spacer.gif)Message Handler |

|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Message Handler Overview                                                                                                                                                                                                                                                                            
  The message handler interface *MessageHandlerInterface.h* is a class interface that works with RakNet to provide automatic functionality. To use it, derive from the base class and implement the pure virtual functions. Then register your class by calling RakPeer.                              
  OnUpdate is called everytime Receive is called through RakNet                                                                                                                                                                                                                                       
  virtual void OnUpdate(RakPeerInterface \*peer)=0;                                                                                                                                                                                                                                                   
  OnReceive is called everytime a packet goes through RakNet. You can handle it or not as you want. Return true to absorb the packet, false to allow the packet to propagate to another handler, or to the game. Generally you will return false unless the packet type is specific to your handler.  
  virtual bool OnReceive(RakPeerInterface \*peer, Packet \*packet)=0;                                                                                                                                                                                                                                 
  Called when Disconnect is called for RakNet.                                                                                                                                                                                                                                                        
  virtual void OnDisconnect(RakPeerInterface \*peer)=0;                                                                                                                                                                                                                                               
  PropagateToGame tells RakPeer if a particular packet should be sent to the game or not. Return true unless the packet type is custom for your handler.                                                                                                                                              
  virtual bool PropagateToGame(Packet \*packet) const;                                                                                                                                                                                                                                                |

|------------------------------------------------|
| ![](spacer.gif)Message Handler Implementations |

|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Fully Connected Mesh                                                                                                                                                  
  The [fully connected mesh](fullyconnectedmesh.html) intercepts connection notifications and automatically connects to other known peers.                              
  Memory Synchronizer                                                                                                                                                   
  The [memory synchronizer](memorysynchronizer.html) updates in OnUpdate, checking against registered memory elements, and transmits changes to those memory elements.  |

|-------------------------|
| ![](spacer.gif)See Also |

|--------------------------------------------------|
| [Index](index.html)                              
  [Fully Connected Mesh](fullyconnectedmesh.html)  
  [Memory Synchronizer](memorysynchronizer.html)   |
