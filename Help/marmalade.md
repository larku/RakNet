<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-------------------------------------------------------------------------|
| ![](spacer.gif)<span class="RakNetWhiteHeader">Marmalade support</span> |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">How to integrate with the Marmalade SDK </span>
<p><a href="http://www.madewithmarmalade.com/">Marmalade</a> is a SDK that enables you to write games for the IOS and Android using Native C++. It is not a game engine, although it includes graphic libraries and other tools. As Marmalade can compile Native C++, it can compile RakNet and therefore enable you to use RakNet on those platforms in a consistent way.</p>
<p><strong>Step 1 - Download Marmalde:</strong></p>
<p><a href="http://www.madewithmarmalade.com/downloads">Download</a> and install Marmalade Beta 5.1 or later. This requires registration and other steps with Marmalade.</p>
<p><strong>Step 2 - Create RakNet solution:</strong></p>
<p>Assuming you have RakNet downloaded, go to DependentExtensions\Marmalade and double click RakNet.mkb. If Marmalade is installed correctly, this will create a directory build_raknet_vc9 or similar. If necessary to add or remote RakNet source files, edit in a text editor the .mkb and .mkf files where the RakNet source files are listed and double click the .mkb again.</p>
<p><strong>Step 3 - Build RakNet library:</strong></p>
<p>Find the .sln solution file in the directory created in step 2. Open it, and build for all platforms you care about. Build / batch build / select all / build will do this as well. Assuming this worked, you will now have object files created in a directory such as DependentExtensions\Marmalade\build_raknet_vc9\Debug_RakNet_vc9_x86</p>
<p><strong>Step 4 - Link RakNet to your application:</strong></p>
<p>Add these two lines to your application .mkb file</p>
<p>option module_path=&quot;../../DependentExtensions/Marmalade&quot;<br /> subproject RakNet</p>
<p>The path under option module_path should modified to point to wherever you installed RakNet. There is an example of this under Samples\Marmalade . After doing so you will need to double click the .mkb file to regenerate your project solution.</p>
<p>If, upon building RakNet, you get a build error &quot;unresolved external symbol _strtoull ...&quot; then you need to either update Marmalade to a newer version, or comment out the last strtoull in RakNetGUID::FromString in RakNetTypes.h, then build step 3 again.</p>
<p><strong>Step 5 - Fix allocator:</strong></p>
The Marmalade bucket system is not threadsafe. Be sure you have this code in main()
<pre><code>#include &quot;RakMemoryOverride.h&quot;

void* MarmaladeMalloc(size_t size)
{
    return s3eMallocBase(size);
}
void* MarmaladeRealloc(void *p, size_t size)
{
    return s3eReallocBase(p, size);
}
void MarmaladeFree(void *p)
{
    s3eFreeBase(p);
}
SetMalloc(MarmaladeMalloc);
SetRealloc(MarmaladeRealloc);
SetFree(MarmaladeFree);</code></pre></td>
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
<td align="left"><p><a href="index.html">Index</a></p></td>
</tr>
</tbody>
</table>
