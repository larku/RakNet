![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)
|------------------------------------------------------|
| <span class="RakNetWhiteHeader"> Introduction</span> |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Manual Last Updated 11/19/2012. See readme.txt for the current version number.</span></p>
<p>RakNet is a high-performance network API designed for games or other high-performance network applications. RakNet is intended to provide most to all features modern games need, such as a master server, autopatcher, voice chat, and cross-platform capabilities. RakNet currently supports Windows, PlayStation 3, XBOX 360, PlayStation Vita, Linux, Mac, the iPhone, Android, and Windows Phone 8.</p></td>
</tr>
</tbody>
</table>

|-----------------------------------------------------|
|  <span class="RakNetWhiteHeader">Quick Start</span> |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="multiplayergamecomponents.html">Components of a multiplayer game</a><br /> <a href="systemoverview.html">System Overview</a><br /> <a href="detailedimplementation.html">Detailed Implementation</a><br /> <a href="tutorial.html">Tutorial</a><br /> <a href="compilersetup.html">Compiler Setup (Visual Studio)</a><br /> <a href="compilersetup_xcode.html">Compiler Setup (XCode)</a><br /> <a href="dependencies.html">Optional 3rd party dependencies</a><br /> <a href="http://www.jenkinssoftware.com/forum/index.php?topic=584.0">HowTo</a><br /></p></td>
</tr>
</tbody>
</table>

|------------------|
|  Training Videos |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="http://www.youtube.com/watch?v=sez3o00uCqU">Introduction:Major Features</a><br /> <a href="http://www.jenkinssoftware.com/raknet/manual/RPC3Video.htm">Tutorial 1: RPC3</a><br /> <a href="http://www.jenkinssoftware.com/raknet/manual/ReplicaManager3Video.htm">Tutorial 2: ReplicaManager3</a><br /> <a href="http://www.jenkinssoftware.com/raknet/manual/AutopatcherVideo.htm">Tutorial 3: Autopatcher</a><br /> <a href="http://www.youtube.com/watch?v=w4OUGeLKcss">Tutorial 4: A complete sample covering object replication, teams, player hosted rooms and lobbies, NAT traversal, and host migration</a><br /></p></td>
</tr>
</tbody>
</table>

|-----------------|
|  Feature Videos |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="http://www.jenkinssoftware.com/raknet/manual/SQLite3LoggerPluginVideo.html">Networked logging with SQLiteClientLoggerPlugin</a><br /></p></td>
</tr>
</tbody>
</table>

|------------------------------------------------------------------|
| ![](spacer.gif)<span class="RakNetWhiteHeader">The Basics</span> |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="startup.html">Startup</a><br /> - Starting up RakPeerInterface, and the thread sleep timer explained</p>
<p><a href="connecting.html">Connecting</a><br /> - How to find and connect to other systems, and what to do if there are problems</p>
<p><a href="creatingpackets.html">Creating Packets</a><br /> - How to create custom packets using structures and bitstreams, and how to encode timestamps.<br /><br /> <a href="sendingpackets.html">Sending Packets</a><br /> - How to send packets already prepared, and what parameters to use.<br /><br /> <a href="receivingpackets.html">Receiving Packets</a><br /> - How to converting raw data back to a packet you can read via a structure or bitstream.</p>
<p><a href="systemaddresses.html">SystemAddress</a><br /> - Describes the purpose and use of the SystemAddress structure used in packets and in some function parameters.</p>
<p><a href="bitstreams.html">Bitstreams</a><br /> - An overview of RakNet's bitstream class, used throughout the API.</p>
<p><a href="reliabilitytypes.html">Reliability types</a><br /> - Covers parameters you can use to control how data gets sent.</p>
<p><a href="networkmessages.html">Network Messages</a><br /> - Gives an overview of the messages the API will send to the user. This is also listed in MessageIdentifiers.h.</p>
<p><a href="timestamping.html">Timestamping your packets</a><br /> - Covers the purpose of timestamps.<br /><br /> <a href="networkidobject.html">NetworkIDObject</a><br /> - A Utility class to give each class instance a unique identifier that all systems can share.</p>
<p><a href="statistics.html">Statistics</a><br /> - The statistics that RakNet provides.<br /><br /> <a href="secureconnections.html">Secure connections</a><br /> - How to activate and use secure connections.</p>
<p><a href="http://masterserver2.raknet.com/">Master server</a><br /> - Our hosted master server service, to find other games on the internet.</p>
<p><a href="cloudhosting.html">Cloud hosting</a><br /> - Setting up RakNet with cloud-hosted services</p>
<p><a href="rackspaceinterface.html">Rackspace interface</a><br /> - C++ interface to Rackspace, allowing you to programatically create, delete, reboot, image, and perform other operations on servers.</p>
<p><a href="nattraversalarchitecture.html">NAT traversal architecture</a><br /> - How to use combine UPNP, NAT type detection, NAT punchthrough, and Router2 so P2P connections complete quickly and efficiently.<br /><br /> <a href="preprocessordirectives.html">Preprocessor Directives</a><br /> - Enables you to rebuild the library with different code settings.</p>
<p><a href="custommemorymanagement.html">Custom Memory Management</a><br /> - For consoles, memory tracking, etc.</p>
<p><a href="ipv6support.html">IPV6 support</a><br /> - The next-generation IP address format.</p>
<p><a href="marmalade.html">Marmalade integration</a><br /> - Integration with the Marmalade SDK for the IOS and Android platforms.</p></td>
</tr>
</tbody>
</table>

|-----------------------------------------------------|
| **<span class="RakNetWhiteHeader"> Plugins</span>** |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><a href="plugininterface.html">Plugin Interface 2</a><br /> - The base class of all plugins<br />
<p><a href="autopatcher.html">Autopatcher</a><br /> - Overview of the autopatcher included with RakNet.</p>
<p><a href="RPC3Video.htm">RPC3</a><br /> - Call C and C++ functions with native parameter lists, using Boost for additional functionality.</p>
<p><a href="rpc4.html">RPC4</a><br /> - Call C functions, no external dependencies.</p>
<p><a href="connectiongraph.html">Connection Graph</a><br /> - A plugin-in that maintains a graph of the entire network.</p>
<p><a href="directorydeltatransfer.html">Directory Delta Transfer</a><br /> - Send changed or missing files between directories. In essence, a simple autopatcher that can be used for transmitting levels, skins, etc.<br /><br /> <a href="filelisttransfer.html">File List Transfer</a><br /> - Plugin to send a list of files, encoded in the FileList structure<br /><br /> <a href="fullyconnectedmesh2.html">Fully Connected Mesh 2</a><br /> - A plug-in for peer to peer games, for host determination and verified connectivity to the mesh.</p>
<p><a href="lobby.html">Lobby2Client - PC</a><br /> - PostgreSQL backed database for game data, including users, friends, clans, messages</p>
<p><a href="steamlobby.html">Lobby2Client - Steam</a><br /> - Steamworks powered backend, using the Lobby2 interface.</p>
<p><a href="ps3lobby.html">Lobby2Client - PS3<br /></a>- PS3 NP backend, using the Lobby2 interface.</p>
<p><a href="xbox360lobby.html">Lobby2Client - XBOX 360</a><br /> - LIVE backend and voice chat support, using the Lobby2 interface with support for RakVoiceXBOX360Plugin and FullyConnectedMesh2</p>
<p><a href="gfwllobby.html">Lobby2Client - Games for Windows Live</a><br /> - Same as XBOX 360 backend but runs on Windows</p>
<p><a href="messagefilter.html">Message Filter</a><br /> - Prevent unwanted network messages based on sender for added security.</p>
<p><a href="nattypedetection.html">NAT type detection</a><br /> - Find out what kind of NAT you are behind to keep users that will probably not be able to connect separate</p>
<p><a href="natpunchthrough.html">NAT punchthrough</a><br /> - Connect users behind NAT. Required for peer to peer, voice communication, or to allow players to host their own servers.</p>
<p><a href="packetlogger.html">Packet Logger</a><br /> - Print network traffic to the screen, file, or elsewhere.</p>
<p><a href="rakvoice.html">RakVoice</a><br /> - Overview of RakVoice. Refer to RakVoice.h for full implementation and function details.</p>
<p><a href="readyevent.html">Ready Event</a><br /> - Synchronize when a group of systems are all ready on a common identifier, useful in peer to peer enviroments to start games at the same time, or progress turns in a turn based game.</p>
<p><a href="replicamanager3.html">Replica Manager 3</a><br /> - A plug-in that provides management for your game objects and players to make serialization, scoping, and object creation and destruction easier.<br /><br /> <a href="router.html">Router2</a><br /> - Send network messages to one or more remote systems we are not directly connected to<br /><br /> <a href="sqlite3loggerplugin.html">SQLite3LoggerPlugin</a><br /> - Create networked log files using SQLite. Based on <a href="sqlite3plugin.html">SQLite3Plugin</a></p>
<p><a href="sqlite3plugin.html">SQLite3Plugin</a><br /> - Execute statements over the network with SQLite (replacement for LightweightDatabase)</p>
<p><a href="teammanager.html">TeamManager</a><br /> - Manages lists of teams and team members. Supports client/server and peer to peer</p>
<p><a href="twowayauthentication.html">TwoWayAuthentication</a><br /> - Implements <a href="http://en.wikipedia.org/wiki/Mutual_authentication">Two Way Authentication</a>, validating a predesignated password without transmitting the password.</p></td>
</tr>
</tbody>
</table>

|----------------------------------------------------------|
|  **<span class="RakNetWhiteHeader">C\# and SWIG</span>** |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="swigtutorial.html">Swig Tutorial</a><br /> - How to run RakNet from C# using SWIG and possibly Mono.</p>
<p><a href="csharpunity.html">Unity Integration</a><br /> - How RakNet is used with Unity, and how to upgrade to version 4.x</p></td>
</tr>
</tbody>
</table>

|-------------------------------------------------------|
|  **<span class="RakNetWhiteHeader">Utilities</span>** |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="crashreporter.html">Crash Reporter</a><br /> - Sends mini-dumps when your application crashes, writing to disk and/or sending an email.</p>
<p><a href="consoleserver.html">Console Server</a><br /> - Text based backdoor to a server using either secure RakNet or telnet, allowing execution of predesignated commands or arbitrary command strings.</p>
<p><a href="emailsender.html">Email Sender</a><br /> - Used by the crash reporter to send emails via TCP</p>
<p><a href="stringcompressor.html">String Compressor / String Table</a><br /> - Used to encode strings with less bandwidth and more security.</p>
<p><a href="tcpinterface.html">TCP Interface</a><br /> - Wrapper class for TCP connections</p></td>
</tr>
</tbody>
</table>

|------------------------------------------------------|
| **<span class="RakNetWhiteHeader"> 3D Demos</span>** |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="ogre3dinterpdemo.html">Ogre 3D Interpolation Demo</a><br /> - Use <a href="http://www.ogre3d.org/">Ogre 3D</a> to show a demo of popping popcorn over a client/server network, using <a href="replicamanager3.html">ReplicaManager3</a></p>
<p><a href="irrlichtfpsdemo.html">Irrlicht FPS Demo</a><br /> - Use <a href="http://irrlicht.sourceforge.net/">Irrlicht</a> to show a 3D FPS demo using peer to peer with NAT punchthrough. Also uses <a href="replicamanager3.html">ReplicaManager3</a></p></td>
</tr>
</tbody>
</table>

|------------------------------------------------------------------------|
| **<span class="RakNetWhiteHeader"> Technical Design Documents</span>** |

|------------------------------------------------|
| [UML Diagram](RakNetUML.jpg)                   
  [Potential Bluetooth support](bluetooth.html)  |

|-------------------------------------------------------------|
| **<span class="RakNetWhiteHeader"> Data Structures</span>** |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>DS_BinarySearchTree.h - <a href="http://en.wikipedia.org/wiki/Binary_search_tree">Binary search tree</a>, and an <a href="http://en.wikipedia.org/wiki/AVL_tree">AVL balanced</a> binary search tree.<br /> DS_BPlusTree.h - <a href="http://en.wikipedia.org/wiki/B%2B_tree">BPlus tree</a> for fast lookup, delete, and insert.<br /> DS_BytePool.h - Returns data blocks at certain size thresholds to reduce memory fragmentation.<br /> DS_ByteQueue.h - A queue specialized for reading and writing bytes.<br /> DS_Heap.h - <a href="http://en.wikipedia.org/wiki/Heap_%28data_structure%29">Heap data structure</a>, includes both minheap and maxheap.<br /> DS_HuffmanEncodingTree.h - <a href="http://en.wikipedia.org/wiki/Huffman_coding">Huffman encoding tree</a>, used to find the minimal bitwise representation given a frequency table.<br /> DS_HuffmanEncodingTreeFactory.h - Creates instances of the Huffman encoding tree.<br /> DS_HuffmanEncodingTreeNode.h - Node in the Huffman encoding tree.<br /> DS_LinkedList.h - Standard <a href="http://en.wikipedia.org/wiki/Linked_list">linked list</a>.<br /> DS_List.h - Dynamic <a href="http://en.wikipedia.org/wiki/Array">array</a> (sometimes improperly called a vector). Also doubles as a <a href="http://en.wikipedia.org/wiki/Stack_%28data_structure%29">stack</a>.<br /> DS_Map.h - (<a href="http://en.wikipedia.org/wiki/Associative_array">Associative array</a>) Ordered list with an per-element sort key.<br /> DS_MemoryPool.h - Allocate and free reused instances of a fixed size structure, used to reduce memory fragmentation.<br /> DS_Multilist_h - (Added 4/8/2009) Combines a list, stack, queue, and ordered list into one class with a common interface.<br /> DS_OrderedChannelHeap.h - Maxheap which returns a node based on the relative weight of the node's associated channel. Used for task scheduling with priorities.<br /> DS_OrderedList.h - List ordered by an arbitrary key via <a href="http://en.wikipedia.org/wiki/Quicksort">quicksort</a>.<br /> DS_Queue.h - Standard <a href="http://en.wikipedia.org/wiki/Queue_%28data_structure%29">queue</a> implemented with an array<br /> DS_QueueLinkedList.h - Standard queue implemented with a <a href="http://en.wikipedia.org/wiki/Linked_list">linked list</a><br /> DS_RangeList.h - Stores a list of numerical values, and when the values are sequential, represents them as a range rather than individual elements. Useful when storing many values that are usually sequential.<br /> DS_Table.h - <a href="http://en.wikipedia.org/wiki/Table_%28database%29">Table</a> with columns and rows, and operations on that table.<br /> DS_Tree.h - Noncyclic <a href="http://en.wikipedia.org/wiki/Graph_%28data_structure%29">graph</a><br /> DS_WeightedGraph.h - Graph with weighted edges, used for routing via <a href="http://en.wikipedia.org/wiki/Dijkstra%27s_algorithm">Dijkstra's algorithm</a><br /> RakString - String implementation, up to 4.5 times faster than std::string</p></td>
</tr>
</tbody>
</table>

|----------|
|  Support |

|--------------------------------------------------------|
| [FAQ](faq.html)                                        
  [Debugging Disconnections](debuggingdisconnects.html)  
  [Programming Tips](programmingtips.html)               
  [Revision Log](revisionlog.html)                       |
