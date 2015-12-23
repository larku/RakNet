<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|---------------------------------------------------------------------|
| ![](spacer.gif)Fully Connected Mesh Plugin Interface Implementation |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Automatically connect all peers in a peer to peer game</span><br /><br /> The fully connected mesh implementation reads connection messages from RakNet and tries to connect to all systems on the network. On a new connection, it will tell the remote system of all systems it knows about. When that packet arrives, that system will then try to connect to all those systems in turn. You can optionally specify a password which you can use to allow certain systems to join the mesh and other systems to not do so.</p>
<p>NOTE : The implementation requires that a ConnectionGraph plugin is also attached to the RakPeer.<br /></p>
<p>/// Set the password to use to connect to the other systems<br /> void Startup(const char *password, int _passwordLength);</p>
<p>/// Use the NAT punchthrough system to connect rather than calling directly<br /> void ConnectWithNatPunchthrough(NatPunchthrough *np, SystemAddress _facilitator);</p>
<p>See <em>Samples/FullyConnectedMesh</em> for a demonstration of this plugin</p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="index.html">Index</a><br /> <a href="plugininterface.html">PluginInterface</a><br /> <a href="natpunchthrough.html">NAT-Punchthrough</a><br /> <a href="readyevent.html">Ready Event</a><br /></p></td>
</tr>
</tbody>
</table>
