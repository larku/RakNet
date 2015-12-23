<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>

|----------------------------------------------|
| ![](spacer.gif)PHP Directory Server Overview |

<span class="RakNetBlueHeader">Use a shared webhost to list games</span>
 The [Lightweight database](lightweightdatabase.html) plugin is powerful, but requires an instance of RakNet running on a dedicated server. In some cases this is more than is needed and the burden of running a dedicated server is undesirable. For these instances, RakNet provides DirectoryServer.php, found under Samples\\PHPDirectoryServer.

To setup the web server, simply upload Samples\\PHPDirectoryServer\\DirectoryServer.php to your PHP capable webhost (which is any standard shared webhost). Go to your newly uploaded webpage, click the reveal button, and enter a password and other settings if desired.

The C++ code is more complicated. First, you need an instance of TCPInterface that is started. This is used for general TPC communication

TCPInterface tcp;
 tcp.Start(0, 64);

Second, you need an instance of HTTPConnection. This is used to communicate with webpages through TCPInterface.

HTTPConnection httpConnection(tcp, "jenkinssoftware.com");

Third, you need an instance of PHPDirectoryServer. This does parsing and communications that are specific to DirectoryServer.php.

PHPDirectoryServer phpDirectoryServer(httpConnection, "/raknet/DirectoryServer.php");

You can actually see this page if you to go <http://www.jenkinssoftware.com/raknet/DirectoryServer.php>.
 You can then set columns, upload your table, or download the existing tables:

// Set fields with columnname / value
 phpDirectoryServer.SetField("beehive","inthewater");
 // Upload previously set fields, along with required parameters game name, game port, and password
 phpDirectoryServer.UploadTable(50, "Game name", 1234, "");
 // Download uploaded servers
 phpDirectoryServer.DownloadTable("");

To update the system, you should pass packets from TCPInterface to both interfaces, and also call Update(). This is complicated by the fact that TCP packets may not contain an entire webpage response, the webpage may contain error codes, and not all messages from the webpage are necessarily relevent to our directory server. The sample demonstrates how to do this. The following is an abbreviated excerpt:

Packet \*packet = tcp.Receive();
 if(packet)
 {
 // In this sample this line is not necessary, but if we were using TCPInterface for other things, we want to make sure we only give it messages intended for this connection
 if (packet-\>systemAddress==httpConnection.GetServerAddress())
 {
 // Multiple packets may come in for a single reply from a webserver. When the final packet arrives, ProcessFinalTCPPacket will return true
 if (httpConnection.ProcessFinalTCPPacket(packet))
 {
 int code;
 RakNet::RakString data;
 /// Check that the request was handled and is not an error code
 if (httpConnection.HasBadResponse(&code, &data)==false)
 {
 // Good response, let the PHPDirectoryServer class handle the data
 // If resultCode is not an empty string, then we got something other than a table
 // (such as delete row success notification, or the message is for HTTP only and not for this class).
 HTTPReadResult readResult = phpDirectoryServer.ProcessHTTPRead(httpResult);
 if (readResult==HTTP\_RESULT\_GOT\_TABLE)
 {
 /// Got a table which was stored internally. Print it out
 char out[10000];
 const DataStructures::Table \*games = phpDirectoryServer.GetLastDownloadedTable();
 games-\>PrintColumnHeaders(out,sizeof(out),',');
 printf("COLUMNS: %s\\n", out);
 // print each row of the table
 for (unsigned i=0; i \< games-\>GetRowCount(); i++)
 {
 games-\>PrintRow(out,sizeof(out),',',true, games-\>GetRowByIndex(i,NULL));
 printf("ROW %i: %s\\n", i+1, out);
 }
 }
 }
 }
 }
 // Deallocate the packet the same way you do with RakPeerInterface
 tcp.DeallocatePacket(packet);
 }
 httpConnection.Update();
 phpDirectoryServer.Update();

 Certain columns are reserved, and returned to you in a query. These column names are not allowed for use by the end-user, and will assert if attempted.

// Column with this header contains the name of the game, passed to UploadTable()
 static const char \*GAME\_NAME\_COMMAND="\_\_GAME\_NAME";
 // Column with this header contains the port of the game, passed to UploadTable()
 static const char \*GAME\_PORT\_COMMAND="\_\_GAME\_PORT";
 // Returned from the PHP server indicating when this row was last updated.
 static const char \*LAST\_UPDATE\_COMMAND="\_\_SEC\_AFTER\_EPOCH\_SINCE\_LAST\_UPDATE";
 // Command to delete a row. Internal
 static const char \*DELETEME\_COMMAND="\_\_DELETE\_ROW";
 // Column passed to the PHP server as the password. Internal
 static const char \*GAME\_PASSWORD\_COMMAND="\_\_PHP\_DIRECTORY\_SERVER\_PASSWORD";
 // Column passed to the PHP server as how long to list this server before auto expiring. Internal
 static const char \*GAME\_TIMEOUT\_COMMAND="\_\_GAME\_LISTING\_TIMEOUT";

 \_\_SYSTEM\_ADDRESS will also be returned indicating the external IP that was used to upload to the webpage.
 Note that due to technical limitations only one upload can be processed at a time. This is not a problem if your server is only hosting one game anyway. Calling PHPDirectoryServer::UploadTable() while an upload is pending will overwrite the prior upload. You can perform another upload when HTTPConnectionIsBusy() returns false.

|-------------------------|
| ![](spacer.gif)See Also |

|-------------------------------------|
| [Index](index.html)                 
  [TCP Interface](tcpinterface.html)  |
