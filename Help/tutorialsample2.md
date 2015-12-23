![Oculus VR, Inc.](RakNetLogo.jpg)
|-------------------------------------------|
| **![](spacer.gif)Tutorial code sample 2** |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><strong>Connecting, reading, and parsing network messages.</strong><br /><br /> The target of this exercise was to add the following features to sample 1:
<ol>
<li>Create a main loop for the program.</li>
<li>Call Receive and store the pointer returned in a pointer variable of type Packet. Note that we continually call Receive() until no further packets are returned. A common mistake is to only call it once.</li>
<li>Include the header file to use struct Packet</li>
<li>If a packet arrived, check the first byte of Packet::data, and process accordingly. The list of enumerations is in MessageIdentifiers.h.</li>
<li>Print out the comment that goes along with the enumeration.</li>
<li>Deallocate the packet pointer when done with it.</li>
</ol>
New code over sample 1 is in bold.
<pre><code>


#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &quot;RakPeerInterface.h&quot;
#include &quot;MessageIdentifiers.h&quot;

#define MAX_CLIENTS 10
#define SERVER_PORT 60000

int main(void)
{
    char str[512];

    RakNet::RakPeerInterface *peer = RakNet::RakPeerInterface::GetInstance();
    bool isServer;
    RakNet::Packet *packet;
    
    printf(&quot;(C) or (S)erver?\n&quot;);
    gets(str);

    if ((str[0]==&#39;c&#39;)||(str[0]==&#39;C&#39;))
    {
        RakNet::SocketDescriptor sd;
        peer-&gt;Startup(1,&amp;sd, 1);
        isServer = false;
    } else {
        RakNet::SocketDescriptor sd(SERVER_PORT,0);
        peer-&gt;Startup(MAX_CLIENTS, &amp;sd, 1);
        isServer = true;
    }

    
    if (isServer)
    {
        printf(&quot;Starting the server.\n&quot;);
        // We need to let the server accept incoming connections from the clients
        peer-&gt;SetMaximumIncomingConnections(MAX_CLIENTS);
    } else {
        printf(&quot;Enter server IP or hit enter for 127.0.0.1\n&quot;);
        gets(str);
        if (str[0]==0){
            strcpy(str, &quot;127.0.0.1&quot;);
        }
        printf(&quot;Starting the client.\n&quot;);
        peer-&gt;Connect(str, SERVER_PORT, 0,0);

    }

    while (1)
    {
        for (packet=peer-&gt;Receive(); packet; peer-&gt;DeallocatePacket(packet), packet=peer-&gt;Receive())
        {
            switch (packet-&gt;data[0])
                {
                case ID_REMOTE_DISCONNECTION_NOTIFICATION:
                    printf(&quot;Another client has disconnected.\n&quot;);
                    break;
                case ID_REMOTE_CONNECTION_LOST:
                    printf(&quot;Another client has lost the connection.\n&quot;);
                    break;
                case ID_REMOTE_NEW_INCOMING_CONNECTION:
                    printf(&quot;Another client has connected.\n&quot;);
                    break;
                case ID_CONNECTION_REQUEST_ACCEPTED:
                    printf(&quot;Our connection request has been accepted.\n&quot;);
                    break;                  
                case ID_NEW_INCOMING_CONNECTION:
                    printf(&quot;A connection is incoming.\n&quot;);
                    break;
                case ID_NO_FREE_INCOMING_CONNECTIONS:
                    printf(&quot;The server is full.\n&quot;);
                    break;
                case ID_DISCONNECTION_NOTIFICATION:
                    if (isServer){
                        printf(&quot;A client has disconnected.\n&quot;);
                    } else {
                        printf(&quot;We have been disconnected.\n&quot;);
                    }
                    break;
                case ID_CONNECTION_LOST:
                    if (isServer){
                        printf(&quot;A client lost the connection.\n&quot;);
                    } else {
                        printf(&quot;Connection lost.\n&quot;);
                    }
                    break;
                default:
                    printf(&quot;Message with identifier %i has arrived.\n&quot;, packet-&gt;data[0]);
                    break;
                }
        }
    }
    
    
    RakNet::RakPeerInterface::DestroyInstance(peer);

    return 0;
}




</code></pre></td>
</tr>
</tbody>
</table>
