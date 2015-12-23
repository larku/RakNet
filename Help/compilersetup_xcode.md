![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)
|----------------------------|
| ![](spacer.gif)Xcode notes |

|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Although Raknet is cross-platform, not all of the samples provided will compile/run on Mac OS X or iOS. Here I'll show you how to compile Raknet for Mac OS X and iOS, along with one of the samples for testing |

|-----------------------------------------------------------|
| ![](spacer.gif)Compiling as a static library for Mac OS X |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>Create an empty workspace</p>
<ol>
<li>Create a folder named <em>RakNetTests</em>, and then create a new empty Workspace. Name it <em>RakNetTests</em>, and save it inside the new folder<br /><br /> <img src="xcode_newworkspace.jpg" /><br /><br /></li>
</ol>
<p>Create the RakNet static library project</p>
<ol>
<li>Using <em>File-&gt;New-&gt;New Project</em>, create a new Mac OS X C/C++ library project<br /><br /> <a href="xcode_library.jpg"><img src="xcode_librarysmall.jpg" /></a><br /><br /> Name it <em>RakNet</em> and use the following options:<br /> <img src="xcode_libraryname.jpg" /><br /> Save it inside the same folder as the workspace<br /><br /></li>
<li>Using Finder, copy RakNet's source code (<em>Source</em> folder) to where RakNet project file was created<br /><br /> <img src="xcode_sourcefolder.jpg" /><br /><br /></li>
<li>Right-click the <em>RakNet</em> project, and select <em>Add Files to &quot;RakNet&quot;...</em> , and select the new <em>Source</em> folder you should have in the same folder as the <em>RakNet</em> Project file. Use the following options:<br /><br /> <img src="xcode_addfiles.jpg" /><br /><br /> This should create a <em>Source</em> group, like this:<br /> <img src="xcode_addfiles_newgroup.jpg" /><br /><br /></li>
<li>The files inside the <em>cat</em> folder aren't supposed to be compiled, so remove the <em>Source/cat</em> group from the project files.<br /> When prompted for the deletion method, pick <strong><em>Remove References Only</em></strong>.<br /> <img src="xcode_remove_cat.jpg" /><br /><br /></li>
<li>Build Raknet using <em>Product-&gt;Build</em><br /> You should get a successful compilation.</li>
</ol></td>
</tr>
</tbody>
</table>

|-------------------------------------------|
| ![](spacer.gif)Testing the static library |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>A simple sample...</p>
For testing, we're going to use the sample <em>&quot;Chat Example&quot;</em> provided with the Raknet. You can find it the <em>&quot;Samples/Chat Example&quot;</em> folder. This sample has 2 source files, named <em>&quot;Chat Example Server.cpp&quot;</em> and <em>&quot;Chat Example Client.cpp&quot;</em>. We are going to create two projects from those files (one project for the Server, and another for the Client).<br /><br /> Inside the folder where you have your workspace, create another folder named <em>Samples</em>, and copy <em>&quot;Chat Example Server.cpp&quot;</em> and <em>&quot;Chat Example Client.cpp&quot;</em> into that folder.<br /><br />
<p>The server</p>
<ol>
<li>Create a <em>Command Line Tool</em> project for the Server:<br /> <img src="xcode_newcommandlinetool.jpg" /><br /> In the next window where it asks for the options for your new project, name it <em>ChatExampleServer</em>, leave <em>Type</em> as <em>C++</em>, and <em>&quot;Use Automatic Reference Counting&quot;</em> unchecked.<br /> Save the project inside your <em>Samples</em> folder.<br /><br /></li>
<li>Inside the newly created ChatExampleServer, you should have a group named <em>ChatExampleServer</em>. Delete the <em>main.cpp</em> you'll find inside that group, and add the <em>&quot;Chat Example Server.cpp&quot;</em> file<br /><br /></li>
<li>Specify where to look for the RakNet header files, by changing the Build Settings of the ChatExampleServer project. This can be done in the <strong><em>Header Search Paths</em></strong> option, under the <strong><em>Search Paths</em></strong> section:<br /> <img src="xcode_headersearchpaths.jpg" /><br /> If your folder structure is exactly the same as the one used for this tutorial, then the search path should be what you see in the above image. If not then you need to adjust it accordingly.<br /> <strong>NOTE:</strong> The search path is relative to the project file's location.<br /><br /></li>
<li>Link <em>ChatExampleServer</em> project with our RakNet static library, by going to <strong><em>Build Phases</em></strong>, section <strong><em>Link Binary With Libraries</em></strong>, clicking the <strong>'+'</strong> button and picking our RakNet library as shown:<br /> <img src="xcode_linkwithlibrary.jpg" /><br /><br /></li>
<li>You should be able to successfully build and run the Server now.<br /></li>
</ol>
<p>The Client</p>
The steps to create the client project are the same as the ones for the Server:
<ol>
<li>Create a <em>&quot;Command Line Tool&quot;</em> project, and name it <em>ChatExampleClient</em></li>
<li>Delete the file <em>main.cpp</em> and add the file <em>&quot;Chat Example Client.cpp&quot;</em></li>
<li>Change the C/C++ compiler to <strong><em>LLVM GCC</em></strong></li>
<li>Set the header search paths</li>
<li>Add <em>RakNet</em> library to the list of libraries to link with.</li>
</ol>
<p>Running the sample</p>
You should now have 2 products ready to run (ChatExampleClient and ChatExampleServer). You can run one of them from inside Xcode, and run the other externally by right-clicking on it and selecting <strong>&quot;Open With External Editor&quot;</strong>.</td>
</tr>
</tbody>
</table>

|------------------------------------------------------|
| ![](spacer.gif)Compiling as a static library for iOS |

|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compiling Raknet as a static library for iOS is essentially the same as for Mac OS X.                                                                          
  You can create a new project for the iOS library, or you can just create another Target for your Mac OS X static library project, and change what SDK to use:  
  ![](xcode_changesdk.jpg)                                                                                                                                       
  You can find some iOS samples in the **Samples/iOS** folder.                                                                                                   |

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
