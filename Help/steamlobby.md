<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|---------------------------------------------|
| ![](spacer.gif)Lobby2Client\_Steam Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Use Steamworks to join Lobbies, Rooms, and do NAT traversal</span></p>
<p>The Lobby2Client_Steam plugin provides an interface to Steamworks services that cooperate with RakNet. This avoids the need to host your owns sever, which is otherwise required by RakNet's <a href="natpunchthrough.html">NATPunchthrough</a> and <a href="lobby.html">Lobby</a> systems.</p>
<p>Dependencies:</p>
<ol>
<li>Assumes the Steam SDK is located at C:\Steamworks . If not, modify the post-build step paths and linker paths accordingly.</li>
<li>You can get the Steamworks API from https://partner.steamgames.com/ , which requires a signup and legal agreement.</li>
<li>You need your own steam_appid.txt file. This sample just copies the one that comes with steam.</li>
</ol>
<p>Operations include:</p>
<ul>
<li><strong>Get the list of rooms</strong> - L2MID_Console_SearchRooms</li>
<li><strong>Leave a room</strong> - L2MID_Console_LeaveRoom</li>
<li><strong>Create a room</strong> - L2MID_Console_CreateRoom</li>
<li><strong>Join a room</strong> - L2MID_Console_JoinRoom</li>
<li><strong>Get details about a specific room</strong> - L2MID_Console_GetRoomDetails</li>
<li><strong>Send a chat message to a room</strong> - L2MID_Console_SendRoomChatMessage</li>
<li><strong>Get the members in a room</strong>- Lobby2Client_Steam::GetRoomMembers</li>
<li><strong>Get notification of NAT Traversal success</strong> - SteamResults::Notification_Console_RoomMemberConnectivityUpdate</li>
</ul>
<p><em>See the sample project Samples\SteamLobby for an implementation of this system.</em></p></td>
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
<td align="left"><p><a href="index.html">Index</a><br /> <a href="lobby.html">Lobby</a><br /> <a href="natpunchthrough.html">NATPunchthrough</a><br /> <a href="plugininterface.html">PluginInterface</a></p></td>
</tr>
</tbody>
</table>
