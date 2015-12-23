<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|--------------------------------|
| ![](spacer.gif)SystemAddresses |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">What are SystemAddresses?</span><br /><br /> SystemAddresses are structs containing the binary IP address and port of a system on the network. The structure is defined in the file RakNetTypes.h Here's some cases where you'll need SystemAddress:<br />
<ul>
<li>The server got a message from a particular client and wants to relay to all other clients. You would specify the sender SystemAddress (given in the Packet::systemAddress field) in the Send function with broadcast as true. Some item in the gameworld, such as a mine, belongs to a particular player and you want to give the appropriate person points for the kill.</li>
<li>You want to send a message to any peer on a peer to peer network.</li>
</ul>
<p><strong>Utility functions:</strong></p>
<p>ToString() - Given a system address structure, return a dotted IP address.</p>
<p>FromString() - Given a dotted IP address, fill out the binaryAddress part of the structure.</p>
<p><br /> <strong>Important Considerations:</strong><br /><br /> 1. The recipient of a packet automatically knows the SystemAddress of any system that sends a packet to it because it determines this from the sender's IP/Port combination. The sender does not need to encode its own SystemAddress in the data structure if all you need is for the server to know what the SystemAddress is. The SystemAdddress of the originating sender is automatically passed to the programmer in the Packet structure that is returned by Receive.<br /><br /> 2. When using the client server model, clients DO NOT know the SystemAddress of who originally sent the packet. As far as a client is concerned all packets originate with the server. Therefore if it is necessary that a client know the SystemAddress of another client you must add a SystemAddress field into the data struct. You can either have the sending client fill this field in, or you can have the server fill it out when it gets the packet from the original sender.</p>
<p>3. The system address for a particular instance of RakPeer won't change over the course of the connection, with the exception of the <a href="router.html">Router2</a> plugin. However, is not necessarily the same to all other systems (such as if that system were behind a symmetric NAT). For a unique identifier, use rakNetGUID, found in the Packet structure. RakPeerInterface has functions to operate on RakNetGUID.</p>
<p>4. It is preferred that you refer to remote systems by RakNetGUID, instead of SystemAddress. RakNetGUID is a unique identifier for an instance of RakPeer, while SystemAddress is not. And it is necessary to exclusively use RakNetGUID if you plan to use the <a href="router.html">Router2</a> plugin.<br /><br /><br /></p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|-------------------------|
| [Index](index.html)     
  [Router2](router.html)  |
