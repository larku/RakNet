![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)
|-------------------------|
| ![](spacer.gif)Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">What is Swig?</span><br /><br /> Swig is an application that generates wrapper code for a native DLL to interface with other languages allowing you to use a C/C++ library in one of the supported languages. Currently Swig configuration for RakNet is set up to generate a managed interface that can be used in managed C# in Windows, as well as in Linux with Mono.<br /><br /> Swig generates a CXX and .h file, which exposes interfaces that can be used by the target language. The CXX file is included when building the C++ dll that the target language  uses.<br /><br /> Swig also generates files in the target language for inclusion in the project of the target language to interface with the dll. These are added to the project of the target language.<br /><br /> <span class="RakNetBlueHeader">What is Mono?<br /><br /> </span>Mono is a cross platform implementation of the .Net framework. This allows you to compile C# and other .Net code and run them on platforms that originally are not .Net compatible like Linux.<br /><br /> <span class="RakNetBlueHeader">Choices to Make.<br /><br /> </span>You may choose to use the tools with RakNet or start from scratch.<br /><br /> On Windows batch tools are provided make the process easier. These tools are located under the Swig Directory under DependentExtensions. For Linux the tools will use Wget to pull down Swig and install version 2.0.00 if Swig is not installed.<br /><br /> You may also download Swig yourself and compile them by specifying the options manually. In the manual portion the options used will be explained.<br /><br /> <span class="RakNetBlueHeader">Step Summary<br /><br /> </span><span style="font-weight: bold;">Manually:<br /> </span>
<ol>
<li>Download Swig, see &quot;Downloading Swig&quot;</li>
<li>Generate the Swig files, see &quot;Generating the Swig Files Manually&quot;</li>
<li>Create the DLL, see &quot;Creating the Swig Wrapped DLL Project&quot;</li>
<li>Create the C# project. see  &quot;Creating the C# project&quot;</li>
</ol>
<span style="font-weight: bold;">Using tools:<br /><br /> *Windows<br /><br /> -Using DLL_Swig\RakNet.sln solution<br /> </span>
<ol>
<li>Download Swig, see section &quot;Downloading Swig&quot;</li>
<li>If you do not plan on using SQLiteClientLoggerPlugin, DependentExtensions\Swig\DLL_Swig\RakNet.sln already contains a DLL project that makes the Swig files, builds the DLL, and copies it to the C# sample. Perform a rebuild on this.</li>
<li>Create the C# project see  &quot;Creating the C# project&quot;</li>
</ol>
<span style="font-weight: bold;">-Not using DLL_Swig project</span><span style="font-weight: bold;"><br /> </span>
<ol>
<li>Download Swig, See section &quot;Downloading Swig&quot;</li>
<li>Generate the Swig files, see &quot;Generating the Swig Files Using Included Tools&quot;</li>
<li>Create the DLL, see &quot;Creating the Swig Wrapped DLL Project&quot;</li>
<li>Create the C# project see  &quot;Creating the C# project&quot;</li>
</ol>
<br /> <span style="font-weight: bold;">*Linux<br /> </span>
<ol>
<li>Download Swig, See section &quot;Downloading Swig&quot;.</li>
<li>Generate the Swig files, see &quot;Generating the Swig Files Using Included tools&quot;</li>
<li>Create the C# project see  &quot;Creating the C# project&quot;</li>
</ol>
<span class="RakNetBlueHeader"></span></td>
</tr>
</tbody>
</table>

|---------------------------------|
| ![](spacer.gif)Downloading Swig |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">Windows<br /><br /> Download Swig and Unzip.<br /><br /> </span>
<ol>
<li>Go to <a href="http://www.swig.org/" class="uri">http://www.swig.org/</a></li>
<li>On the left hand side click <a href="http://www.swig.org/survey.html">Download</a>.</li>
<li>Either fill out the survey or click the <a href="http://www.swig.org/download.html">Download area</a> link.</li>
<li>It will say Windows users should download &lt;link&gt; click on the link.</li>
<li>Unzip to C:\Swig or where you prefer C:\Swig will be used for this tutorial.</li>
<li>If you wish to use the DLL_Swig project you will need to add to the path variable as shown below.</li>
</ol>
<span class="RakNetBlueHeader"> Adding Swig to the Path Variable.<br /><br /> </span>
<ol>
<li>Right click My Computer and click Properties.</li>
<li>Click on the Advanced tab.</li>
<li>Under Advanced, there is an Environment Variables button click it.</li>
<li>Click the on the Path variable</li>
<li>Click the Edit button</li>
<li>Add the path C:\Swig or wherever you unzipped Swig to.<br /></li>
</ol>
<table>
<tbody>
<tr class="odd">
<td align="left"><a href="PathVariableShot.jpg"><img src="PathVariableShot.jpg" /></a><br /></td>
</tr>
<tr class="even">
<td align="left"><span style="font-weight: bold;">The path variable screen</span></td>
</tr>
</tbody>
</table>
<span class="RakNetBlueHeader">Linux</span><br /><br /> <span class="RakNetBlueHeader">Download Swig,Unzip,Make, and Install.<br /><br /> <span class="RakNetManualTextBody">Note: You may be able to install Swig using your favorite package manager, if you can go that route do so. The instructions vary depending on your package manager and distribution. The batch file will attempt to install 1.6, but it is recommended to use the package manager if you are able to.</span></span>
<ol>
<li>Go to <a href="http://www.swig.org/" class="uri">http://www.swig.org/</a></li>
<li>On the left hand side click <a href="http://www.swig.org/survey.html">Download</a>.</li>
<li>Either fill out the survey or click the <a href="http://www.swig.org/download.html">Download area</a> link.</li>
<li>It will say latest development release is &lt;link&gt; click on the link.</li>
<li>Open a terminal</li>
<li>Change to the directory you download swig into</li>
<li>type tar xzf swig-VERSIONNUMBER.tar.gz and  hit enter<br /></li>
<li>type cd VERSIONNUMBER and  hit enter</li>
<li>type ./configure and  hit enter</li>
<li>type make and  hit enter</li>
<li>switch to your root user, or if user is a sudo user skip to next step</li>
<li>if root type make install and hit enter, other wise type sudo make install and hit enter if user is a sudo user.<br /></li>
</ol></td>
</tr>
</tbody>
</table>

|-------------------------------------------------|
|  Generating the Swig Files Using Included Tools |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">Generating the Swig Files Using Included Tools on Windows.<br /><br /> </span>Note: This step on Windows can be skipped if you are using the included DLL_Swig project as it runs the tools on rebuild.<br />
<ol>
<li>Click the start menu and click on run. In Vista click start-&gt;search type &quot;run&quot; hit enter.</li>
<li>Type cmd and hit enter.</li>
<li>In the next command PATH_TO_RAKNET_SWIG_FILES is the path to the swig directory. For example C:\RakNet\DependentExtensions\Swig</li>
<li>Type cd PATH_TO_RAKNET_SWIG_FILES hit enter</li>
<li>In the next command PATH_TO_RAKNETSOURCE is the path to the swig source directory. For example: C:\RakNet\Source. PATH_TO_SWIG is an unquoted path with a trailing slash to the Swig directory Example: C:\Swig\. If you added swig to your path variable then PATH_TO_SWIG is not needed and can be ignored.</li>
<li>Enter MakeSwig.bat PATH_TO_RAKNETSOURCE PATH_TO_SWIG. For example, MakeSwig.bat c:\RakNet\Source c:\swigwin-2.0.9<br /></li>
<li>If you want to use SQLiteClientLoggerPlugin skip #6, the next steps will be used instead.</li>
<li>If you have added swig to your path variable, just use &quot;&quot; for PATH_TO_SWIG</li>
<li>PATH_TO_SQLITEPLUGIN is the path to the SQLite plugin directory. EX: C:\RakNet\DependentExtensions\SQLite3Plugin</li>
<li>Type MakeSwig.bat PATH_TO_RAKNETSOURCE PATH_TO_SWIG  PATH_TO_SQLITEPLUGIN hit enter</li>
<li>Goto &quot;Creating the Swig Wrapped DLL Project&quot;</li>
</ol>
<span class="RakNetBlueHeader">Generating the Swig Files Using Included Tools on Linux.<br /><br /> <span class="RakNetManualTextBody">Note: The Linux batch <span class="RakNetBlueHeader"></span><span class="RakNetBlueHeader"></span>requires Wget,Tar,Make and GCC  to be installed, unless swig is already installed. Most of the time </span></span><span class="RakNetBlueHeader"><span class="RakNetManualTextBody">Wget,Tar,Make and GCC are</span></span><span class="RakNetBlueHeader"><span class="RakNetManualTextBody"> already installed.</span><br /> </span>
<ol>
<li>Open a terminal if you are not already at one.<br /></li>
<li>In the next command PATH_TO_RAKNET_SWIG_FILES is the path to the swig directory. For example /home/usr/RakNet/DependentExtensions/Swig</li>
<li>Type cd PATH_TO_RAKNET_SWIG_FILES hit enter</li>
<li>Type chmod u+x MakeSwig.sh<br /></li>
<li>In the next command PATH_TO_RAKNETSOURCE is the path to the swig source directory. For example: /home/usr/RakNet/Source</li>
<li>Type ./MakeSwig.sh PATH_TO_RAKNETSOURCE hit enter</li>
<li>If you want to use SQLiteClientLoggerPlugin skip #6, the next steps will be used instead.<br /></li>
<li>PATH_TO_SQLITEPLUGIN is the path to the SQLite plugin directory. EX: /home/usr/RakNet/DependentExtensions/SQLite3Plugin<br /></li>
<li>Type MakeSwig.sh PATH_TO_RAKNETSOURCE PATH_TO_SQLITEPLUGIN hit enter<br /></li>
<li>Skip to &quot;Creating the C# project&quot;.   </li>
</ol></td>
</tr>
</tbody>
</table>

|-------------------------------------|
|  Generating the Swig Files Manually |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">Windows<br /> </span><span class="RakNetBlueHeader"><br /> </span> <span class="RakNetBlueHeader">Generate the Swig Files</span><br />
<ol>
<li>Click the start menu and click on run. In Vista click start-&gt;search type &quot;run&quot; hit enter.</li>
<li>Type cmd and hit enter.</li>
<li>In the next command PATH_TO_RAKNET_SWIG_FILES is the path to the swig directory. For example C:\RakNet\DependentExtensions\Swig</li>
<li>Type cd PATH_TO_RAKNET_SWIG_FILES hit enter</li>
<li>In the next command PATH_TO_RAKNETSOURCE is the path to the swig source directory. For example: C:\RakNet\Source</li>
<li>Type C:\Swig\swig -c++ -csharp -namespace RakNet -I&quot;PATH_TO_RAKNETSOURCE&quot; -I&quot;SwigInterfaceFiles&quot; -outdir SwigOutput\SwigCSharpOutput -o SwigOutput\CplusDLLIncludes\RakNet_wrap.cxx SwigInterfaceFiles\RakNet.i hit enter</li>
<li>If you want to use SQLiteClientLoggerPlugin skip #6</li>
<li>PATH_TO_SQLITEPLUGIN is the path to the SQLite plugin directory. EX: C:\RakNet\DependentExtensions\SQLite3Plugin</li>
<li>Type C:\Swig\swig -c++ -csharp -namespace RakNet -I&quot;PATH_TO_RAKNETSOURCE&quot; -I&quot;SwigInterfaceFiles&quot; -I&quot;PATH_TO_SQLITEPLUGIN&quot; -DSWIG_ADDITIONAL_SQL_LITE -outdir SwigOutput\SwigCSharpOutput -o SwigOutput\CplusDLLIncludes\RakNet_wrap.cxx SwigInterfaceFiles\RakNet.i hit enter </li>
</ol>
<span class="RakNetBlueHeader">Linux</span><br /> <span class="RakNetBlueHeader"><br /> </span> <span class="RakNetBlueHeader">Generate the Swig Files</span><br />
<ol>
<li>Open a terminal<br /></li>
<li>In the next command PATH_TO_RAKNET_SWIG_FILES is the path to the swig directory. For example /home/usr/RakNet/DependentExtensions/Swig</li>
<li>Type cd PATH_TO_RAKNET_SWIG_FILES hit enter</li>
<li>In the next command PATH_TO_RAKNETSOURCE is the path to the swig source directory. For example: /home/usr/RakNet/Source</li>
<li>Type swig -c++ -csharp -namespace RakNet -I&quot;PATH_TO_RAKNETSOURCE&quot; -I&quot;SwigInterfaceFiles&quot; -outdir SwigOutput/SwigCSharpOutput -o SwigOutput/CplusDLLIncludes/RakNet_wrap.cxx SwigInterfaceFiles/RakNet.i and hit enter</li>
<li>f you want to use SQLiteClientLoggerPlugin skip #6</li>
<li>PATH_TO_SQLITEPLUGIN is the path to the SQLite plugin directory. EX: /home/usr/RakNet/DependentExtensions/SQLite3Plugin</li>
<li>Type swig -c++ -csharp -namespace RakNet -I&quot;PATH_TO_RAKNETSOURCE&quot; -I&quot;SwigInterfaceFiles&quot; -I&quot;PATH_TO_SQLITEPLUGIN&quot; -DSWIG_ADDITIONAL_SQL_LITE -outdir SwigOutput/SwigCSharpOutput -o SwigOutput/CplusDLLIncludes/RakNet_wrap.cxx SwigInterfaceFiles/RakNet.i and hit enter</li>
</ol>
<span class="RakNetBlueHeader">Swig Options Explained<br /><br /> </span><span style="font-weight: bold;">-c++ </span><br /><br /> This must come first. This means that the source files are C++ not C.<br /><br /> <span style="font-weight: bold;">-csharp </span><br /> <span style="font-weight: bold;"><br /> </span>This is the target language. Currently the files are made for C# and may not work with the other options.<br /><br /> <span style="font-weight: bold;">-namespace RakNet</span><br /> <span style="font-weight: bold;"><br /> </span>This puts the generated files in the C# namespace RakNet<br /> <span style="font-weight: bold;"><br /> -I&quot;PATH_TO_RAKNETSOURCE&quot; </span><br /><br /> This option includes the directory for any source and includes if different from the interface file location.<br /><br /> <span style="font-weight: bold;">-I&quot;SwigInterfaceFiles&quot; </span><br /><br /> This option includes the directory for the interface files.<br /><br /> <span style="font-weight: bold;">-outdir SwigOutput/SwigCSharpOutput </span><br /><br /> This is where the output files to be included in the target language are placed.<br /><br /> <span style="font-weight: bold;">-o SwigOutput/CplusDLLIncludes/RakNet_wrap.cxx</span><br /> <span class="RakNetBlueHeader"><br /> </span>This is where the file that is included in the DLL project is placed.<br /><br /> <span style="font-weight: bold;">*The below two are used if SQLiteClientLoggerPlugin is used:</span><br /><br /> <span style="font-weight: bold;">-I&quot;PATH_TO_SQLITEPLUGIN&quot;<br /><br /> </span>Another include directory, the base directory where the SQLite plugin is.<br /><br /> <span style="font-weight: bold;">-DSWIG_ADDITIONAL_SQL_LITE<br /><br /> </span>A conditional compilation symbol, defined to include SQLiteClientLoggerPlugin.<span style="font-weight: bold;"><br /> </span><span class="RakNetBlueHeader"><br /> </span></td>
</tr>
</tbody>
</table>

|------------------------------------------------------|
| ![](spacer.gif)Creating the Swig Wrapped DLL Project |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Windows</span></p>
<span style="font-weight: bold;"><br /> </span>Note: If you do not plan on using SQLiteClientLoggerPlugin, DependentExtensions\Swig\DLL_Swig\RakNet.sln already contains a DLL project that makes the Swig files, builds the DLL, and copies it to the C# sample. You do not need to run these steps if you use that project. In that case, run the solution, rebuild the project, and goto &quot;Creating the C# project&quot;<span style="font-weight: bold;"><br /> </span>
<p>Creating the Swig  DLL</p>
<ol>
<li>Create a DLL project. I'll assume you know how to do this. In MSVC 7 you would create an empty project, then under Application Settings you check DLL and empty project.</li>
<li>Add the source files under the /Source directory to the project.</li>
<li>Add <span style="font-size: 12pt;">RakNet.cxx from Swigtools/CplusDLLIncludes</span></li>
<li>Add to the project these files under DependentExtensions: SQLite3Plugin\SQLite3ClientPlugin.h, SQLite3Plugin\SQLite3PLuginCommon.h, SQLite3Plugin\Logger\ClientOnly\SQLiteClientLoggerPlugin.h, SQLite3Plugin\Logger\SQLliteLoggerCommon.h, SQLite3Plugin\SQLite3ClientPlugin.cpp, SQLite3Plugin\SQLite3PLuginCommon.cpp, SQLite3Plugin\Logger\ClientOnly\SQLiteClientLoggerPlugin.cpp, SQLite3Plugin\Logger\SQLliteLoggerCommon.cpp</li>
<li>Add to &quot;Additional Include Directories&quot; the path to these folders in the DependentExtensions directory: SQLite3Plugin\Logger\ClientOnly, SQLite3Plugin\Logger, SQLite3Plugin</li>
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
<br />
<p><span class="RakNetBlueHeader">Linux</span></p>
<p>Creating the Swig  Dynamic Link</p>
<p><span class="RakNetManualTextBody">Note: If you ran the linux batch tool it will have made the dynamic link and attempted to install it, so you may skip these steps if it ran successfully.</span><br /></p>
<ol>
<li>In the next command PATH_TO_RAKNET_SWIG_FILES is the path to the swig directory. <span style="font-size: 12pt; font-family: &quot;Times New Roman&quot;;"> EX:: </span>../DependentExtensions/Swig<span style="font-size: 12pt; font-family: &quot;Times New Roman&quot;;"></span></li>
<li>g++ *.cpp PATH_TO_RAKNET_SWIG_FILES/SwigOutput/CplusDLLIncludes/RakNet_wrap.cxx -l pthread -I./ -shared -o RakNet</li>
<li>Note: In the previous command -l pthread is lower case L while -I./ is uppercase i.</li>
<li>If you wish to use SQLiteClientLoggerPlugin in the place of #2 use the below instructions</li>
<li> PATH_TO_SQLITEPLUGIN is the path to the SQLite plugin directory. EX: /home/usr/RakNet/DependentExtensions/SQLite3Plugin</li>
<li>g++ *.cpp PATH_TO_RAKNET_SWIG_FILES/SwigOutput/CplusDLLIncludes/RakNet_wrap.cxx PATH_TO_SQLITEPLUGIN\SQLite3ClientPlugin.cpp  PATH_TO_SQLITEPLUGIN \SQLite3PLuginCommon.cpp  PATH_TO_SQLITEPLUGIN \Logger\ClientOnly\SQLiteClientLoggerPlugin.cpp  PATH_TO_SQLITEPLUGIN \Logger\SQLliteLoggerCommon.cpp  -l pthread -I./ -IPATH_TO_SQLITEPLUGIN\Logger\ClientOnly -IPATH_TO_SQLITEPLUGIN\Logger -IPATH_TO_SQLITEPLUGIN -shared -o RakNet</li>
<li>A file called RakNet should be created that will be copied in the next section<br /></li>
</ol>
<br /></td>
</tr>
</tbody>
</table>

|-----------------------------------------|
| ![](spacer.gif)Creating the C\# project |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">Windows<br /><br /> </span><span class="RakNetBlueHeader">C# Project Setup<br /> </span>
<ol>
<li>Create a new C# project. In the included sample a console project is used.</li>
<li>If you wish to keep the project cleaner create a new folder called Swig. Right click the project and go to Add -&gt; New folder</li>
<li>Add the source files that were generated under the SwigOutput\SwigCSharpOutput directory to the project.The first step to doing this is right clicking the project, or if you added a folder right click the folder.</li>
<li>Go to Add -&gt; Existing Item. </li>
<li>Browse to the SwigOutput\SwigCSharpOutput directory.</li>
<li> If you want to link to the files rather than create a copy click on the arrow next to add and add a link. Otherwise click add.</li>
<li>Copy the DLL created in the previous step to the same folder as the binary. Example: bin\Debug</li>
<li>If you wish the project to be compatible with 64 bit vista you need to use the following steps.</li>
<li>When the project is opnen there should be a menu item called Build. Click it.</li>
<li>Go to Configuration manager and click it.</li>
<li>On the top right there is a drop down menu called Active Solution platform.</li>
<li>If X86 is in the project menu click it and go to step 16.</li>
<li>If not click New.</li>
<li>Under &quot;Type or select the new platform&quot; pick x86. Copy settings should be set to &quot;Any CPU&quot;</li>
<li>Make sure &quot;Create new project platforms&quot; is checked and click ok.</li>
<li>Click on &quot;Active Solutions configuration&quot; in the top left.</li>
<li>Pick the next item after the one currently selected if that item is not &lt;New&gt; or &lt;Edit&gt;.</li>
<li>Repeat 11-17 for that item.</li>
<li>There should be a list under &quot;Project Contexts&quot;.</li>
<li>Under the &quot;Platform&quot; column it should say x86 under each item, if not change it to x86.</li>
<li>Thre should be an column called &quot;Configuration&quot;, cycle through all the items except for &lt;New&gt; and &lt;Edit&gt; and make sure that #20 is true.</li>
<li>Do #20-21 for each item in the list.</li>
<li><strong>Make sure you have &quot;using RakNet;&quot; at the top of the files using RakNet. If this tutorial was followed the generated files are in the RakNet namespace.</strong></li>
</ol>
<table>
<tbody>
<tr class="odd">
<td align="left"><a href="CSharpBuildConfiguration.jpg"><img src="CSharpBuildConfiguration.jpg" /></a><br /></td>
</tr>
<tr class="even">
<td align="left"><span style="font-weight: bold;">Example of steps 9-21 to run on 64-bit systems. </span></td>
</tr>
</tbody>
</table>
<span class="RakNetBlueHeader"><br />C# Sample</span><br /> <span class="RakNetBlueHeader"></span> <br /> A C# sample project is included under DependentExtensions\Swig\SwigWindowsCSharpSample. The DLL file is not included and needs to be copied to the bin/Debug or the bin/Release folder depending on whether or not you are doing a release or debug build. If the DLL_Swig project is used the dll is copied automatically.<br /><br /> <span class="RakNetBlueHeader">Linux<br /><br /> </span><span class="RakNetBlueHeader">Installing Mono, if it is not Installed</span><br /> <span class="RakNetBlueHeader"><br /> </span><span class="RakNetBlueHeader"><span class="RakNetManualTextBody">Note: Mono greater than version 2.0 is needed for full compatability, earlier versions work with most of RakNet but cause crashes in some of it. You may be able to install Mono using your favorite package manager, if you can go that route do so. The instructions vary depending on your package manager and distribution. Some package manager have different options for the C# compiler. This tutorial assumes <span style="font-weight: bold;">gmcs</span> is installed.</span></span><br /><br />
<ol>
<li>Go to <a href="http://www.go-mono.com/mono-downloads/download.html" class="uri">http://www.go-mono.com/mono-downloads/download.html</a></li>
<li>Download the linux version</li>
<li>open a terminal</li>
<li>change to the directory you downloaded mono to..</li>
<li>type tar xzvf mono-VERSION.tar.gz hit enter</li>
<li>type cd mono-VERSION hit enter</li>
<li>type ./configure --prefix=/usr/local and  hit enter</li>
<li>type make and  hit enter</li>
<li>switch to your root user, or if user is a sudo user skip to next step</li>
<li>if root type make install and hit enter, other wise type sudo make install and hit enter if user is a sudo user.</li>
</ol>
<br /> <span class="RakNetBlueHeader">C# Project Setup<br /><br /> </span><span class="RakNetBlueHeader"><span class="RakNetManualTextBody">Note: If you used the included tools and just want to run the sample you may skip this section.</span></span><br />
<ol>
<li>Copy the dynamic link library to /usr/lib and rename it to RakNet with no extension if it is not already.<br /></li>
<li>Copy the files in Swigtools/SwigCSharpOutput into your project directory.</li>
<li>Create your main C# .cs file in that folder.</li>
<li><strong>Make sure you have &quot;using RakNet;&quot; at the top of the files using RakNet. If this tutorial was followed the generated files are in the RakNet namespace.</strong></li>
</ol>
<span class="RakNetBlueHeader">Building the Project</span>
<ol>
<li>Type gmcs *.cs -out:ExecutableName.exe where ExecutableName is the name you want and hit enter.</li>
<li>The file can be ran with mono ExecutableName.exe<br /><br /></li>
</ol>
<span class="RakNetBlueHeader">C# Sample</span><br /> <span class="RakNetBlueHeader"></span> <br /> A C# sample is included under DependentExtensions/Swig/SwigLinuxCSharpSample. The DLL file is not included and needs to be copied to /usr/lib as per the earlier instructions. If the tools are used the cs files are automatically copied to the sample directory and there is an attempt by tools to copy the dll to /usr/lib.
<ol>
</ol></td>
</tr>
</tbody>
</table>

|----------------------------|
| ![](spacer.gif)Limitations |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">Limitations<br /> </span>
<ol>
<li>BitStream may have problems if larger than 9 megabytes.</li>
</ol>
<br />
<ol>
</ol></td>
</tr>
</tbody>
</table>

|-----------------------|
| Important Information |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">Important Information<br /> </span>
<ol>
<li>RakPeer,RakString,FullyConnectedMesh2,UDPForwarder, and UDPProxyCoordinator need to be released before the program ends.Any item that go out of scope before program end just needs to be garbage collected, so items not in main or global generally do not have issues. For these just call the GC before program end.For globals and items in main the items can be released with the Dispose method if new was used, or DestroyInstance if CreateInstance was used.<br /></li>
</ol>
<br />
<ol>
</ol></td>
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
<td align="left"><p><a href="index.html">Index</a><br /></p></td>
</tr>
</tbody>
</table>
