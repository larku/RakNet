<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-----------------------------------|
| ![](spacer.gif)Plugin Interface 2 |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Plugin Interface 2 Overview</span><br /><br /> <em>PluginInterface2.h</em> is a class interface that works with RakNet to provide automatic functionality. It can intercept, modify, and create messages before they get to the user. Plugins can be attached to either RakPeerInterface or PacketizedTCP. Plugins update every time Receive() is called. To use it, derive from the base class and implement the virtual functions you want to handle. Then register your class by calling RakPeerInterface::AttachPlugin()</p>
<p>These are the virtual functions you'll probably need to handle in most cases:</p>
<p>/// Update is called every time a packet is checked for .<br /> virtual void Update(void);<br /><br /> /// OnReceive is called for every packet.<br /> /// \param[in] packet the packet that is being returned to the user<br /> /// \return True to allow the game and other plugins to get this message, false to absorb it<br /> virtual PluginReceiveResult OnReceive(Packet *packet);</p>
<p>/// Called when a connection is dropped because the user called RakPeer::CloseConnection() for a particular system<br /> /// \param[in] systemAddress The system whose connection was closed<br /> /// \param[in] rakNetGuid The guid of the specified system<br /> /// \param[in] lostConnectionReason How the connection was closed: manually, connection lost, or notification of disconnection<br /> virtual void OnClosedConnection(SystemAddress systemAddress, RakNetGUID rakNetGUID, PI2_LostConnectionReason lostConnectionReason );</p>
<p>/// Called when we got a new connection<br /> /// \param[in] systemAddress Address of the new connection<br /> /// \param[in] rakNetGuid The guid of the specified system<br /> /// \param[in] isIncoming If true, this is ID_NEW_INCOMING_CONNECTION, or the equivalent<br /> virtual void OnNewConnection(SystemAddress systemAddress, RakNetGUID rakNetGUID, bool isIncoming);</p>
<p><br /></p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|---------------------|
| [Index](index.html) |
