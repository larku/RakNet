<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-----------------------------------|
| Directory Delta Transfer Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Send file differences between directories automatically</span><br /><br /> DirectoryDeltaTransfer.h is useful if you allow user-moddable content. For example, if each server has a /skins directory, you could run this plugin to upload that directory to the clients. Each client that does not already have a particular skin will get it. You will get download progress notifications via a user-supplied callback. DirectoryDeltaTransfer relies on the FileListTransfer plugin to actually transmit hte files.</p>
<p>Usage:</p>
<ol>
<li>Attach the plugin and connect to the remote system</li>
<li>Server and client: Call directoryDeltaTransfer.SetFileListTransferPlugin(&amp;fileListTransfer);</li>
<li>Server: set the application directory: <span class="style1"><code> directoryDeltaTransfer.SetApplicationDirectory(&quot;c:\myGame&quot;);</code></span></li>
<li>Server: set the download directory: <span class="RakNetCode">directoryDeltaTransfer.AddUploadsFromSubdirectory(&quot;skins&quot;);</span></li>
<li>Client: to download call: <span class="RakNetCode">directoryDeltaTransfer.DownloadFromSubdirectory(&quot;skins&quot;, &quot;downloaded\skins&quot;, true, serverAddress, &amp;transferCallback, HIGH_PRIORITY, 0);</span></li>
<li>Client: Wait for the callback member <span class="RakNetCode">OnFileProgress()</span>. When onFileStruct-&gt;fileIndex is equal to onFileStruct-&gt;setCount this download is done.</li>
</ol>
<p>For full details on all parameters and other available functions, see the header file DirectoryDeltaTransfer.h and the sample at <em>Samples/DirectoryDeltaTransfer</em></p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|--------------------------------------------|
| [Index](index.html)                        
  [FileListTransfer](filelisttransfer.html)  |
