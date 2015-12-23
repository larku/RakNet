<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|---------------------------|
| ![](spacer.gif)Statistics |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">How to read and interpret RakNet's statistical data </span><br /><br /> Statistical data is important for an online game because it lets you see where your traffic bottlenecks are.</p>
<p>RakPeerInterface provides the structure RakNetStatistics which is returned by the GetStatistics() function present in RakPeerInterface. This structure is defined in <em>Source/RakNetStatistics.h</em>. The function StatisticsToString is also provided which will convert these statistics to a formatted buffer.</p>
<p>A running total is kept for the following enumerations</p>
<p>/// How many bytes per pushed via a call to RakPeerInterface::Send()<br /> USER_MESSAGE_BYTES_PUSHED,</p>
<p>/// How many user message bytes were sent via a call to RakPeerInterface::Send(). This is less than or equal to USER_MESSAGE_BYTES_PUSHED.<br /> /// A message would be pushed, but not yet sent, due to congestion control<br /> USER_MESSAGE_BYTES_SENT,</p>
<p>/// How many user message bytes were resent. A message is resent if it is marked as reliable, and either the message didn't arrive or the message ack didn't arrive.<br /> USER_MESSAGE_BYTES_RESENT,</p>
<p>/// How many user message bytes were received, and returned to the user successfully.<br /> USER_MESSAGE_BYTES_RECEIVED_PROCESSED,</p>
<p>/// How many user message bytes were received, but ignored due to data format errors. This will usually be 0.<br /> USER_MESSAGE_BYTES_RECEIVED_IGNORED,</p>
<p>/// How many actual bytes were sent, including per-message and per-datagram overhead, and reliable message acks<br /> ACTUAL_BYTES_SENT,</p>
<p>/// How many actual bytes were received, including overead and acks.<br /> ACTUAL_BYTES_RECEIVED,</p>
<p>If you want to track statistics over time, we also provide <em>Source/StatisticsHistoryPlugin.h,</em> used by the sample <em>StatisticsHistoryTest</em>. It tracks values for some user-defined amount of time and does various calculations on the data set. RakPeerInterface::GetStatistics() is read automatically. Here is sample code that tracks a sin and cos wave.</p>
<p>DataStructures::Queue&lt;StatisticsHistory::TimeAndValue&gt; histogram;<br /> StatisticsHistory::TimeAndValueQueue *tav;<br /> StatisticsHistory::TimeAndValueQueue tavInst;<br /> StatisticsHistory statisticsHistory;<br /> statisticsHistory.SetDefaultTimeToTrack(10000);<br /> statisticsHistory.AddObject(StatisticsHistory::TrackedObjectData(HO_SIN_WAVE,0,0));<br /> statisticsHistory.AddObject(StatisticsHistory::TrackedObjectData(HO_COS_WAVE,0,0));<br /> while (1) {<br /> double f = (double) ((double)GetTime() / (double)1000);<br /> statisticsHistory.AddValueByObjectID(HO_SIN_WAVE,&quot;Waveform&quot;,sin(f),GetTime(), false);<br /> statisticsHistory.AddValueByObjectID(HO_COS_WAVE,&quot;Waveform&quot;,cos(f),GetTime(), false);<br /> RakSleep(30);<br /> }<br /></p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|---------------------|
| [Index](index.html) |
