<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-------------------|
|  Router2 Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Send messages through intermediate systems</span><br /></p>
<p>Router2 routes datagrams between two systems that are not directly connected by using the bandwidth of a third system, to which the other two systems were connected. It is of benefit when a fully connected mesh topology is desired, but could not be completely established due to routers and/or firewalls. As the system address of a remote system will be the system address of the intermediary, it is necessary to use the RakNetGUID object to refer to systems, including with other plugins</p>
<p>To use</p>
<p>Attach one instance of the Router2 plugin to each system..</p>
<p>Call EstablishRouting with the RakNetGUID of the destination system.</p>
<p>A message will be broadcast to all connected instances of RakPeer. For each of those that are running the Router2 plugin, the connection list will be queried to see who, if anyone is connected to the target system.</p>
<p>After all systems are queried, the system with the lowest total ping, that is also already forwarding the fewest connections, will start the UDPForwarder system and return ID_ROUTER_2_FORWARDING_ESTABLISHED. If no path is available, ID_ROUTER_2_FORWARDING_NO_PATH will be returned instead.</p>
<p>Once a path is established, you should attempt to connect to the target system as usual. The following code should be used:</p>
<p>RakNet::BitStream bs(packet-&gt;data, packet-&gt;length, false);<br /> bs.IgnoreBytes(sizeof(MessageID));<br /> RakNetGUID endpointGuid;<br /> bs.Read(endpointGuid);<br /> unsigned short sourceToDestPort;<br /> bs.Read(sourceToDestPort);<br /> char ipAddressString[32];<br /> packet-&gt;systemAddress.ToString(false, ipAddressString);<br /> rakPeerInterface-&gt;EstablishRouting(ipAddressString, sourceToDestPort, 0,0);</p>
<p>Note that rerouting happens automatically. When a connection is rerouted, you will get ID_ROUTER_2_REROUTED. While the SystemAddress will have changed, the RakNetGUID will not. Therefore, you must only refer to the target system using RakNetGUID when using this plugin.</p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|--------------------------------------------------------------|
| [Index](index.html)                                          
  [NAT Traversal architecture](nattraversalarchitecture.html)  |
