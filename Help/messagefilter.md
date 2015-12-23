<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|--------------------------|
|  Message Filter Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Limit incoming messages by category</span><br /></p>
<p>With client/server topologies, you do not usually want any system to send any message. For example, perhaps only the server can send kill messages. Or perhaps you want to segment players into stages, where players who have logged on but not yet provided their password cannot yet send game messages. The message filter is designed to deal with these situations automatically.</p>
<p>The MessageFilter plugin defines categories of users by 'filterSet' which is just a numerical identifier supplied by the user. For example, you may have one filter set for newly connected systems, and another for authenticated systems. For each filter set you can</p>
<ol>
<li>Automatically add new connections</li>
<li>Allow RPC calls</li>
<li>Limit what messages, or ranges of messages, can be received, and what actions to take if this condition is violated (kick/ban/notify)</li>
<li>Delete them</li>
</ol>
<p><strong>Example:</strong></p>
<p>messageFilter.SetAutoAddNewConnectionsToFilter(0);<br /> messageFilter.SetAllowMessageID(true, ID_USER_PACKET_ENUM, ID_USER_PACKET_ENUM, 0);<br /> messageFilter.SetAllowMessageID(true, ID_USER_PACKET_ENUM+1, ID_USER_PACKET_ENUM+1, 1);</p>
<p>This setup would automatically add all new connections to filter set 0, and only allow the message ID_USER_PACKET_ENUM to arrive. It would also create a new filter set, with the filterSet id 1, that only allowed ID_USER_PACKET_ENUM+1 to arrive.</p>
<p><strong>Messages that are always allowed (filtering them has no effect):</strong></p>
<p>ID_CONNECTION_LOST<br /> ID_DISCONNECTION_NOTIFICATION<br /> ID_NEW_INCOMING_CONNECTION<br /> ID_CONNECTION_REQUEST_ACCEPTED<br /> ID_CONNECTION_ATTEMPT_FAILED<br /> ID_NO_FREE_INCOMING_CONNECTIONS<br /> ID_RSA_PUBLIC_KEY_MISMATCH<br /> ID_CONNECTION_BANNED<br /> ID_INVALID_PASSWORD<br /> ID_MODIFIED_PACKET<br /> ID_PONG<br /> ID_ALREADY_CONNECTED<br /> ID_ADVERTISE_SYSTEM<br /> ID_REMOTE_DISCONNECTION_NOTIFICATION<br /> ID_REMOTE_CONNECTION_LOST<br /> ID_REMOTE_NEW_INCOMING_CONNECTION<br /> ID_DOWNLOAD_PROGRESS</p>
<p>See <em>Samples/MessageFilter</em> for a complete sample. See MessageFilter.h for a complete list of all documented functions and parameters.</p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|---------------------|
| [Index](index.html) |
