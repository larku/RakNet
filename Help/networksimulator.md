<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|----------------------------|
| Network Simulator Overview |

|-----------------------------------------------------------------------------------------------------------------------------|
| <span class="RakNetBlueHeader">Description</span>                                                                           
  If you are using RakNet locally, chances are you will not have real-world problems like packet loss and ping fluctuations.  
  RakNet provides some functions to simulate those problems.                                                                  |

|-----------------------------|
| Network Simulator Functions |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetCode">ApplyNetworkSimulator( double maxSendBPS, unsigned short minExtraPing, unsigned short extraPingVariance)</span><br /><br /> Enables simulated ping and packet loss to the outgoing data flow. To simulate bi-directional ping and packet loss, you should call this on both the sender and the recipient, with half the total ping and maxSendBPS value on each.
<p><em>maxSendBPS</em> - Maximum bits per second to send. Packetloss grows linearly. 0 to disable.<br /> <em>minExtraPing</em> - The minimum time to delay sends.<br /> <em>extraPingVariance</em> The additional random time to delay sends.<br /><br /> <span class="RakNetCode">bool IsNetworkSimulatorActive( void )</span><br /><br /> Returns if you previously called ApplyNetworkSimulator<br /><br /><br /> For a better solution, see</p>
<p><a href="http://www.jenkinssoftware.com/raknet/forum/index.php?topic=1671.0" class="uri">http://www.jenkinssoftware.com/raknet/forum/index.php?topic=1671.0</a></p>
<table>
<tbody>
<tr class="odd">
<td align="left"><img src="spacer.gif" />See Also</td>
</tr>
</tbody>
</table>
<table>
<tbody>
<tr class="odd">
<td align="left"><a href="index.html">Index</a><br /></td>
</tr>
</tbody>
</table></td>
</tr>
</tbody>
</table>
