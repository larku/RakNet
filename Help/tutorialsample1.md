![Oculus VR, Inc.](RakNetLogo.jpg)
|-------------------------------------------|
| **![](spacer.gif)Tutorial code sample 1** |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><strong>Instantiation and destruction.</strong><br /><br /> This is the base code to instantiate and destroy a peer as client or a server, along with some simple user input.
<pre><code>#include &lt;stdio.h&gt;
#include &quot;RakPeerInterface.h&quot;

#define MAX_CLIENTS 10
#define SERVER_PORT 60000

int main(void)
{
    char str[512];
    RakNet::RakPeerInterface *peer = RakNet::RakPeerInterface::GetInstance();
    bool isServer;

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


    // TODO - Add code body here

    RakNet::RakPeerInterface::DestroyInstance(peer);

    return 0;
}
</code></pre></td>
</tr>
</tbody>
</table>
