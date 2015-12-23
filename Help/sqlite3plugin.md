<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|---------------------------------------|
| ![](spacer.gif)SQLite3Plugin Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Store game or session data over the internet with SQLite</span></p>
<p>Previous versions of RakNet provided this service with the class <a href="lightweightdatabase.html">LightweightDatabase.</a> It is a C++ implementation of a database,using a proprietary interface. However, this proved to have performance issues and was harder to use and less flexible than SQL. As a replacement, SQLite3Plugin was written. It allows clients to execute SQL statements on a remote system (a master server) running SQLite.</p>
<p>Why not use SQLite directly? By default, SQLite only works with file handles. Games need to execute statements over an actual internet connection. The SQLite3Plugin solves this, by using either PacketizedTCP or RakPeerInterface to transmit the statements, parse the results into a structure, and send it back to the user. Since it's a RakNet plugin, you also have access to event callbacks when players connect and disconnect, and information about those players.</p>
<p>SQLite is public domain software, and is included in the RakNet distribution at DependentExtensions\SQLite3Plugin</p>
<p>For both the client and the server, attach the plugin to a connected instance of PacketizedTCP or RakPeerInterface as usual. Derive from SQLite3PluginResultInterface and implement the functions to perform whatever processing you want when those events occur. Register your derived class with the plugin.</p>
<p>On the server only, you will need to setup the SQLite connection before any statements can be executed. The sample creates one in memory:</p>
<p>sqlite3_open_v2(&quot;:memory:&quot;, &amp;database, SQLITE_OPEN_READWRITE | SQLITE_OPEN_CREATE, 0)</p>
<p>For details on sqlite3_open_v2 or other commands, see their manual page <a href="http://www.sqlite.org/c3ref/open.html" class="uri">http://www.sqlite.org/c3ref/open.html</a></p>
<p>Pass the opened database to the plugin with AddDBHandle. The dbIdentifier parameter with AddDBHandle is just a lookup association with the pointer, and can be anything you want. It may make sense to just have it be the same as the path to the database file.</p>
<p>On the client, you can send statements with SQLite3Plugin::_sqlite3_exec. The server will reply, and your result handler callback will be called, with either SQLite3PluginResultInterface::_sqlite3_exec for success, or SQLite3PluginResultInterface::OnUnknownDBIdentifier for failure (unknown database identifier).</p>
<p><strong>Do not forget to escape user input!</strong> RakString::SQLEscape() can be used to do this. It will append a backslash before any quotes, double quotes, or backslashes that already exist in the string.</p>
<p>This system is insecure by default, as by default basically anyone can execute any query. If you want security, you can derive from SQLite3Plugin to control who can send queries, etc. Or put contraints in the database itself in various forms.</p>
<p>Comment or uncomment SQLITE3_STATEMENT_EXECUTE_THREADED at the top of SQLitePlugin.h to control if the statements execute in a thread or not. Usually you want this uncommented (the default), so that blocking on statement execution does not also block your application.</p>
<p>Since this system is intended to replace the <a href="lightweightdatabase.html">LightweightDatabase plugin</a> the sample SQLite3Sample.cpp shows how to automatically perform the most significant functionality that Lightweight database did, which is to add and remove the IP addresses of systems that connect and disconnect. You can modify and/or derive from the sample implementation to add more fields as needed.</p>
<p><em>See the sample project DependentExtensions\SQLite3Plugin for an implementation of this system.</em></p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|---------------------------------------------------------|
|                                                         
 [Index](index.html)                                      
  [LightweightDatabase plugin](lightweightdatabase.html)  
  [PluginInterface](plugininterface.html)                 |
