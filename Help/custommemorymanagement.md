<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|--------------------------------------------------------------------------------|
| ![](spacer.gif)<span class="RakNetWhiteHeader">Custom Memory Management</span> |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Override new, delete, malloc, free, and realloc
<p>Users wishing to provide custom memory management functions can do so via RakMemoryOverride.cpp.</p>
<p>There are 3 global pointers defined in this file, with predefined defaults:</p>
<p>void* (*rakMalloc) (size_t size) = RakMalloc;<br /> void* (*rakRealloc) (void *p, size_t size) = RakRealloc;<br /> void (*rakFree) (void *p) = RakFree;</p>
<p>To override, simply set the values of these variables to something else.</p>
<p>For example, to override malloc, you may write:</p>
<p>#include &quot;RakMemoryOverride.h&quot;</p>
<p>void *MyMalloc(size_t size)<br /> {<br /> return malloc(size);<br /> }</p>
<p>int main()<br /> {<br /> rakMalloc=MyMalloc;<br /> // ...<br /> }</p>
<p>Then edit the file RakNetDefinesOverrides.h and add</p>
<p>#define _USE_RAK_MEMORY_OVERRIDE 1</p>
<p>Alternatively, edit RakNetDefines.h _USE_RAK_MEMORY_OVERRIDE</p></td>
</tr>
</tbody>
</table>

|----------------------------------------------------------------|
| ![](spacer.gif)<span class="RakNetWhiteHeader">See Also</span> |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="index.html">Index</a><br /> <a href="networkidobject.html">NetworkIDObject</a></p></td>
</tr>
</tbody>
</table>
