<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-------------------------------------|
| ![](spacer.gif)TeamManager Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Manages lists of teams and team members. Supports client/server and peer to peer</span></p>
<p>TeamManager reduces the work involved with managing teams and team members. Functionality includes:</p>
<ul>
<li>Support for any number of teams with join conditions on each.</li>
<li>Support for team members being on multiple teams at the same time, or no team at all.</li>
<li>Support for independent worlds, enabling you to host multiple game sessions simultaneously.</li>
<li>The ability to lock, unlock, and resize teams.</li>
<li>Team requests for locked or full teams are buffered and filled in first-come first-server order, including the ability to swap teams.</li>
<li>The state of all teams and all team members are fully replicated to all systems.</li>
<li>Flexible architecture, with support client/server, <a href="replicamanager3.html">ReplicaManager3</a> and the use of either composition or inheritance.</li>
</ul>
<p>Usage</p>
<ol>
<li>Define your game classes to represent teams and team members. Your game classes should hold game-specific information such as team name and color.</li>
<li>Have those game classes contain a corresponding TM_Team or TM_TeamMember instance. Operations on teams will be performed by those instances. Use SetOwner() to refer to the parent object when using composition.</li>
<li>Call TeamManager::SetTopology() for client/server or peer to peer.</li>
<li>Call AddWorld() to instantiate a TM_World object which will contain references to your TM_TeamMember and TM_Team instances.</li>
<li>When you instantiate a TM_TeamMember or TM_Team object, call ReferenceTeam() and ReferenceTeamMember() for each corresponding object. If using ReplicaManager3, the Reference calls should be at the top of DeserializeConstruction(), as well as when creating the object locally.</li>
<li>When sending world state to a new connection, for example in ReplicaManager3::SerializeConstruction(), call TM_SerializeConstruction() on the corresponding TM_TeamMember and TM_Team objects. TM_Team instances on the new connection must be created before TM_TeamMember instances.</li>
<li>Call TM_DeserializeConstruction() on your new corresponding TM_TeamMember and TM_Team instances.</li>
<li>Execute team operations. ID_TEAM_BALANCER_REQUESTED_TEAM_FULL, ID_TEAM_BALANCER_REQUESTED_TEAM_LOCKED, ID_TEAM_BALANCER_TEAM_REQUESTED_CANCELLED, and ID_TEAM_BALANCER_TEAM_ASSIGNED are returned to all systems when the corresponding event occurs for a team member.</li>
<li>As the peer to peer session host changes, call SetHost() (Not necessary if using FullyConnectedMesh2). If using client/server, you must set the host</li>
<li>DeserializeConstruction must be called on TM_Team before DeserializeConstruction is called on TM_TeamMember that is a member of that team. If using ReplicaManager3, you can simply call ReplicaManager3::Reference() on TM_TeamMember later than any ReplicaManager3::Reference() calls to TM_Team</li>
</ol>
<p>See the header file TeamManager.h for more information and complete documentation of each parameter and function, as well as messages returned to the user.</p>
<p><em>See the sample project Samples\TeamManager for an implementation of this system.</em></p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|------------------------------------------|
| [Index](index.html)                      
  [PluginInterface](plugininterface.html)  
  [ReplicaManager3](replicamanager3.html)  
  [TeamManager](teammanager.html)          |
