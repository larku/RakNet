<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-----------------------------|
| ![](spacer.gif)Timestamping |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">How to refer to events on different computers in the same timeframe</span><br /><br /> A timestamp is nothing more than the local system time. Unfortunately, every system has a different local system time. If you were simply to send the system time over the network you'd get the value of the other machine's clock which tells you nothing about when the event happened because you only know your own system time, not anyone else's. The timestamping feature of RakNet allows times to be read relative to your own system, allowing you to concentrate on the game and not worry about the local times of other systems. It does this transparently and automatically, and gives a fairly high degree of precision even with fluctuating pings.<br /><br />
<img src="timestamp.jpg" />
<br /><br /> Suppose an event were to happen on a client where the local system time on that client is 2000, the local system time on the server was 12000, and the local system time on another client was 8000. If the packet were not time-stamp adjusted the server would get the time 2000, or 10000 ms ago when actually it happened ping / 2 ms ago, which is probably around 100. Likewise, the client would get 2000 which would be 6000 ms ago from its own system time from 8000.<br /><br /> Fortunately, RakNet deals with this for you, compensating for both system time and ping. Using relative time, the server would see that the event happened roughly to ping/2 ms ago, and the other client would see the event happened roughly ping/2 ms (relative to each client) ago. To make a long story short, you can just use timestamps as long as you encode your packets correctly and don't have to worry about it otherwise.<br /><br /> See <a href="creatingpackets.html">Creating Packets</a> for instructions on how to encode timestamps into your packets.<br /><br /> Note: It's recommended that you use the time functions found in GetTime.h to get your system time as this is a high resolution timer. You can also use the windows function timeGetTime(), but this is less accurate. Timestamping also depends on automatic pinging, so you will need to call the function SetOccasionalPing for it to maintain accuracy.</td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|---------------------|
| [Index](index.html) |
