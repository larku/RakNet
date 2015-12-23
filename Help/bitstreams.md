<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-----------------------------------|
| ![](spacer.gif)Bitstream Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">Description</span><br /><br /> The bitstream class is a helper class under the namespace RakNet that is used to wrap a dynamic array for the purpose of packing and unpacking bits. Its main four benefits are:
<ol>
<li>Creating packets dynamically</li>
<li>Compression</li>
<li>Writing Bits</li>
<li>Endian swapping</li>
</ol>
<p>With structs you have to predefine your structures and cast them to a (char*). With a bitstream, you can choose to write blocks at runtime, depending on the context. Bitstreams can compress the native types using the SerializeBitsFromIntegerRange and SerializeFloat16().<br /><br /> You can also write bits. Most of the time you will not care about this. However, when writing booleans it will automatically only write one bit. This can also be very useful for encryption since your data will no longer be byte aligned.<br /><br /> <strong>Writing Data</strong></p>
<p>Bitstream is templated to take any type of data. If this is a built-in type, such as NetworkIDObject, it uses partial template specialization to write the type more efficiently. If it's a native type, or a structure, it writes the individual bits, similar to memcpy. You can pass structs containing multiple data members to bitstreams. However, you may wish to serialize each individual element to do correct <a href="http://en.wikipedia.org/wiki/Endianness">endian</a> swapping (needed for communication between PCs and Macs, for example).<br /><br /> <span class="RakNetCode"> struct MyVector<br /> {<br /> float x,y,z;<br /> } myVector;<br /><br /> // No endian swapping<br /> bitStream.Write(myVector);<br /><br /> // With endian swapping<br /> #undef __BITSTREAM_NATIVE_END<br /> bitStream.Write(myVector.x);<br /> bitStream.Write(myVector.y);<br /> bitStream.Write(myVector.z);<br /><br /> // You can also override operator left shift and right shift</span><br /> <span class="RakNetCode">// Shift operators have to be in the namespace RakNet or they might use the default one in BitStream.h instead. Error occurs with std::string<br /> namespace RakNet<br /> {</span></p>
<p><span class="RakNetCode"> RakNet::BitStream&amp; operator &lt;&lt; (RakNet::BitStream&amp; out, MyVector&amp; in)<br /> {<br /> out.WriteNormVector(in.x,in.y,in.z);<br /> return out;<br /> }<br /> RakNet::BitStream&amp; operator &gt;&gt; (RakNet::BitStream&amp; in, MyVector&amp; out)<br /> {<br /> bool success = in.ReadNormVector(out.x,out.y,out.z);<br /> assert(success);<br /> return in;<br /> }</span></p>
<p><span class="RakNetCode">} // namespace RakNet<br /><br /> // Read from bitstream<br /> myVector &lt;&lt; bitStream;<br /> // Write to bitstream<br /> myVector &gt;&gt; bitStream;<br /> </span><br /> Optional - One of the constructor versions takes a length in bytes as a parameter. If you have an idea of the size of your data you can pass this number when creating the bitstream to avoid internal reallocations.<br /><br /> See <a href="creatingpackets.html">Creating Packets</a> for more details.<br /><br /> <strong>Reading Data</strong><br /><br /> Reading data is equally simple. Create a bitstream, and in the constructor assign it your data.<br /><br /> <span class="RakNetCode"> // Assuming we have a Packet *<br /> BitStream myBitStream(packet-&gt;data, packet-&gt;length, false);<br /> struct MyVector<br /> {<br /> float x,y,z;<br /> } myVector;<br /><br /> // No endian swapping<br /> bitStream.Read(myVector);<br /><br /> // With endian swapping (__BITSTREAM_NATIVE_END should just be commented in RakNetDefines.h)<br /> #undef __BITSTREAM_NATIVE_END<br /> #include &quot;BitStream.h&quot; bitStream.Read(myVector.x);<br /> bitStream.Read(myVector.y);<br /> bitStream.Read(myVector.z);<br /><br /> </span> See <a href="receivingpackets.html">Receiving Packets</a> for a more complete example.<br /><br /> <strong>Serializing Data</strong><br /><br /> You can have the same function read and write, by using BitStream::Serialize() instead of Read() or Write().<br /><br /> <span class="RakNetCode"> struct MyVector<br /> {<br /> float x,y,z;<br /> // writeToBitstream==true means write, writeToBitstream==false means read<br /> void Serialize(bool writeToBitstream, BitStream *bs)<br /> {<br /> bs-&gt;Serialize(writeToBitstream, x);<br /> bs-&gt;Serialize(writeToBitstream, y);<br /> bs-&gt;Serialize(writeToBitstream, z);<br /> }<br /> } myVector;<br /> </span><br /> See <a href="receivingpackets.html">Receiving Packets</a> for a more complete example.<br /><br /></p></td>
</tr>
</tbody>
</table>

|---------------------------------|
| ![](spacer.gif)Useful functions |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">See BitStream.h for a full list of functions.</span><br /><br /> <strong>Reset</strong><br /> Restart the bitstream, clearing all data.</p>
<p><strong>Write functions</strong><br /> The write functions write data to the bitstream at the end of the bitstream. You should use the analogous Read to get the data back out.<br /><br /> <strong>Read functions</strong><br /> The read functions read data already in the bitstream, in order from beginning to end. The read function returns false if there is no more data in the bitstream.</p>
<p><strong>WriteBitsFromIntegerRange, ReadBitsFromIntegerRange, SerializeBitsFromIntegerRange<br /></strong> If a number only uses a specific range of values (such as 10-20) this function will determine automatically the number of bits needed to write that range of values.</p>
<p><strong>WriteCasted, ReadCasted</strong><br /> Write a variable of one type as if it were casted to another type. For example, WriteCasted&lt;char&gt;(5); is equivalent to char c=5; Write(c);</p>
<p><strong>WriteNormVector, ReadNormVector</strong><br /> Writes a normalized vector where each component ranges from -1 to +1. Each component is written with 16 bites.</p>
<p><strong>WriteFloat16, ReadFloat16<br /></strong> Given a min and max value for a floating point number, divide the range by 65535 and write the result in 16 bytes (lossy)..</p>
<p><strong>WriteNormQuat, ReadNormQuat</strong><br /> Write a quaternion in 16*3+4 bits (lossy).</p>
<p><strong>WriteOrthMatrix, ReadOrthMatrix</strong><br /> Convert an orthonormal matrix to a quaternion, then call WriteNormQuat/ReadNormQuat.</p>
<p><strong>GetNumberOfBitsUsed</strong><br /> <strong>GetNumberOfBytesUsed</strong><br /> Gives you the number of bytes or bits written.<br /><br /> <strong>GetData</strong><br /> Gives you a pointer to the internal data of the bitstream. This is a (char*) allocated with malloc and is presented in case you need direct assess to the data.</p>
<p><br /></p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|---------------------------------------------|
| [Index](index.html)                         
  [Creating Packets](creatingpackets.html)    
  [Receiving Packets](receivingpackets.html)  |
