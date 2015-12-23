![Oculus VR, Inc.](RakNetLogo.jpg)
|---------------------------|
| ![](spacer.gif)Player IDs |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">What are Player IDs?<br /><br /> PlayerIDs are structs containing the binary IP address and port of a system on the network. Here's some cases where you'll need PlayerID:<br />
<ul>
<li>The server got a message from a particular client and wants to relay to all other clients. You would specify the sender PlayerID (given in the Packet::PlayerID field) in the Send function with broadcast as true.</li>
<li>Some item in the gameworld, such as a mine, belongs to a particular player and you want to give the appropriate person points for the kill.</li>
<li>You want some kind of 1:1 mapping for each player that every client knows about. For example if each player in your game had a unique score.</li>
<li>You want to send a message to any peer on a peer to peer network.</li>
</ul>
<br /> <strong>Important Considerations:</strong><br /><br /> 1. The recipient of a packet automatically knows the PlayerID of any system that sends a packet to it because it determines this from the sender's IP/Port combination. The sender does not need to encode its own PlayerID in the data structure if all you need is for the server to know what the PlayerID is. The PlayerID of the originating sender is automatically passed to the programmer in the Packet structure that is returned by Receive.<br /><br /> 2. When using the client server model, clients DO NOT know the PlayerID of who originally sent the packet. As far as a client is concerned all packets originate with the server. Therefore if it is necessary that a client know the PlayerID of another client you must add a PlayerID field into the data struct. You can either have the sending client fill this field in, or you can have the server fill it out when it gets the packet from the original sender.<br /><br /> 3. PlayerIDs are no longer sequentially assigned or assigned within a certain range. Prior users should be aware of this.<br /><br /></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|---------------------|
| [Index](index.html) |
