<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|------------------------------------------|
| ![](spacer.gif)NAT Punchthrough overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><a href="http://science.webhostinggeeks.com/raknet-nat-punchthrough">Serbo-Croatian translation</a>
<p><span class="RakNetBlueHeader">What is NAT?</span><br /><br /> NAT is short for network address translation. It is used by routers to map addresses behind the router to a single destination address, using different ports. For example, if you have two computers behind a router, but only one ISP, then both computers will use the same IP address, but with different source ports than what the application actually assigned. The router keeps a lookup table of what mappings it provides, so when a remote computer replies it is routed to the correct local computer behind the NAT.</p>
<p>The problem with NAT is that remote computers cannot initiate sends to local computers because no mapping yet exists. Therefore, if two computers both behind NAT try to connect, neither will be able to do so. This is a problem with voice communication, peer to peer games, or games where your users host and the host is behind a NAT. In the old days your users would have to go to their router configuration screen and setup a mapping. However, in modern applications users are not usually required to do this, thanks to NatPunchthrough.</p>
<p><span class="RakNetBlueHeader">NAT Punchthrough Overview</span></p>
<p>The NatPunchthroughClient.cpp plugin requires a user hosted server, not behind NAT, running NatPunchthroughServer.cpp that both clients can connect to. The server will find the external IP address of each client, and tell both clients to connect to that address at the same time. If that fails, each client will attempt to estimate the ports used by the other. If that fails, the process repeats once again, in case later port estimation opened a prior port. If that also fails, the plugin returns ID_NAT_PUNCHTHROUGH_FAILED.</p>
<p>Note 1: If you publish through Steam, we also provide <a href="steamlobby.html">SteamLobby</a>, which uses servers hosted by Valve, in which case NATPunchthrough is not necessary.<br /> Note 2: NAT Punchthrough is not necessary if exclusively using <a href="ipv6support.html">IPV6</a>.<br /> Note 3: If your game is client/server only, you may simply enforce that servers must support UPNP. See <em>DependentExtensions\miniupnpc-1.6.20120410</em>. Most routers support this these days, and assuming UPNP passes, anyone can connect to you.</p>
<p><span class="RakNetBlueHeader">NAT Punchthrough Algorithm</span></p>
<ol>
<li>Peer P1 wants to connect to peer P2, both of whom are connected to a third Non-NAT system, F</li>
<li>Peer P1 calls OpenNAT() with the RakNetGUID (unique identifier) of P2 to F.</li>
<li>F returns failure if P2 is not connected, or already attempting punchthrough to P1.</li>
<li>F remembers busy state of P1 and P2. If either P1 or P2 is busy, the request is pushed to a queue. Otherwise F requests most recently used external port from P1 and P2. P1 and P2 are flagged as busy.</li>
<li>If either P1 or P2 do not respond punchthrough fails with ID_NAT_TARGET_UNRESPONSIVE and the busy flag is unset. Otherwise, F sends timestamped connection message to P1 and P2 simultaneously.</li>
<li>P1 and P2 act identically at this point. First, they send multiple UDP datagrams to each other's internal LAN addresses. They then try each other's external IP/port as seen by F. Ports are attempted sequentially, up to MAX_PREDICTIVE_PORT_RANGE.</li>
<li>If at any point a datagram arrives from the remote peer, we enter state PUNCHING_FIXED_PORT. Datagrams are only sent to that IP/port combination the remainder of the algorithm. If our reply arrives on the remote system, the NAT is considered bidirectional and ID_NAT_PUNCHTHROUGH_SUCCEEDED is returned to the user.</li>
<li>When NAT is open, or if we exhaust all ports, P1 and P2 send to F that they are ready for a new punchthrough attempt.</li>
</ol>
<p>Algorithm effectivness depends on the NAT types involved. It will work with whichever NAT is the most permissive.</p>
<p><strong>Full cone NAT</strong>: Accepts any datagrams to a port that has been previously used. Will accept the first datagram from the remote peer.</p>
<p><strong>Address-Restricted cone NAT</strong>: Accepts datagrams to a port as long as the datagram source IP address is a system we have already sent to. Will accept the first datagram if both systems send simultaneously. Otherwise, will accept the first datagram after we have sent one datagram.</p>
<p><strong>Port-Restricted cone NAT</strong>: Same as address-restricted cone NAT, but we had to send to both the correct remote IP address and correct remote port. The same source address and port to a different destination uses the same mapping.</p>
<p><strong>Symmetric NAT</strong>: A different port is chosen for every remote destination. The same source address and port to a different destination uses a different mapping. Since the port will be different, the first external punchthrough attempt will fail. For this to work it requires port-prediction (MAX_PREDICTIVE_PORT_RANGE&gt;1) and that the router chooses ports sequentially.</p>
<strong>Success Graph</strong>
<table>
<tbody>
<tr class="odd">
<td align="left">Router Type</td>
<td align="left"><em>Full cone NAT</em></td>
<td align="left"><em>Address-Restricted cone NAT</em></td>
<td align="left"><em>Port-Restricted cone NAT</em></td>
<td align="left"><em>Symmetric NAT</em></td>
</tr>
<tr class="even">
<td align="left"><em>Full cone NAT</em></td>
<td align="left">YES</td>
<td align="left">YES</td>
<td align="left">YES</td>
<td align="left">YES</td>
</tr>
<tr class="odd">
<td align="left"><em>Address-Restricted cone NAT</em></td>
<td align="left">YES</td>
<td align="left">YES</td>
<td align="left">YES</td>
<td align="left">YES</td>
</tr>
<tr class="even">
<td align="left"><em>Port-Restricted cone NAT</em></td>
<td align="left">YES</td>
<td align="left">YES</td>
<td align="left">YES</td>
<td align="left">NO</td>
</tr>
<tr class="odd">
<td align="left"><em>Symmetric NAT</em></td>
<td align="left">YES</td>
<td align="left">YES</td>
<td align="left">NO</td>
<td align="left">NO</td>
</tr>
</tbody>
</table>
<br /> * NO may still connect if port estimation works, but can't be relied upon.
<p><span class="RakNetBlueHeader">Client Implementation</span></p>
<ol>
<li>Create an instance of the plugin: <span class="RakNetCode">NatPunchthroughClient natPunchthroughClient;</span></li>
<li>Attach the plugin to an instance of RakPeerInterface: <span class="RakNetCode">rakPeer-&gt;AttachPlugin(&amp;natPunchthroughClient);</span></li>
<li>Connect to the server, and wait for ID_CONNECTION_REQUEST_ACCEPTED. Use the following line to use the free server provided by RakNet: <span class="RakNetCode">rakPeer-&gt;Connect(&quot;natpunch.jenkinssoftware.com&quot;, 61111, 0, 0);</span></li>
<li>Call OpenNAT with the RakNetGUID (globally unique identifier) of the remote system that you want to connect to. In order to get the RakNetGUID, you would either have to transmit it with your own code on the server, upload it to the <a href="phpdirectoryserver.html">PHPDirectoryServer</a>, or use a plugin that stores it, such as <a href="lightweightdatabase.html">LightweightDatabase</a>: <span class="RakNetCode">natPunchthroughClient.OpenNAT(remoteGuid, serverSystemAddress);</span>. In order to read your own RakNetGUID, use <span class="RakNetCode">RakPeerInterface::GetGuidFromSystemAddress(UNASSIGNED_SYSTEM_ADDRESS);</span></li>
<li>Wait a while. It may take over 10 seconds to try every possible port twice, although it often works within a couple of seconds. If you want to get text messages on what is happening, you can use <span class="RakNetCode">NatPunchthroughClient::SetDebugInterface()</span></li>
<li>ID_NAT_PUNCHTHROUGH_SUCCEEDED means the punchthrough succeeded, and you should be able to connect or send other messages to the remote system. Packet::SystemAddress is the address of the system you can now connect to. Any other ID_NAT_* means the punchthrough failed. See MessageIdentifiers.h for the list of codes and comments on each.</li>
</ol>
<p><span class="RakNetBlueHeader">Server Implementation</span></p>
<ol>
<li>Host a server somewhere, not using NAT / e.g. behind a firewall. (RakNet provides a free one at 8.17.250.34:60481, however you may wish to host your own for consistent uptime).</li>
<li>Create an instance of the plugin: <span class="RakNetCode">NatPunchthroughServer natPunchthroughServer;</span></li>
<li>Attach the plugin: <span class="RakNetCode">rakPeer-&gt;AttachPlugin(&amp;natPunchthroughServer);</span></li>
<li>Don't forget to call <span class="RakNetCode">RakPeerInterface::Startup()</span> and <span class="RakNetCode">RakPeerInterface::SetMaximumIncomingConnections(MAX_CONNECTIONS);</span></li>
</ol>
<p><span class="RakNetBlueHeader">Using the NatPunchthrough class </span><br /><br /> See the sample <em>\Samples\NATCompleteClient</em> and <em>\Samples\NATCompleteServer</em></p>
<p></p>
<table>
<tbody>
<tr class="odd">
<td align="left"><img src="spacer.gif" />UDP Proxy</td>
</tr>
</tbody>
</table>
<p>With some poor quality or homemade routers, it is possible that NAT punchthrough will not work. For example, a router that picks a new random port for each outgoing connection, and will only allow incoming connections to this port, will never work. This happens about 5% of the time. To handle this case, RakNet provides the UDPProxy system. Essentially, it uses a server that you run to route messages between the source and destination client transparently. This even works to route datagrams from games not using RakNet (though you need RakNet to setup the forwarding). The combination of NATPunchthrough and UDPProxy should enable any system to connect to any other system with a 100% success rate, provided you are willing to host enough proxy servers to forward all the traffic.</p>
<p>The UDP Proxy system uses 3 main classes:</p>
<ul>
<li><strong>UDPProxyClient</strong>: Makes requests of UDPProxyCoordinator to setup forwarding. This is the class the client runs.</li>
<li><strong>UDPProxyCoordinator</strong>: Runs on the server that will get all requests from UDPProxyClient. Also, gets all logins from UDPProxyServer</li>
<li><strong>UDPProxyServer</strong>: Actually does the UDP datagram forwarding, via a composite instance of UDPForwarder.cpp<br /></li>
</ul>
<p><span class="RakNetBlueHeader">Client Implementation</span>:</p>
<ol>
<li>Create an instance of the plugin: <span class="RakNetCode">UDPProxyClient udpProxyClient;</span></li>
<li>Derive a class from <span class="RakNetCode">RakNet::UDPProxyClientResultHandler</span> to get event notifications for the system.</li>
<li>Attach the plugin to an instance of RakPeerInterface: <span class="RakNetCode">rakPeer-&gt;AttachPlugin(&amp;udpProxyClient);</span></li>
<li>Call UDPProxyClient::SetResultHandler() on the class created in step 2.</li>
<li>Try NATPunchthrough first. If you get ID_NAT_PUNCHTHROUGH_FAILED for the system that initiated NATPunchthrough, go to step 6. Both systems will return ID_NAT_PUNCHTHROUGH_FAILED, however, only one system needs to start the proxy system.</li>
<li>Call UDPProxyClient::RequestForwarding with the address of the coordinator, the address you want to forward from (UNASSIGNED_SYSTEM_ADDRESS for your own), the address you want to forward to, and how long to keep the forwarding active on no data. For example:<br /> <span class="RakNetCode">SystemAddress coordinatorAddress;<br /> coordinatorAddress.SetBinaryAddress(&quot;8.17.250.34&quot;);<br /> coordinatorAddress.port=60481;<br /> udpProxyClient.RequestForwarding(coordinatorAddress, UNASSIGNED_SYSTEM_ADDRESS, p-&gt;systemAddress, 7000);</span></li>
<li>Assuming you are connected to the coordinator, and the coordinator is running the plugin, your event handler class created in step 2 should get a callback within a second or two. <span class="RakNetCode">UDPProxyClientResultHandler::OnForwardingSuccess</span> will be returned if a UDPProxyServer has been assigned to forward datagrams from the source system specified in step 6, to the target system specified in step 6. For example, to connect to the remote system use: <span class="RakNetCode">rakPeer-&gt;Connect(proxyIPAddress, proxyPort, 0, 0);</span></li>
</ol>
<p>If more than one server is available, and both source and target relay systems are running RakNet, then source and target will automatically ping all available servers. The servers will be attempted in order of lowest ping sum to highest. This is based on the assumption that the lowest ping sum gives you the server that has the shortest path between the two systems, and therefore the least lag.</p>
<p><span class="RakNetBlueHeader">Coordinator Implementation</span>:</p>
<ol>
<li>Create an instance of the plugin: <span class="RakNetCode">UDPProxyCoordinator udpProxyCoordinator;</span></li>
<li>Attach the plugin to an instance of RakPeerInterface: <span class="RakNetCode">rakPeer-&gt;AttachPlugin(&amp;udpProxyCoordinator);</span></li>
<li>Set the password on the coordinator for the servers to use <span class="RakNetCode">udpProxyCoordinator.SetRemoteLoginPassword(COORDINATOR_PASSWORD);</span></li>
<li>Don't forget to call <span class="RakNetCode">RakPeerInterface::Startup()</span> and <span class="RakNetCode">RakPeerInterface::SetMaximumIncomingConnections(MAX_CONNECTIONS);</span></li>
</ol>
<p><span class="RakNetBlueHeader">Server Implementation</span>:</p>
<ol>
<li>Create an instance of the plugin: <span class="RakNetCode">UDPProxyServer udpProxyServer;</span></li>
<li>Attach the plugin to an instance of RakPeerInterface: <span class="RakNetCode">rakPeer-&gt;AttachPlugin(&amp;udpProxyServer);</span></li>
<li>Connect to the coordinator</li>
<li>Login to the coordinator. This can be done at runtime, so you can dynamically add more forwarding servers as your game is more popular.<br /> <span class="RakNetCode">udpProxyServer.LoginToCoordinator(COORDINATOR_PASSWORD, coordinatorSystemAddress);</span><br /> If the coordinator plugin is on the same system as the server plugin, use:<br /> <span class="RakNetCode">udpProxyServer.LoginToCoordinator(COORDINATOR_PASSWORD, rakPeer-&gt;GetInternalID(UNASSIGNED_SYSTEM_ADDRESS));</span></li>
<li>If you want to get callbacks as events occur (especially login failure) derive from <span class="RakNetCode">RakNet::UDPProxyServerResultHandler</span> and register your derived class with <span class="RakNetCode">UDPProxyServer::SetResultHandler()</span></li>
</ol>
<p><strong>State diagram with UDP Proxy</strong></p>
<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><a href="natpunchpanel1.jpg"><img src="natpunchpanel1small.jpg" /></a></td>
<td align="left"><a href="natpunchpanel2.jpg"><img src="natpunchpanel2small.jpg" /></a></td>
<td align="left"><a href="natpunchpanel3.jpg"><img src="natpunchpanel3small.jpg" /></a></td>
<td align="left"><a href="natpunchpanel4.jpg"><img src="natpunchpanel4small.jpg" /></a></td>
</tr>
<tr class="even">
<td align="left"><a href="natpunchpanel5.jpg"><img src="natpunchpanel5small.jpg" /></a></td>
<td align="left"><a href="natpunchpanel6.jpg"><img src="natpunchpanel6small.jpg" /></a></td>
<td align="left"><a href="natpunchpanel7.jpg"><img src="natpunchpanel7small.jpg" /></a></td>
<td align="left"></td>
</tr>
</tbody>
</table>
<br />
<table>
<tbody>
<tr class="odd">
<td align="left"><img src="spacer.gif" />Server hosting</td>
</tr>
</tbody>
</table>
<p><span class="RakNetBlueHeader">Server requirements</span></p>
<ol>
<li>No network address translation.</li>
<li>No firewall, or firewall opened on the appropriate ports.</li>
<li>Static IP address. <a href="http://www.dyndns.com/">Dynamic DNS</a> is one way to get around this requirement.</li>
<li>Compile with __GET_TIME_64BIT if you want to run the server longer than a month without rebooting</li>
<li>Enough bandwidth to handle all connections</li>
</ol>
<p><span class="RakNetBlueHeader">Commercial hosting solutions</span></p>
<ol>
<li><a href="http://www.hypernia.com/">Hypernia</a><br /> Worldwide locations. Servers are individual machines. Starting at $150 a month</li>
</ol>
If you find more hosting solutions, <script type="text/javascript">
<!--
h='&#106;&#x65;&#110;&#x6b;&#x69;&#110;&#x73;&#x73;&#x6f;&#102;&#116;&#x77;&#x61;&#114;&#x65;&#46;&#x63;&#x6f;&#x6d;';a='&#64;';n='&#114;&#x61;&#x6b;&#x6b;&#x61;&#114;';e=n+a+h;
document.write('<a h'+'ref'+'="ma'+'ilto'+':'+e+'" clas'+'s="em' + 'ail">'+'contact us'+'<\/'+'a'+'>');
// -->
</script><noscript>&#x63;&#x6f;&#110;&#116;&#x61;&#x63;&#116;&#32;&#x75;&#x73;&#32;&#40;&#114;&#x61;&#x6b;&#x6b;&#x61;&#114;&#32;&#x61;&#116;&#32;&#106;&#x65;&#110;&#x6b;&#x69;&#110;&#x73;&#x73;&#x6f;&#102;&#116;&#x77;&#x61;&#114;&#x65;&#32;&#100;&#x6f;&#116;&#32;&#x63;&#x6f;&#x6d;&#x29;</noscript> and it'll be added to this list.<br /><br />
<table>
<tbody>
<tr class="odd">
<td align="left"><img src="spacer.gif" />See Also</td>
</tr>
</tbody>
</table>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="index.html">Index</a><br /> <a href="cloudhosting.html">Cloud Hosting</a><br /> <a href="ipv6support.html">IPV6 support</a><br /> <a href="nattraversalarchitecture.html">NAT Traversal architecture</a><br /> <a href="nattypedetection.html">NAT type detection</a><br /> <a href="steamlobby.html">SteamLobby</a><br /><br /></p></td>
</tr>
</tbody>
</table></td>
</tr>
</tbody>
</table>
