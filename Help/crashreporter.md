<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|--------------------------|
|  Crash Reporter Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Minidumps made easy</span><br /></p>
<p>The CrashReporter, found at RakNet/Samples/CrashReporter, is a Windows-only class designed to make it easier to debug unmonitored servers and/or game clients. When a crash occurs, the CrashReporter implementation catches the exception, writes a minidump, and then writes to disk or sends an email. The email operation can be interactive, opening the user's email client, or non-interactive, conecting to the mail server with the <a href="emailsender.html">EmailSender</a> class and sending a report automatically.</p>
<p><strong>Copied from CrashReporter.h</strong></p>
<p>The minidump can be opened in visual studio and will show you where the crash occurred and give you the local variable values.<br /><br /> <em>How to use the minidump:</em><br /><br /> Put the minidump on your harddrive and double click it. It will open Visual Studio. It will look for the exe that caused the crash in the directory that the program that crashed was running at. If it can't find this exe, or if it is different, it will look in the current directory for that exe. If it still can't find it, or if it is different, it will load Visual Studio and indicate that it can't find the executable module. No source code will be shown at that point. However, you can specify modpath=&lt;pathToExeDirectory&gt; in the project properties window for &quot;Command Arguments&quot;. The best solution is copy the .dmp to a directory containing a copy of the exe that crashed.<br /><br /> On load, Visual Studio will look for the .pdb, which it uses to find the source code files and other information. This is fine as long as the source code files on your harddrive match those that were used to create the exe. If they don't, you will see source code but it will be the wrong code. There are three ways to deal with this.<br /><br /> The first way is to change the path to your source code so it won't find the wrong code automatically. This will cause the debugger to not find the source code pointed to in the .pdb . You will be prompted for the location of the correct source code.<br /><br /> The second way is to build the exe on a different path than what you normally program with. For example, when you program you use c:/Working/Mygame When you release builds, you do at c:/Version2.2/Mygame . After a build, you keep the source files, the exe, and the pdb on a harddrive at that location. When you get a crash .dmp, copy it to the same directory as the exe, ( c:/Version2.2/Mygame/bin ) This way the .pdb will point to the correct sources to begin wtih.<br /><br /> The third way is save build labels or branches in source control and get that version (you only need source code + .exe + .pdb) before debugging. After debugging, restore your previous work.<br /></p>
<p>To use:<br /> #include &quot;DbgHelp.h&quot;<br /> Link with Dbghelp.lib ws2_32.lib</p></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|---------------------|
| [Index](index.html) |
