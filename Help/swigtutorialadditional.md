![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)
|-------------------------|
| ![](spacer.gif)Overview |

|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="RakNetBlueHeader"></span>This file contains additional instructions for using Swig with the optional dependent extension options.                                                                                                                                                                                                                    
  The base instructions are available in the [Swig Tutorial](swigtutorial.html)                                                                                                                                                                                                                                                                                     
  <span class="RakNetBlueHeader"></span><span class="RakNetBlueHeader"></span><span class="RakNetBlueHeader"></span><span style="font-weight: bold;"></span><span style="font-weight: bold;"></span><span style="font-weight: bold;"></span><span style="font-weight: bold;"></span><span style="font-weight: bold;"></span><span class="RakNetBlueHeader"></span>  |

|------------------------------------------|
| ![](spacer.gif)Autopatcher MySql Version |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">Windows<br /><br /> <span class="RakNetManualTextBody">The below instructions detail the extra configuration needed on the Visual Studio Project.</span><br /><br /> Extra Project Options<br /><br /> </span><span class="RakNetManualTextBody">Under General-&gt;Common Language Runtime support the option needs to be set to No Common Language Runtime support</span><span class="RakNetBlueHeader"><br /><br /> Additional Include Directories<br /><br /> </span><span class="RakNetManualTextBody">Under Dependent Extensions the following additional directories need to be in the include configuration.<br /><br /> -Autopatcher/AutopatcherMySQLRepository<br /> -Autopatcher<br /> -bzip2-1.0.3<br /> -MySQLInterface<br /><br /> MySql header directory needs to be included.<br /><br /> For 5.1 on Vista the directory looks like: C:\Program Files (x86)\MySQL\MySQL Server 5.1\include</span><span class="RakNetBlueHeader"><br /><br /> Additional sources<br /><br /> </span><span class="RakNetManualTextBody">Under Dependent Extensions the following additional source files need to be included in the project.<br /><br /> </span><span class="RakNetManualTextBody">-bzip2-1.0.3/blocksort.c<br /> -</span><span class="RakNetManualTextBody">bzip2-1.0.3/</span><span class="RakNetManualTextBody">bzip2.c<br /> -</span><span class="RakNetManualTextBody">bzip2-1.0.3/</span><span class="RakNetManualTextBody">bzlib.c<br /> -</span><span class="RakNetManualTextBody">bzip2-1.0.3/</span><span class="RakNetManualTextBody">compress.c<br /> -</span><span class="RakNetManualTextBody">bzip2-1.0.3/</span><span class="RakNetManualTextBody">crctable.c<br /> -</span><span class="RakNetManualTextBody">bzip2-1.0.3/</span><span class="RakNetManualTextBody">decompress.c<br /> -</span><span class="RakNetManualTextBody">bzip2-1.0.3/</span><span class="RakNetManualTextBody">dlltest.c<br /> -</span><span class="RakNetManualTextBody">bzip2-1.0.3/</span><span class="RakNetManualTextBody">huffman.c<br /> -</span><span class="RakNetManualTextBody">bzip2-1.0.3/</span><span class="RakNetManualTextBody">randtable.c<br /><br /> -Autopatcher/ApplyPatch.cpp<br /> -</span><span class="RakNetManualTextBody">Autopatcher/</span><span class="RakNetManualTextBody">AutopatcherClient.cpp<br /> -</span><span class="RakNetManualTextBody">Autopatcher/</span><span class="RakNetManualTextBody">AutopatcherMySQLRepository/</span><span class="RakNetManualTextBody">AutopatcherMySQLRepository.cpp<br /> -</span><span class="RakNetManualTextBody">Autopatcher/</span><span class="RakNetManualTextBody">AutopatcherServer.cpp<br /> -</span><span class="RakNetManualTextBody">Autopatcher/</span><span class="RakNetManualTextBody">CreatePatch.cpp<br /> -</span><span class="RakNetManualTextBody">Autopatcher/</span><span class="RakNetManualTextBody">MemoryCompressor.cpp<br /> -</span><span class="RakNetManualTextBody">MySQLInterface/</span><span class="RakNetManualTextBody">MySQLInterface.cpp<br /><br /> Additional Libraries<br /><br /> </span><span class="RakNetManualTextBody">The MySql library needs to be included.</span><br /> <span class="RakNetManualTextBody"><br /> </span><span class="RakNetManualTextBody">For 5.1 on Vista the location looks like:<br /><br /> </span><span class="RakNetManualTextBody">For debug:<br /> C:\Program Files (x86)\MySQL\MySQL Server 5.1\lib\debug\libmysql.lib<br /><br /> </span><span class="RakNetManualTextBody">For release:<br /> C:\Program Files (x86)\MySQL\MySQL Server 5.1\lib\opt\libmysql.lib</span><br />
<ol>
</ol>
<span class="RakNetBlueHeader">Replacement Swig File Generation Tool Steps<br /><br /> <span class="RakNetManualTextBody">These are replacement steps. If the dll project is used the PreBuild.bat needs to be modified, the last steps are instructions on how to do that.</span><br /> </span>
<ol>
<li>Click the start menu and click on run. In Vista click start-&gt;search type &quot;run&quot; hit enter.</li>
<li>Type cmd and hit enter.</li>
<li>In the next command PATH_TO_RAKNET_SWIG_FILES is the path to the swig directory. For example C:\RakNet\DependentExtensions\Swig</li>
<li>Type cd PATH_TO_RAKNET_SWIG_FILES hit enter</li>
<li>In the next command PATH_TO_RAKNETSOURCE is the path to the swig source directory. For example: C:\RakNet\Source. PATH_TO_SWIG is an unquoted path with a trailing slash to the Swig directory Example: C:\Swig\. If you added swig to your path variable then PATH_TO_SWIG is not needed and can be ignored or set to &quot;&quot;. PATH_TO_DEPENDENTEXTENSIONS is the path to the Dependent Extensions directory Example: C:\RakNet\DependentExtensions. OPTION1 in this case will be MYSQL_AUTOPATCHER.</li>
<li>Type MakeSwigWithExtras.bat PATH_TO_RAKNETSOURCE PATH_TO_SWIG PATH_TO_DEPENDENTEXTENSIONS OPTION1 hit enter<br /></li>
<li>If you want to use SQLiteClientLoggerPlugin skip #6, the next steps will be used instead.</li>
<li>If you have added swig to your path variable, just use &quot;&quot; for PATH_TO_SWIG</li>
<li>PATH_TO_SQLITEPLUGIN is the path to the SQLite plugin directory. EX: C:\RakNet\DependentExtensions\SQLite3Plugin</li>
<li>Type MakeSwig.bat PATH_TO_RAKNETSOURCE PATH_TO_SWIG  PATH_TO_SQLITEPLUGIN hit enter</li>
<li>If you are not using the DLL_Swig project goto &quot;Creating the Swig Wrapped DLL Project&quot; in the <a href="swigtutorial.html">Swig Tutorial</a></li>
<li>If you are using the DLL_Swig project, in the project directory there is a file called Prebuild.bat, open it for editing.</li>
<li>Replace the line &quot;MakeSwig.bat &quot;../../Source&quot;&quot; with the command created in these steps.</li>
<li>Now goto &quot;Creating the Swig Wrapped DLL Project&quot; in the <a href="swigtutorial.html">Swig Tutorial</a>. Make sure the extra configuration in this help file is followed.</li>
</ol>
<span class="RakNetBlueHeader"></span><span class="RakNetBlueHeader"><br /> Replacement Swig File Generation Manual Steps<br /> </span>
<ol>
<li>Click the start menu and click on run. In Vista click start-&gt;search type &quot;run&quot; hit enter.</li>
<li>Type cmd and hit enter.</li>
<li>In the next command PATH_TO_RAKNET_SWIG_FILES is the path to the swig directory. For example C:\RakNet\DependentExtensions\Swig</li>
<li>Type cd PATH_TO_RAKNET_SWIG_FILES hit enter</li>
<li>In the next command PATH_TO_RAKNETSOURCE is the path to the swig source directory. For example: C:\RakNet\Source. PATH_TO_DEPENDENTEXTENSIONS is the path to the Dependent Extensions directory Example: C:\RakNet\DependentExtensions.</li>
<li>Type C:\Swig\swig -c++ -csharp -namespace RakNet -I&quot;PATH_TO_RAKNETSOURCE&quot; -I&quot;SwigInterfaceFiles&quot; -I&quot;PATH_TO_DEPENDENTEXTENSIONS&quot; -DSWIG_ADDITIONAL_AUTOPATCHER_MYSQL -outdir SwigOutput\SwigCSharpOutput -o SwigOutput\CplusDLLIncludes\RakNet_wrap.cxx SwigInterfaceFiles\RakNet.i hit enter</li>
<li>Goto &quot;Creating the Swig DLL&quot; in the <a href="swigtutorial.html">Swig Tutorial</a>. Make sure the extra configuration in this help file is followed.<a href="swigtutorial.html"></a></li>
</ol>
<span class="RakNetBlueHeader"></span><span class="RakNetBlueHeader">Linux</span><br /><br /><span class="RakNetBlueHeader">Replacement Swig File Tool Steps<br /><br /></span><span class="RakNetBlueHeader"> <span class="RakNetManualTextBody">Note: The Linux batch <span class="RakNetBlueHeader"></span><span class="RakNetBlueHeader"></span>requires Wget,Tar,Make and GCC  to be installed, unless swig is already installed. Most of the time </span></span><span class="RakNetBlueHeader"><span class="RakNetManualTextBody">Wget,Tar,Make and GCC are</span></span><span class="RakNetBlueHeader"><span class="RakNetManualTextBody"> already installed.</span><br /> </span>
<ol>
<li>Open a terminal if you are not already at one.<br /></li>
<li>In the next command PATH_TO_RAKNET_SWIG_FILES is the path to the swig directory. For example /home/usr/RakNet/DependentExtensions/Swig</li>
<li>Type cd PATH_TO_RAKNET_SWIG_FILES hit enter</li>
<li>Type chmod u+x MakeSwig.sh<br /></li>
<li>In the next command PATH_TO_RAKNETSOURCE is the path to the swig source directory. For example: /home/usr/RakNet/Source. PATH_TO_DEPENDENTEXTENSIONS is the path to the Dependent Extensions directory Example: /home/usr/RakNet/DependentExtensions. OPTION1 in this case will be MYSQL_AUTOPATCHER.</li>
<li>Type MakeSwigWithExtras.sh PATH_TO_RAKNETSOURCE  PATH_TO_DEPENDENTEXTENSIONS OPTION1 hit enter</li>
<li>If you want to use SQLiteClientLoggerPlugin skip #6, the next steps will be used instead.<br /></li>
<li>PATH_TO_SQLITEPLUGIN is the path to the SQLite plugin directory. EX: /home/usr/RakNet/DependentExtensions/SQLite3Plugin<br /></li>
<li>Type MakeSwig.sh PATH_TO_RAKNETSOURCE PATH_TO_SQLITEPLUGIN hit enter<br /></li>
<li>Skip to &quot;Creating the C# project&quot; in the <a href="swigtutorial.html">Swig Tutorial</a>.   </li>
</ol>
<br /><span class="RakNetBlueHeader">Replacement Manual Swig File Generation Steps</span><br /><br />
<ol>
<li>Open a terminal<br /></li>
<li>In the next command PATH_TO_RAKNET_SWIG_FILES is the path to the swig directory. For example /home/usr/RakNet/DependentExtensions/Swig</li>
<li>Type cd PATH_TO_RAKNET_SWIG_FILES hit enter</li>
<li>In the next command PATH_TO_RAKNETSOURCE is the path to the swig source directory. For example: /home/usr/RakNet/Source</li>
<li>Type swig -c++ -csharp -namespace RakNet -I&quot;PATH_TO_RAKNETSOURCE&quot; -I&quot;SwigInterfaceFiles&quot; -outdir SwigOutput/SwigCSharpOutput -o SwigOutput/CplusDLLIncludes/RakNet_wrap.cxx SwigInterfaceFiles/RakNet.i and hit enter</li>
<li>Goto the next section below.</li>
</ol>
<p>Replacement Creating the Swig  Dynamic Link Steps</p>
<p><span class="RakNetManualTextBody">Note: If you ran the linux batch tool it will have made the dynamic link and attempted to install it, so you may skip these steps if it ran successfully.</span><br /></p>
<ol>
<li>In the next command PATH_TO_RAKNET_SWIG_FILES is the path to the swig directory. <span style="font-size: 12pt; font-family: &quot;Times New Roman&quot;;"> EX:: </span>../DependentExtensions/Swig<span style="font-size: 12pt; font-family: &quot;Times New Roman&quot;;"></span></li>
<li>In the next command PATH_TO_DEPENDENTEXTENSIONS is the path to the Dependent Extensions directory. EX: /home/usr/RakNet/DependentExtensions</li>
<li>First we need to compile the C files seperatly with GCC in C mode type the following command and hit enter: gcc -c PATH_TO_DEPENDENTEXTENSIONS/bzip2-1.0.3/blocksort.c PATH_TO_DEPENDENTEXTENSIONS/bzip2-1.0.3/bzip2.c PATH_TO_DEPENDENTEXTENSIONS/bzip2-1.0.3/bzlib.c PATH_TO_DEPENDENTEXTENSIONS/bzip2-1.0.3/compress.c PATH_TO_DEPENDENTEXTENSIONS/bzip2-1.0.3/crctable.c PATH_TO_DEPENDENTEXTENSIONS/bzip2-1.0.3/decompress.c PATH_TO_DEPENDENTEXTENSIONS/bzip2-1.0.3/dlltest.c PATH_TO_DEPENDENTEXTENSIONS/bzip2-1.0.3/huffman.c $2/bzip2-1.0.3/randtable.c<br /></li>
<li>Now we use those object files and compile the C++ files with C++ mode type the following command and hit enter: g++ *.cpp PATH_TO_RAKNET_SWIG_FILE/SwigOutput/CplusDLLIncludes/RakNet_wrap.cxx blocksort.o bzip2.o bzlib.o compress.o crctable.o decompress.o dlltest.o huffman.o randtable.o PATH_TO_DEPENDENTEXTENSIONS/Autopatcher/ApplyPatch.cpp PATH_TO_DEPENDENTEXTENSIONS/Autopatcher/AutopatcherClient.cpp PATH_TO_DEPENDENTEXTENSIONS/Autopatcher/AutopatcherMySQLRepository/AutopatcherMySQLRepository.cpp PATH_TO_DEPENDENTEXTENSIONS/Autopatcher/AutopatcherServer.cpp PATH_TO_DEPENDENTEXTENSIONS/Autopatcher/CreatePatch.cpp PATH_TO_DEPENDENTEXTENSIONS/Autopatcher/MemoryCompressor.cpp PATH_TO_DEPENDENTEXTENSIONS/MySQLInterface/MySQLInterface.cpp  -l pthread -lmysqlclient -I/usr/include/mysql/ -I./ -IPATH_TO_DEPENDENTEXTENSIONS/Autopatcher/AutopatcherMySQLRepository -IPATH_TO_DEPENDENTEXTENSIONS/Autopatcher -IPATH_TO_DEPENDENTEXTENSIONS/bzip2-1.0.3 -IPATH_TO_DEPENDENTEXTENSIONS/MySQLInterface -shared -o RakNet           </li>
<li>Note: In the previous command -l pthread is lower case L while -I./ is uppercase i.</li>
<li>If you wish to use SQLiteClientLoggerPlugin in the place of #2 use the below instructions</li>
<li> PATH_TO_SQLITEPLUGIN is the path to the SQLite plugin directory. EX: /home/usr/RakNet/DependentExtensions/SQLite3Plugin</li>
<li>g++ *.cpp PATH_TO_RAKNET_SWIG_FILES/SwigOutput/CplusDLLIncludes/RakNet_wrap.cxx PATH_TO_SQLITEPLUGIN\SQLite3ClientPlugin.cpp  PATH_TO_SQLITEPLUGIN \SQLite3PLuginCommon.cpp  PATH_TO_SQLITEPLUGIN \Logger\ClientOnly\SQLiteClientLoggerPlugin.cpp  PATH_TO_SQLITEPLUGIN \Logger\SQLliteLoggerCommon.cpp  -l pthread -I./ -IPATH_TO_SQLITEPLUGIN\Logger\ClientOnly -IPATH_TO_SQLITEPLUGIN\Logger -IPATH_TO_SQLITEPLUGIN -shared -o RakNet</li>
<li>A file called RakNet should be created that will be copied in the next section</li>
<li>Go to &quot;Creating the C# project&quot; in the <a href="swigtutorial.html">Swig Tutorial</a>.<a href="swigtutorial.html"></a></li>
</ol>
<br /><span class="RakNetBlueHeader"><br /> </span></td>
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
<td align="left"><p><a href="index.html">Swig Tutorial<br /> Index</a></p></td>
</tr>
</tbody>
</table>
