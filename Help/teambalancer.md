<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|--------------------------------------|
| ![](spacer.gif)TeamBalancer Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Request and balance teams client/server or peer to peer</span></p>
<p><strong>DEPRECATED: See <a href="teammanager.html">TeamManager</a></strong></p>
<p>The TeamBalancer plugin is used to assign each player in a game session a team number. Players by default have no team, and join teams by calling <em>RequestSpecificTeam()</em> or <em>RequestAnyTeam()</em></p>
<p>Operations include:</p>
<ul>
<li><strong>SetTeamSizeLimits()</strong> - Define the maximum number of players that can join a given team number</li>
<li><strong>SetDefaultAssignmentAlgorithm()</strong> - Define how to automatically add new players to team - either filling teams in order, or joining the smallest team. This is triggered by a call to <em>RequestAnyTeam()</em></li>
<li><strong>SetForceEvenTeams()</strong> - Cause all teams to be evenly balanced. Teams with too many players will have players randomly moved to teams with too few players.</li>
<li><strong>RequestSpecificTeam()</strong> - Change to a requested team. If this team is full, your join will be pending, until either that team is not full, or a player on the desired team wants to switch with your team.</li>
<li><strong>CancelRequestSpecificTeam()</strong> - If <em>RequestSpecificTeam()</em> has not yet completed, this will remove that request.</li>
<li><strong>RequestAnyTeam()</strong> - Join a team randomly, based on the default team assignment algorithm.</li>
<li><strong>GetMyTeam()</strong> - Return which team I am on, if any.</li>
<li><strong>SetAllowHostMigration()</strong> - Call with true for peer to peer, otherwise call with false.</li>
</ul>
<p>See the header file TeamBalancer.h for more information and complete documentation of each parameter and function, as well as messages returned to the user.</p>
<p><em>See the sample project Samples\TeamBalancer for an implementation of this system.</em></p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|------------------------------------------|
| [Index](index.html)                      
  [PluginInterface](plugininterface.html)  |

[TeamManager](teammanager.html)
