<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-------------------------|
| ![](spacer.gif)RakVoice |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Real time voice communication </span><br /><br /> RakVoice is a feature of RakNet that allows real time voice communication at a cost of only ~2200 bytes per second at 8000 16 bit samples per second. It uses Speex (<a href="http://www.speex.org/" class="uri">http://www.speex.org/</a>) to do the encoding. RakVoice is plugin class that makes it easier to encode, send, decode, and relay raw sound data.<br /> Note that RakVoice is not part of the RakNet core, so you'll need to add it to your project.<br /><br /> To get an instance of RakVoice just allocate one with new, or have global object handy.<br /><br /> RakVoice rakVoice;<br /><br /> Since RakVoice is a plugin, you need to attach it to a peer.<br /><br /> rakPeer-&gt;AttachPlugin(&amp;rakVoice);<br /><br /> initialize the class with the sample rate and the size of the buffer to process. A buffer size of 512 bytes is a reasonable value when using frequencies of 8000hz.<br /> The buffer size is the number of bytes to encode at a time, and the number of bytes returned by the decoder. This is normally the same size you want to lock the sound buffer by. Encoding will reduce the packet size by about 75%.<br /><br /> rakVoice.Init(8000, 512);<br /><br /> When data comes in on the sound buffer from the microphone, you should call SendFrame, passing the SystemAddress of the recipient system, and a pointer to the buffer to encode. Unlike normal API send calls, you cannot broadcast voice packets because each encoder and decoder is a matched pair. Therefore, you must always specify the SystemAddress so the sender knows which encoder to use. To broadcast, send it to everyone individually. Note that the size of the input buffer must match the buffer size we set earlier. For example:<br /><br /> rakVoice.SendFrame(recipientSystemAddress, (char*)inputBuffer);<br /><br /> In the other system, where data arrives, depending on the sound engine you are using, you'll probably have circular sound buffer, which you'll need to feed with the decoded data you receive from RakVoice. Every time your sound engine needs data to play, you should call ReceiveFrame. It will write a buffer of sound at the passed pointer, or just just silence if no new data was available. Once again, remember that the data returned has the same size you specified in Init (see above).<br /><br /> rakVoice.ReceiveFrame((char*)outbufffer);<br /></p>
The last point of note is that RakVoice requires all clients in a chat session to be aware of all other clients' connection states. The reasons for this are:
<ul>
<li>1. You need to call SendFrame with a specific recipient for broadcasting.</li>
<li>2. You may want to call CloseVoiceChannel, to stop communications with a particular system.</li>
</ul>
<br /> RakVoice only provides a means to encode and decode raw sound data, and means to communicate with the network. It does not include a mechanism to play or record sound. However, two samples are provived as an example on how to integrate with sound engines:<br /><br /> <em>\Samples\RakVoiceFMOD</em> - Provides an easy to use class to integrate RakVoice with the well known <a href="http://www.fmod.org/">FMOD</a> sound engine.<br /> <em>\Samples\RakVoice</em> - An example on how to integrate RakVoice with the free and open-source PortAudio sound engine.<br /><br /><br /> <strong>The source to PortAudio (<a href="http://www.portaudio.com/" class="uri">http://www.portaudio.com/</a>) and Speex (<a href="http://www.speex.org/" class="uri">http://www.speex.org/</a>) are included in RakNet and can be found in the root directory for rebuilding on other platforms. These are independent open-source APIs and are not owned by me nor do I provide support for them. Please refer to the respective webpages for more information on these APIs and the included license agreements for them.</strong></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|---------------------|
| [Index](index.html) |
