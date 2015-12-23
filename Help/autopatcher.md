# Autopatcher

## The autopatcher system

The autopatcher is a class that manages the copying of missing or changed files between two or more systems. It handles transferring files, compressing transferred files, security, and file operations. It does not handle basic connectivity or provide a user interface. For basic connectivity, make sure you connect RakPeerInterface or PacketizedTCP before using it. For a UI, that is entirely up to you. Autopatchers are used in all online only games and most AAA commercial games.

On the client, you need an instance of AutopatcherClient. The server needs an instance of AutopatcherServer. The code for those classes is not part of RakNet core. They are located at .\DependentExtensions\ , and you need to add them to your project.

**On the client side**

On the client side, most work is done with just a couple of methods of AutopatcherClient.

<span class="RakNetCode">void SetFileListTransferPlugin(FileListTransfer *flt);</span>

This plugin has a dependency on the FileListTransfer plugin, which it uses to actually send the files. So you need an instance of that plugin registered with RakPeerInterface, and a pointer to that interface should be passed in this function.

<span class="RakNetCode">bool PatchApplication(const char *_applicationName, const char *_applicationDirectory, const char *lastUpdateDate, SystemAddress host, FileListTransferCBInterface *onFileCallback, const char *restartOutputFilename, const char *pathToRestartExe);</span>

Patches a certain directory associated with a named application to match the same named application on the patch server
__applicationName_ - The name of the application
__applicationDirectory_ - The directory to write the output to.
_lastUpdateDate_ - A string representing the last time you updated from the patch server, so you only check newer files. Should be 0 the first time, or if you want to do a full scan. Returned in GetServerDate() after you call PatchApplication successfully.
_host_ - The address of the remote system to send the message to.
_onFileCallback_ - Callback to call per-file (optional). When fileIndex+1==setCount in the callback then the download is done.
__restartOutputFilename_ - If it is necessary to restart this application, where to write the restart data to. You can include a path in this filename.
_pathToRestartExe_ - What exe to launch from the AutopatcherClientRestarter . argv[0] will work to relaunch this application.

There are other functions and classes involved in the client side, but you should study the sample at _\Samples\AutopatcherClient_.

**On the server side**

On the client side, you use an instance of AutopatcherServer.

<span class="RakNetCode">void SetFileListTransferPlugin(FileListTransfer *flt);</span>
Like AutopatcherClient, this plugin also has a dependency on the FileListTransfer plugin, which it uses to actually send the files. So you need an instance of that plugin registered with RakPeerInterface, and a pointer to that interface should be passed here.

<span class="RakNetCode">void SetAutopatcherRepositoryInterface(AutopatcherRepositoryInterface *ari);</span>
With the function, you tell AutopatchServer how to take care of the network transfer of the patchs. This class only does the network transfers for the autopatcher. All the data is stored in a repository. Pass the interface to your repository with this function. RakNet comes with AutopatcherPostgreRepository if you wish to use that.

Check the sample at _\Samples\AutoPatcherServer_. It uses an implementation of AutopatcherRepositoryInterface for PostgreSQL (AutopatcherPostgreRepository), to store all the files of an application in a PostgreSQL database.

**Directory structure**

It's possible to use directory structures. For example, suppose you have the following directory structure for your application:

Readme.txt
Music/Song1.wav
Music/Song2.wav

The Autopatcher will keep directories structures intact. So if the sending system has that directory structure, the downloading system will mirror this directory structure.

**Server required files (using [PostgreSQL](http://www.postgresql.org/)):**

*   All source files in DependentExtensions\bzip2-1.0.3
*   DependentExtensions\CreatePatch.h and .cpp
*   DependentExtensions\MemoryCompressor.h and .cpp
*   DependentExtensions\AutopatcherServer.h and .cpp
*   All source files in DependentExtensions\AutopatcherPostgreRepository
*   All source files in DependentExtensions\PostgreSQLInterface
*   All source files in Samples\AutopatcherServer, should you want to use the default console application to run the server.

**Server Dependencies (using [PostgreSQL](http://www.postgresql.org/)):**

PostgreSQL 8.2 or newer, installed at C:\Program Files\PostgreSQL\8.2\. Change the project property paths should your installation directory be different. Do not forget to check development tools in the PostgreSQL installer or the headers and libs will not be installed.

**Client required files**

*   All source files in DependentExtensions\bzip2-1.0.3
*   DependentExtensions\MemoryCompressor.h and .cpp
*   DependentExtensions\ApplyPatch.h and .cpp
*   DependentExtensions\AutopatcherClient.h and .cpp
*   All source files in Samples\AutopatcherClient, should you want to use the default console application as a design template.
*   All source files in Samples\AutopatcherClientRestarter, should you want to use the default console application to restart the autopatcher after it patches itself.

**Using TCP instead of UDP (recommended)**

RakPeerInterfaces uses UDP, which is a problem if RakNet's protocol changes - the autopatcher wouldn't be able to connect to the new protocol. It is recommended you use TCP instead of UDP. In the sample AutopatcherClientTest.cpp and AutopatcherServerTest_MySQL.cpp or AutopatcherServerTest.cpp you can enable this with the define USE_TCP found at the top of those files.

Changes:

1.  Use the PacketizedTCP class instead of RakPeerInterface
2.  PacketizedTCP returns connection state changes with HasCompletedConnectionAttempt(), HasNewIncomingConnection(), and HasLostConnection(), rather than byte identifiers.
3.  Attach the plugin to your instance of PacketizedTCP instead of RakPeerInterface

**Optimizing for large games:**

AutopatcherClient::PatchApplication has a parameter lastUpdateDate. If 0 is passed for this parameter, the server does not know what version the client is using. This generates a full-scan, where the server has to access the database for the hash of every file for the application, and is very slow, so should be avoided whenever possible.

When distributing your application, you should get the timestamp on the patching server using the time() function and store this with the distribution. You can then pass that value to AutopatcherClient::PatchApplication. As long as this value is greater than or equal to the time when AutopatcherRepositoryInterface::UpdateApplicationFiles was called, the server can use it to do an optimized lookup for which files changed from the version the client is using.

AutopatcherServer has an additional optimization through the function CacheMostRecentPatch(). This will store the most recent patch in memory if using AutopatcherPostgreRepository. If the time passed to PatchApplication() is greater than the most recent patch, the patch is skipped entirely. If it is less than the most recent patch, but still greater than the patch prior to that, the patch is served from memory instead of disk. For this to work, every time you get ID_AUTOPATCHER_FINISHED call AutopatcherClient::GetServerDate(), save it to disk, and use that date for the next call to PatchApplication().

Summary of performance tips:

1.  The autopatcher was not designed to serve the entire application, only to patch. Your users should download from FTP or a webserver as much as possible before patching.
2.  Use AutopatcherServer::CacheMostRecentPatch() if using AutopatcherPostgreRepository
3.  AutopatcherPostgreRepository is about 10x faster than AutopatcherMySQLRepository even without CacheMostRecentPatch()
4.  If 0 is passed to lastUpdateDate for AutopatcherClient::PatchApplication, this generates a full-scan of every file accessing the database for each file. Do not do it except on user request to repair corrupted installations.

5.  To distribute your application, call time() on the server immediately after UpdateApplicationFiles(). Store that value with the client distribution, and use as the default to lastUpdateParameter for the first call to AutopatcherClient::PatchApplication.

6.  Whenever you get ID_AUTOPATCHER_FINISHED, save the value returned by AutopatcherClient::GetServerDate() to use for the lastUpdateDate parameter of next call to AutopatcherClient::PatchApplication

7.  With CacheMostRecentPatch(), only the most recent patch is stored in memory, so do not call UpdateApplicationFiles pointlessly.

**Notes on memory usage:**

Patches are created with code from Colin Percival [http://www.daemonology.net/bsdiff/](http://www.daemonology.net/bsdiff/) . The patching algorithm uses [Larsson and Sadakane's qsufsort](http://www.cs.lth.se/Research/Algorithms/Papers/jesper5.ps) and is quite memory intensive, and not recommended for files over several hundred megabytes. If you use CacheMostRecentPatch() your most recent patch is also kept in memory. Additionally, file transfer takes about 8 megabytes of memory per user.
