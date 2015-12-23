<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|------------------------|
|  Email Sender Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Simple class to send emails via C++</span><br /><br /> The EmailSender class, found at EmailSender.h, is a simple class with only one function, Send(...), to send an email using a provided mail server. It is used internally by the <a href="crashreporter.html">CrashReporter</a> class to send emails for unmonitored servers. See EmailSender.h for a full description of each parameter.</p>
<p>See the project Samples/SendEmail for a sample of this</p>
<p>The class has also been tested to work with Gmail POP servers, so if you have a Gmail account you can send emails without using your own mail server. The sample has the settings you need by default. You will also need to uncomment OPEN_SSL_CLIENT_SUPPORT in RakNetDefines.h, as Gmail requires the TCP connection to be made with SSL.</p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="index.html">Index</a><br /> <a href="crashreporter.html">Crash Repoter</a><br /></p></td>
</tr>
</tbody>
</table>
