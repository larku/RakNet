<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|------------------------------------------|
| ![](spacer.gif)Data Compression Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">Description </span><br /><br /> RakNet can automatically compress all your outgoing data and decompress your incoming data. To do this, it needs a 'sample' frequency table for your average game so it can pre-compute how to encode the data to get maximum savings. Here is the general process of how to go about this:
<ol>
<li>Run a sample 'average' game. Get the frequency table for the server and for one of the clients (or average all the clients if you want).</li>
<li>Generate the decompression layer for the server from the client's frequency table</li>
<li>Generate the compression layer for the server from the server's frequency table</li>
<li>Generate the decompression layer for the client from the server's frequency table</li>
<li>Generate the compression layer for the client from the client's frequency table.</li>
</ol>
After that everything is handled automatically.<br /><br /> The functions are described below. See Samples\Compression for a full example.</td>
</tr>
</tbody>
</table>

|-------------------------------------------|
| ![](spacer.gif)Data Compression Functions |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetCode">SetCompileFrequencyTable( bool doCompile )</span><br /><br /> Enables or disables frequency table tracking. This is required to get a frequency table, which is used in GenerateCompressionLayer() This value persists between connect calls and defaults to false (no frequency tracking) You can call this at any time - however you SHOULD only call it when disconnected. Otherwise you will only trackpart of the values sent over the network.<br /><br /> <span class="RakNetCode">GenerateCompressionLayer(unsigned long inputFrequencyTable[256], bool inputLayer)</span><br /><br /> This is an optional function to generate the compression layer based on the input frequency table, which you get with GetOutgoingFrequencyTable.You should call this twice - once with inputLayer as true and once as false. The frequency table passed here with inputLayer=true should match the frequency table on the recipient with inputLayer=false. Likewise, the frequency table passed here with inputLayer=false should match the frequency table on the recipient with inputLayer=true. Calling this function when there is an existing layer will overwrite the old layer.<br /><br /> <span class="RakNetCode">DeleteCompressionLayer(bool inputLayer)<br /> </span><br /> Delete the output or input layer as specified. This is not necessary to call and is only valuable for freeing memory. You should only call this when disconnected<br /><br /> <span class="RakNetCode">GetOutgoingFrequencyTable(unsigned long outputFrequencyTable[256]) </span><br /><br /> Returns the frequency of outgoing bytes into output frequency table The purpose is to save to file as either a master frequency table from a sample game session for passing to GenerateCompressionLayer() . You should only call this when disconnected. Requires that you first enable data frequency tracking by calling SetCompileFrequencyTable(true)<br /><br /> <span class="RakNetCode">float GetCompressionRatio </span><br /><br /> This returns a number n &gt; 0.0f where lower numbers are better. n == 1.0f means your data is no smaller or greater than the original. This shows how effective your compression rates are.<br /><br /> <span class="RakNetCode">float GetDecompressionRatio </span><br /><br /> This returns a number n &gt; 0.0f where higher numbers are better. n == 1.0f means the incoming data was decompressed to be just as large as it was when it came in. This shows how effective your compression rates are.<br /><br />
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
