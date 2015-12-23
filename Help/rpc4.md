<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|----------------------------|
| ![](spacer.gif)RPC4 Plugin |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Call C functions on local and remote systems.</span></p>
<p>RPC4 contains mappings between string identifiers for functions, and a pointer to those functions.</p>
<p><strong>Registering functions:</strong></p>
<p>To Register a function, use the RegisterSlot() or RegisterBlockingFunction() members.</p>
<p>void RegisterSlot(const char *sharedIdentifier, void ( *functionPointer ) ( RakNet::BitStream *userData, Packet *packet ), int callPriority);<br /> bool RegisterBlockingFunction(const char* uniqueID, void ( *functionPointer ) ( RakNet::BitStream *userData, RakNet::BitStream *returnData, Packet *packet ));</p>
<p>The first parameter is a string representing that function. It can be the same as the name of the function. The second parameter is a pointer to the function to be called. If it is a blocking function, the parameter list also contains a BitStream to return data to the caller.</p>
<p>The class RPC4GlobalRegistration can be used to register functions where they are declared. For example:</p>
<p>void CFunc1( RakNet::BitStream *bitStream, Packet *packet ) {}<br /> RPC4GlobalRegistration cfunc1reg(&quot;CFunc1&quot;, CFunc1);</p>
<p>If you use RPC4GlobalRegistration extensively, you may need to change RPC4_GLOBAL_REGISTRATION_MAX_FUNCTIONS to a higher number in RakNetDefines.h</p>
<p><strong>Calling functions:</strong></p>
<p>Use the Signal() function to call a non-blocking function. Use the CallBlocking() function otherwise.</p>
<p>void Signal(const char *sharedIdentifier, RakNet::BitStream * bitStream, PacketPriority priority, PacketReliability reliability, char orderingChannel, const AddressOrGUID systemIdentifier, bool broadcast, bool invokeLocal);<br /> bool CallBlocking( const char* uniqueID, RakNet::BitStream * bitStream, PacketPriority priority, PacketReliability reliability, char orderingChannel, const AddressOrGUID systemIdentifier, RakNet::BitStream *returnData );</p>
<p>Signal will call all functions registered with that identifier using RegisterSlot(), including potentially on the same system. CallBlocking() will call a single function on a single system, registered with RegisterBlockingFunction(). CallBlocking() does not return until the remote system replies or the connection to that system is lost.</p>
<p>See <em>Samples/RPC4</em> for a demonstration of this plugin</p></td>
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
<td align="left"><p><a href="index.html">Index</a><br /> <a href="RPC3Video.htm">RPC3</a><a href="readyevent.html"></a><br /></p></td>
</tr>
</tbody>
</table>
