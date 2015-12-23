<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|----------------------------------|
| ![](spacer.gif)Network ID Object |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">The NetworkIDObject and NetworkIDManager classes allow lookup of pointers using a common ID</span><br /><br /> The NetworkIDObject class is an optional class you can derive from that will automatically assign identification numbers (NetworkID) to objects that inherit from it. This is useful for multiplayer games, because otherwise you have no way to refer to dynamically allocated objects on remote systems.</p>
<p>In RakNet 4, NetworkIDs are 8 byte globally unique numbers, chosen at random. Older versions of RakNet required that a central authority (server) assign NetworkIDs. This approached was dropped because it creates difficulties for the programmer when the game is peer to peer, or there are multiple distributed servers. It also created extra work if a client were to create an object, because the client would have to assign a temporary ID, request the real NetworkID from the server, and then assign it sometime later.</p>
<p>The NetworkIDObject class provides the following major functions:</p>
<p><strong>SetNetworkIDManager( NetworkIDManager *manager)</strong><br /> The NetworkIDManager holds a list of NetworkIDs for lookup. Therefore, it is required that you call SetNetworkIDManager before calling GetNetworkID() or SetNetworkID(). The reason the list isn't just static is that you might want multiple NetworkIDManagers, for example if you have multiple gameworlds that do not interact with each other.</p>
<p><strong>NetworkID GetNetworkID( void )</strong><br /> If SetNetworkID() was previously called, this returns that value. Otherwise it generates a new, presumably unique NetworkID for the object, if SetNetwor. Internally, the object is not assigned a NetworkID until GetNetworkID() is actually called. In a client/server application if all objects were still created by the server, then it would never be necessary for a client to generate a NetworkID.<br /><br /> <strong>SetNetworkID( NetworkID id )</strong><br /> Assigns a networkID to the object. An example of where this would be used is a server creating a new game object, and sending that object's data to the clients. The client would create a class of the same time, read the NetworkID encoded in the user message, and call SetNetworkID on the same object.</p>
<p>The NetworkIDManager class only has one user function:</p>
<p>template &lt;class returnType&gt;<br /> <strong>returnType GET_OBJECT_FROM_ID(NetworkID x);</strong></p>
<p>This is a templated function, so you write something like:</p>
<p>Solder *soldier = networkIdManager.GET_OBJECT_FROM_ID&lt;Solider*&gt;(networkId);</p>
<p>Here's an example of storing a pointer to a class, retrieving it back again, with an assert to make sure the system worked:</p>
<p>class Soldier : public NetworkIDObject {}<br /> int main(void)<br /> {<br /> NetworkIDManager networkIDManager;<br /> Soldier *soldier = new Soldier;<br /> soldier-&gt;SetNetworkIDManager(&amp;networkIDManager);<br /> NetworkID soldierNetworkID = soldier-&gt;GetNetworkID();<br /> assert(networkIDManager.GET_OBJECT_FROM_ID&lt;Soldier*&gt;(soldierNetworkID)==soldier);<br /> }</p>
<p>Here's an example of using the system to create an object on a remote computer, with the same NetworkID assigned to both:</p>
<p><strong>Server:</strong></p>
<p>void CreateSoldier(void)<br /> {<br /> Soldier *soldier = new Soldier;<br /> soldier-&gt;SetNetworkIDManager(&amp;networkIDManager);<br /> RakNet::BitStream bsOut;<br /> bsOut.Write((MessageID)ID_CREATE_SOLDIER);<br /> bsOut.Write(soldier-&gt;GetNetworkID();<br /> rakPeerInterface-&gt;Send(&amp;bsOut,HIGH_PRIORITY,RELIABLE_ORDERED,0,UNASSIGNED_SYSTEM_ADDRESS,true);<br /> }</p>
<p><strong>Client:</strong></p>
<p>Packet *packet = rakPeerInterface-&gt;Receive();<br /> if (packet-&gt;data[0]==ID_CREATE_SOLDIER)<br /> {<br /> RakNet::BitStream bsIn(packet-&gt;data, packet-&gt;length, false);<br /> bsIn.IgnoreBytes(sizeof(MessageID));<br /> NetworkID soldierNetworkID;<br /> bsIn.Read(soldierNetworkID);<br /> Soldier *soldier = new Soldier;<br /> soldier-&gt;SetNetworkIDManager(&amp;networkIDManager);<br /> soldier-&gt;SetNetworkID(soldierNetworkID);<br /> }</p>
<p>Static objects</p>
<p>Sometimes objects are not created dynamically, but instead already exist on all systems and are known ahead of time. For example, if you have a capture the flag map, with 3 flags, those 3 flags may be hardcoded into the level design and are thus created by all systems when the level loads. Thee are static objects, and can still be referred to by NetworkIDManager. Have those objects derive from NetworkIDObject and call SetNetworkIDManager as usual. Then simply assign a unique ID of your choosing. It doesn't matter what the ID is, as long as it is unique, so you could for example have flag 1 have the ID 0, flag 2 have the ID 1, and flag 3 have the ID 2.</p>
<p>// All static network IDs added here.<br /> enum StaticNetworkIDs<br /> {<br /> CTF_FLAG_1,<br /> CTF_FLAG_2,<br /> CTF_FLAG_3,<br /> };<br /><br /> class Flag : public NetworkIDObject<br /> {<br /> // Level designer named the flag either flag1, flag2, or flag3, and this map will load identically on all systems<br /> Flag(std::string flagName, NetworkIDManager *networkIDManager) {<br /><br /> SetNetworkIDManager(networkIDManager);<br /><br /> if (flagName==&quot;flag1&quot;) SetNetworkID(CTF_FLAG_1);<br /> else if (flagName==&quot;flag2&quot;) SetNetworkID(CTF_FLAG_2);<br /> else if (flagName==&quot;flag3&quot;) SetNetworkID(CTF_FLAG_3);<br /> };</p></td>
</tr>
<tr class="even">
<td align="left"></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|---------------------|
| [Index](index.html) |
