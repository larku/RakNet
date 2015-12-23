<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-----------------------------|
|  Ogre 3D Interpolation Demo |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Demonstrates 3D interpolation</span><br /><br /> The Ogre 3D interpolation demo uses the graphics engine Ogre 3D to render popcorn popping.<br /><br /> The server has a bunch of popcorn kernels, which pops and flies around. After a short while all the popcorn is deleted and respawned.<br /><br /> The client is a dumb client in that the client does not do any physics or handle the details of spawning or popping the corn kernels.<br /><br /> <em>Specific to Ogre:</em><br /><br /> How to interpolate between the real and the visual position using a helper class TransformationHistory. Given a time in the past, it will tell you where you were then, using interpolation. If you hold down space you can see the client run non-interpolated, which is very choppy indeed, as it only sends 4 times a second. Let go of space and it's smooth again.<br /><br /> <em>Covered as part of RakNet:</em><br /><br /> ReplicaManager3 class, which automatically handles the networking for creation, destruction, and serialization of the popcorn kernels.<br /><br /> To run it, start two instances on the same computer. Press 's' for server on one instance, 'c' for client on the other. Hold down space to see the client run without interpolation.<br /><br /> If you want to run it over the internet. change the hardcoded SERVER_IP variable to the server's address.</p>
<p><em>The code is located at DependentExtensions\Ogre3DInterpDemo</em></p></td>
</tr>
</tbody>
</table>

|---------------|
|  Dependencies |

|-----------------------------------------------------------------------------------------------------------------------------------------|
| Ogre 3D must be installed. It assumes you have OGRE\_SDK as an environment variable. If not, change the project properties accordingly. |

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
