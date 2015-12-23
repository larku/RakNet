<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|--------------------|
|  Irrlicht FPS Demo |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Demonstrates peer to peer connectivity in a first person shooter</span><br /><br /> <img src="IrrlichtRakNetDemo.jpg" /><br /></p>
<p>The FPS demo uses the game engine Irrlicht to move actors and bullets around.</p>
<p>To run it, download <a href="http://irrlicht.sourceforge.net/">Irrlicht</a>, a free game engine. By default, it is assumed to be installed at C:\irrlicht-1.6<br /></p>
<p>In the solution, open Samples / 3D Demos / Irrlicht Demo. Right click and build.</p>
<p>Most of the network code is found in RakNetStuff.cpp under DependentExtensions\IrrlichtDemo. For a technical description of how the code was implemented, see readme.txt in that same directory. As the sample is peer to peer, it requires the NAT punchthrough server to be running. The jenkinssoftware.com website provides a free server, pointed to by DEFAULT_NAT_PUNCHTHROUGH_FACILITATOR_IP. If you can't connect, it's likely that the server is down for testing. You can also run your own server, as the code that is running is the sample code found in the NAT punchthrough project.</p>
<p><em>The code is located at DependentExtensions\IrrlichtDemo</em></p></td>
</tr>
</tbody>
</table>

|---------------|
|  Dependencies |

|----------------------------------------------------------------------------------------------------------------------------------------|
| Irrlicht must be installed. It assumes it was installed to c:\\irrlicht-1.6. It also requires irrKlang for sound, provided by default. |

|-------------------------|
| ![](spacer.gif)See Also |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="replicamanager3.html">ReplicaManager3</a><br /> <a href="index.html">Index</a><br /></p></td>
</tr>
</tbody>
</table>
