<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-------------------------|
|  Packet Logger Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Log incoming and outgoing messages for debugging</span><br /><br /> The PacketLogger is a plugin that will print all incoming and outgoing messages for debugging. It parses the message where necessary, indicating if a message is an RPC, or a timestamp. It also converts the numerical MessageID into the corresponding string. The output by default is comma delineated, readable as a <a href="http://en.wikipedia.org/wiki/Comma-separated_values">CSV file</a>, and goes to the console with printf().</p>
<p>To change the output destination, derive from PacketLogger and override WriteLog();</p>
<p>Aside from PacketLogger itself, the following implementations are already included:</p>
<ul>
<li>PacketConsoleLogger - For use with the ConsoleServer.</li>
<li>PacketFileLogger - Logs to a file. Call StartLog() to open the file.</li>
<li>ThreadsafePacketLogger - Same as PacketLogger, but delays WriteLog() until out of the RakNet thread. Use this if you do anything significant with the logs (anything other than printf).</li>
</ul></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|---------------------|
| [Index](index.html) |
