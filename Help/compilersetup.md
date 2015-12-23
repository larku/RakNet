![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)
|---------------------------------|
| ![](spacer.gif)Before you begin |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Use the source, static lib ,or a DLL?<br /><br /> RakNet includes the source, along with the projects to make the DLL and static lib, and samples. It no longer includes pre-built DLLs or libraries - this is due to incompatibilities across compilers. We recommend using the source directly, however you can also use the DLL or static lib projects to build your own.
Projects to build the DLL/Static Lib, and the samples, are provided for Microsoft Visual Studio .Net 2003 and 2005. Users of other compilers will have to make their own projects.<br /></td>
</tr>
</tbody>
</table>

|--------------------------|
| ![](spacer.gif)DLL Users |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>Creating a DLL</p>
<ol>
<li>Create a DLL project. I'll assume you know how to do this. In MSVC 7 you would create an empty project, then under Application Settings you check DLL and empty project.</li>
<li>Add the source files under the /Source directory to the project.</li>
<li>Add to &quot;Additional Include Directories&quot; your directory with the source files.</li>
<li>Import ws2_32.lib, or wsock32.lib if you don't have Winsock 2 installed. In MSVC 7 you can right click on the project, select configuration properties / linker / input / additional dependencies and type &quot;ws2_32.lib&quot; in there.</li>
<li>Set your project to use multi-threaded runtime libraries. In MSVC 7 you can right click on the project, select configuration properties / C/C++ / Code Generation / Runtime Library and change it to Multi-threaded (/MT).</li>
<li>Add _RAKNET_DLL to the Preprocessor Definitions. In VS Project Properties -&gt; Configuration Properties -&gt; C/C++ -&gt; PreProcessor -&gt; Preprocessor Definitions</li>
<li>Set the character set to &quot;not set&quot;.In VS Project Properties -&gt; Configuration Properties -&gt; General-&gt; Character Set</li>
<li>Optionally set your <a href="preprocessordirectives.html">preprocessor directives.</a></li>
<li>Then hit F7 or the equivalent to build your DLL and Lib.</li>
</ol>
<table>
<tbody>
<tr class="odd">
<td align="left"><a href="makedll.jpg"><img src="makedllsmall.jpg" /></a><br /></td>
</tr>
<tr class="even">
<td align="left"><strong>Creating an empty DLL project in .net 2003</strong></td>
</tr>
</tbody>
</table>
<table>
<tbody>
<tr class="odd">
<td align="left"><a href="multithreadeddebug.jpg"><img src="multithreadeddebugsmall.jpg" /></a><br /></td>
</tr>
<tr class="even">
<td align="left"><strong>Setting Multithreaded debug in .net 2003</strong></td>
</tr>
</tbody>
</table>
<table>
<tbody>
<tr class="odd">
<td align="left"><a href="ws2_32include.jpg"><img src="ws2_32includesmall.jpg" /></a><br /></td>
</tr>
<tr class="even">
<td align="left"><strong>Including ws2_32.lib in .net 2003</strong></td>
</tr>
</tbody>
</table>
<p>Game Setup</p>
<ol>
<li>Copy the DLL you just created to your game in the same directory as the exe. The lib can be in any directory you want.</li>
<li>Add the .lib to your project</li>
<li>Add all header files from /Source to your project</li>
</ol>
<br /> If you want to jump right in, refer to the <a href="tutorial.html">Basic code tutorial</a><br /> For more detail, refer to <a href="detailedimplementation.html">Detailed Implementation</a></td>
</tr>
</tbody>
</table>

|-----------------------------|
| ![](spacer.gif)Source users |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">Game Setup</span><br /><br />
<ol>
<li>Add the files under the /Source directory to the project. While not all of these are strictly necessary, the ones you don't use won't hurt you.</li>
<li>Import ws2_32.lib, or wsock32.lib if you don't have Winsock 2 installed. In MSVC 7 you can right click on the project, select configuration properties / linker / input / additional dependencies and type &quot;ws2_32.lib&quot; in there.</li>
<li>Set your project to use multi-threaded runtime libraries. In MSVC 7 you can right click on the project, select configuration properties / C/C++ / Code Generation / Runtime Library and change it to Multi-threaded (/MT).</li>
<li>Set an additional include path to include the RakNet source (if you copied the source files to a different directory).</li>
<li>Optionally set your <a href="preprocessordirectives.html">preprocessor directives.</a></li>
</ol>
<table>
<tbody>
<tr class="odd">
<td align="left"><a href="multithreadeddebug.jpg"><img src="multithreadeddebugsmall.jpg" /></a><br /></td>
</tr>
<tr class="even">
<td align="left"><strong>Setting Multithreaded debug in .net 2003</strong></td>
</tr>
</tbody>
</table>
<table>
<tbody>
<tr class="odd">
<td align="left"><a href="ws2_32include.jpg"><img src="ws2_32includesmall.jpg" /></a><br /></td>
</tr>
<tr class="even">
<td align="left"><strong>Including ws2_32.lib in .net 2003</strong></td>
</tr>
</tbody>
</table>
<br /></td>
</tr>
</tbody>
</table>

|---------------------------------------|
| ![](spacer.gif)3rd party dependencies |

 <span class="RakNetBlueHeader">Projects that won't compile by default</span>
 Not all projects will build without installing 3rd party dependencies. These are not required to use the majority of RakNet, however you will be missing specific features or demos without them.
 [3rd party dependencies page](dependencies.html)
|-----|
|     |

|-------------------------|
| ![](spacer.gif)See Also |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="index.html">Index</a><br /> <a href="dependencies.html">3rd party dependencies</a><br /> <a href="introduction.html">Introduction</a><br /> <a href="systemoverview.html">System Overview</a><br /> <a href="detailedimplementation.html">Detailed Implementation</a><br /> <a href="tutorial.html">Tutorial</a><br /> <a href="preprocessordirectives.html">Preprocessor directives</a><br /></p></td>
</tr>
</tbody>
</table>
