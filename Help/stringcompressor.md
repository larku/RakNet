<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|----------------------------|
|  StringCompressor overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Encode and decode strings safely</span><br /><br /> The StringCompressor class, found in StringCompressor.h, will encode and decode a given string in a safe fashion, protecting from overruns.</p>
<p>Sender:</p>
<p>const char *str = &quot;My string&quot;;<br /> stringCompressor-&gt;EncodeString(str,TRUNCATION_LENGTH,&amp;bitStream,languageId);</p>
<p>Receiver:</p>
<p>char buffer[TRUNCATION_LENGTH];<br /> stringCompressor-&gt;DecodeString(buffer, TRUNCATION_LENGTH, &amp;bitStream, languageId);</p>
<p>The first parameter is the string to encode/decode. The second parameters is the maximum number of characters to read/write. If the string is longer than this, only that many bytes will be sent/read. The third parameter is the bitstream to write to or read from to. The last parameter indicates what character frequency table to use, which must be the same on both systems.</p>
<p>The string will be compressed using Huffman encoding based on the indicated frequency table, denoted by languageId. The default frequency table at index 0 is statically defined in StringCompressor.h with the varaible englishCharacterFrequencies. In order to add your own frequency tables, call GenerateTreeFromStrings() with the desired language ID.</p>
<p>If your application is using the CString class, you can enable support for this by defining _CSTRING_COMPRESSOR in StringCompressor.h.</p>
<p>Similarly, if your application is using std::string, you can enable support for this by defining _STD_STRING_COMPRESSOR in StringCompressor.h</p></td>
</tr>
</tbody>
</table>

|-----------------------|
|  StringTable overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Predefine static strings to reduce bandwidth</span><br /></p>
<p>The StringTable class is identical to the StringCompressor class, with the addition of the AddString function.</p>
<p>void AddString(const char *str, bool copyString);</p>
<p>str is the string to add.<br /> copyString should be true if your string is not persistent, false if is statically in memory (only a pointer will be stored).</p>
<p>The AddString will check an internal array to see if this string has already been registered. If not, it will internally store a two byte identifier representing this string. Further sends using the StringTable class will only send that two byte identifier, rather than the whole string, which is faster and takes less bandwidth if your string is 3 or more characters long. If you send a string which has not been added with AddString(), the function acts the same as if you called stringCompressor, but at the cost of 1 extra bit.</p>
<p>Both systems must have the same set of strings registered in advance, in the same order, and both systems must use StringTable and StringCompressor in the corresponding send and receive calls. You cannot mix and match the two.<br /></p></td>
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
<td align="left"><p><a href="index.html">Index</a><br /> DS_HuffmanEncodingTree.h<br /></p></td>
</tr>
</tbody>
</table>
