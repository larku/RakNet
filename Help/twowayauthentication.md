<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-----------------------------------------|
|  Two Way Authentication Plugin Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Securely verify a password known in advance by a pair of systems</span><br /></p>
<p>Normally with RakNet you can transmit data securely using <a href="secureconnections.html">secure connections</a>. However, sometimes a pair of systems may not have secure connections active. For example, on mobiles the security code may take too much memory, be too slow, or not compile. In this case you can still verify a remote system using a password known to two systems in advance. RakNet implements this using <a href="http://en.wikipedia.org/wiki/Mutual_authentication">two way authentication</a>. Instead of sending the password itself, a <a href="http://en.wikipedia.org/wiki/Cryptographic_hash_function">one way hash</a> of the password is sent. The hash is verified as correct or not, and the results returned to the user.</p>
<p>Usage:</p>
<p>// Attach the plugin to an instance of RakPeerInterface<br /> rakPeer-&gt;AttachPlugin(&amp;twoWayAuthenticationPlugin);<br /> // Add a password. The actual password (Password0) is associated with a identifier (PWD0) for faster hash lookup.<br /> twoWayAuthenticationPlugin.AddPassword(&quot;PWD0&quot;, &quot;Password0&quot;);<br /> // Challenge another system we are connected to.<br /> twoWayAuthenticationPlugin.Challenge(&quot;PWD0&quot;, remoteSystemAddressOrGuid);</p>
<p>If the other system is also running the two way authentication plugin, and had also set the same password, you will get <span class="RakNetCode">ID_TWO_WAY_AUTHENTICATION_INCOMING_CHALLENGE_SUCCESS</span> and the other system will get <span class="RakNetCode">ID_TWO_WAY_AUTHENTICATION_OUTGOING_CHALLENGE_SUCCESS</span>. If the other system is running the plugin but has a different password, they will get <span class="RakNetCode">ID_TWO_WAY_AUTHENTICATION_INCOMING_CHALLENGE_FAILURE</span> and you will get <span class="RakNetCode">ID_TWO_WAY_AUTHENTICATION_OUTGOING_CHALLENGE_FAILURE</span>. If the other system is not running the plugin you will get <span class="RakNetCode">ID_TWO_WAY_AUTHENTICATION_OUTGOING_CHALLENGE_TIMEOUT</span>.</p>
<p>In the case of</p>
<ul>
<li>ID_TWO_WAY_AUTHENTICATION_INCOMING_CHALLENGE_SUCCESS</li>
<li>ID_TWO_WAY_AUTHENTICATION_OUTGOING_CHALLENGE_SUCCESS</li>
<li>ID_TWO_WAY_AUTHENTICATION_OUTGOING_CHALLENGE_TIMEOUT</li>
<li>ID_TWO_WAY_AUTHENTICATION_OUTGOING_CHALLENGE_FAILURE</li>
</ul>
you can read which password challenge the message was associated with using the following code:
<p>RakNet::BitStream bs(packet-&gt;data, packet-&gt;length, false);<br /> bs.IgnoreBytes(sizeof(RakNet::MessageID));<br /> RakNet::RakString password;<br /> bs.Read(password);</p>
<p>All this system does is verify a password between two parties. It does not enable or disable any RakNet features, or prevent any other message from being sent during the challenge phase. However, you can pair this plugin with the <a href="messagefilter.html">MessageFilter</a> plugin so that a new connection cannot send any other messages until validated. To do so, attach the <a href="messagefilter.html">MessageFilter</a> plugin to RakPeerInterface before this one (actually it should be first). Call MessageFilter::SetAutoAddNewConnectionsToFilter() so that new connections are filtered. Make sure that the two way authentication messages are allowed on that same filter channel by calling MessageFilter::SetAllowMessageID(). When a connection has been validated, change the channel for that system using MessageFilter::SetSystemFilterSet().</p>
<p>See <em>Samples/TwoWayAuthentication</em> for a complete sample. See TwoWayAuthentication.h for a complete list of all documented functions and parameters.</p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

[Index](index.html)
 [Message Filter plugin](messagefilter.html)
