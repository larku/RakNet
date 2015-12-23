<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-------------------------------------|
| ![](spacer.gif)Lightweight Database |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><strong>--- Depreciated, use <a href="sqlite3plugin.html">SQLite3Plugin</a> instead ---</strong><br /><br /> LightweightDatabaseServer/LightweightDatabaseClient provides a simple way to implement a master game server.<br /> A master game server is a server that maintains a list of running game servers. It provides the same functionality as similar commercial utilities, at no extra charge.</p>
<p><img src="DirectoryServerListing.jpg" alt="Directory Server Listing" /><br /><br /> There are just two classes you can use. One for the master game server (LightweightDatabaseServer), and one for the client (LightweightDatabaseClient), which your game should use to talk to the master game server. To include this functionality, just make one instantiation of LightweightDatabaseServer or LightweightDatabaseClient as appropriate.<br /><br /> Your master game server, (which uses LightweightDatabaseServer) will generally be a console application or a simple windowed application, whose only purpose is to wait for game servers to inform they are available , or to reply to players systems who want to know what game servers are available.<br /> LightweightDatabaseServer uses in-memory tables to manage the game servers. Just like in a real database, you can define what fields you want in a table.<br /> Check the documentation for LightweightDatabaseServer::AddTable and LightweightDatabaseServer::GetTable<br /> Those functions return a table to which you can add columns (fields).<br /><br /> It's a good idea to host your master game server on a domain name rather than a specific IP so you can change servers without forcing your players to update. For example, if your game's website is mutantkillerfrogs.com you can code the MasterClient to connect to this domain, which will be resolved to an IP.<br /><br /> LightweightDatabaseClient is a client that game servers should use to inform the master game server they are available for use. That is done by connecting to the master game server, and adding a new row in master game server's table. Use LightweightDatabaseClient::UpdateRow for that.<br /><br /> Note you'll also use LightweightDatabaseClient for the player system to connect to the master game server, but just for queries to find available game servers.<br /><br /> See <em>\Samples\LightweightDatabase</em> for full details on how to use those classes.<br /></p></td>
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
<td align="left"><p><a href="index.html">Index</a><br /> <a href="natpunchthrough.html">NAT Punch-through</a><br /> <a href="sqlite3plugin.html">SQLite3Plugin</a><br /></p></td>
</tr>
</tbody>
</table>
