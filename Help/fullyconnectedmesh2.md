<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|----------------------------------------------|
| ![](spacer.gif)Fully Connected 2 Mesh Plugin |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Consistently join a fully connected mesh. Automatically determine who is host.</span></p>
<p>FullyConnectedMesh2 solves two problems with peer to peer games. First, how to connect to a list of systems maintained by the session host, with correct behavior on a failed connection. Secondly, to determine and automatically migrate a session host. A session host is needed for authoritative behavior, for example the host may send out game end notifications or may control the AI.</p>
<p><strong>Properly joining a fully connected mesh:</strong></p>
<ol>
<li>All systems: Call SetConnectOnNewRemoteConnection(false, &quot;&quot;);</li>
<li>All systems: Call SetAutoparticipateConnections(false);</li>
<li>Host: When a new system connects and is allowed to join, call StartVerifiedJoin() to send ID_FCM2_VERIFIED_JOIN_START to that system.</li>
<li>Client: On ID_FCM2_VERIFIED_JOIN_START, call fullyConnectedMesh2-&gt;GetVerifiedJoinRequiredProcessingList() to get a list of systems to connect to.</li>
<li>Client: Attempt to connect to each of the system returned from GetVerifiedJoinRequiredProcessingList(). Use NATPunchthroughClient and/or UPNP as necessary. Once all connections have failed or succeeded, the host is informed automatically.</li>
<li>Client: On ID_FCM2_VERIFIED_JOIN_FAILED, either you lost connection to the host, or the host required you to connect to a participant you were not able to. Inform the player via the UI.</li>
<li>Host: On ID_FCM2_VERIFIED_JOIN_CAPABLE, call RespondOnVerifiedJoinCapable() to accept or reject the join based on game logic.</li>
<li>Client: On ID_FCM2_VERIFIED_JOIN_REJECTED, the host rejected the join for gameplay reasons. Disconnect from the host if the connection is no longer needed. Use GetVerifiedJoinRejectedAdditionalData() to determine why the join was rejected, and inform the player via the UI.</li>
<li>Client: On ID_FCM2_VERIFIED_JOIN_ACCEPTED, the host accepted the join. AddParticipant() has automatically been called on all systems, and you have joined the mesh. Call GetVerifiedJoinAcceptedAdditionalData() if desired.</li>
</ol>
StartVerifiedJoin() is of significant benefit when using NatPunchthroughClient because if a connection fails, previously successful connections are closed automatically. Also, because NatPunchthroughClient takes a significant amount of time, a valid game session may no longer valid by the time it completes. However, even when not using NatPunchthroughClient this system is useful because you may still not fully connect due to firewalls or because sessions are full. Ignoring this means you may only connect partially to the mesh, resulting in some players not able to see other players.
<p><strong>Determining who is host of the fully connected mesh</strong></p>
<ol>
<li>If NOT using StartVerifiedJoin(), call AddParticipant() on every system, for every new connection. Note: this is done automatically by default, and is controlled by SetAutoparticipateConnections(true).</li>
<li>Use GetHostSystem() and IsHostSystem() to query who is the host</li>
<li>Do not start gameplay until the host has been calculated. This is known once you get ID_FCM2_NEW_HOST at least once.</li>
<li>Do host migration if necessary when you get ID_FCM2_NEW_HOST during gameplay</li>
</ol>
<p>Host calculation starts automatically once one or more remote connections have been added with AddParticipant(). Until host calculation has completed, GetHostSystem() will return your own guid and GetConnectedHost() will return UNASSIGNED_RAKNET_GUID. You will get ID_FCM2_NEW_HOST as soon as the host is known. The host will generally be whichever system has been running the longest of all systems added with AddParticipant().</p>
<p>GetParticipantList() and GetHostOrder() can be used to find out which systems were added with AddParticipant().</p>
<p>If the game does not immediately start networked, you should call ResetHostCalculation() to reinitialize the host timer when network play is now relevant. Otherwise, a user could play a game in single player, connect to a network session, and then be considered the host because his system was running the longest, although he was the last to join the network session. A good time to call ResetHostCalculation() is just before attempting to join a network room or lobby.</p>
<p><strong>Reading ID_FCM2_NEW_HOST:</strong></p>
<p>ID_FCM2_NEW_HOST has the RakNetGUID of the old host encoded in the network stream. The new host is written to the systemAddress and guid members of the Packet.</p>
<pre><code>   
case ID_FCM2_NEW_HOST:
{
    if (packet-&gt;guid==rakPeer-&gt;GetMyGUID())
        printf(&quot;Got new host (ourselves)&quot;);
    else
        printf(&quot;Got new host %s, GUID=%s&quot;, packet-&gt;systemAddress.ToString(true), packet-&gt;guid.ToString());
    RakNet::BitStream bs(packet-&gt;data,packet-&gt;length,false);
    bs.IgnoreBytes(1);
    RakNetGUID oldHost;
    bs.Read(oldHost);
    if (oldHost==UNASSIGNED_RAKNET_GUID)
    {
        DataStructures::List participantList;
        fullyConnectedMesh2-&gt;GetParticipantList(participantList);
        // First time ID_FCM2_NEW_HOST was returned. Host is now known - can start gameplay, and add all systems that were previously added with AddParticipant() as players
        // ...
    }
    break;</code></pre>
See <em>Samples/FCMHost</em> for a demonstration of this plugin</td>
</tr>
<tr class="even">
<td align="left"> </td>
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
