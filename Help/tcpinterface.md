<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-------------------------|
|  TCP Interface Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Connect to Telnet, HTTP servers, mail servers, or other</span><br /><br /> TCPInterface, found at TCPInterface.h, is a utility class designed to connect using the TCP protocol in cases where this is necessary. The connection process is similar to RakPeerInterface.h, with the exception that Receive() returns the actual data received, the first byte does not contain any specific identifiers.</p>
<p>To get connection status updates, use HasNewConnection() and HasLostConnection()</p>
<p>There is no sample that uses the TCPInterface class specifically, but TelnetTransport.cpp is a good place to look for a reference.</p>
<p><strong>Functions:</strong></p>
<p>// Starts the TCP server on the indicated port<br /> <span class="RakNetCode">bool Start(unsigned short port, unsigned short maxIncomingConnections);</span></p>
<p>// Stops the TCP server<br /> <span class="RakNetCode">void Stop(void);</span></p>
<p>// Connect to the specified host on the specified port<br /> <span class="RakNetCode">SystemAddress Connect(const char* host, unsigned short remotePort);</span></p>
<p>// Sends a byte stream<br /> <span class="RakNetCode">void Send( const char *data, unsigned length, SystemAddress systemAddress );</span></p>
<p>// Returns data received<br /> <span class="RakNetCode">Packet* Receive( void );</span></p>
<p>// Disconnects a player/address<br /> <span class="RakNetCode">void CloseConnection( SystemAddress systemAddress );</span></p>
<p>// Deallocates a packet returned by Receive<br /> <span class="RakNetCode">void DeallocatePacket( Packet *packet );</span></p>
<p>// Queued events of new connections<br /> <span class="RakNetCode">SystemAddress HasNewConnection(void);</span></p>
<p>// Queued events of lost connections<br /> <span class="RakNetCode">SystemAddress HasLostConnection(void);</span></p></td>
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
<td align="left"><p><a href="index.html">Index</a><br /> <a href="http://en.wikipedia.org/wiki/Transmission_Control_Protocol">TCP (Wikipedia)</a><br /> <a href="emailsender.html">Email Sender</a><br /></p></td>
</tr>
</tbody>
</table>
