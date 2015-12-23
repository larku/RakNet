<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|---------------------------|
| FileListTransfer Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Send and receive file(s) more easily</span><br /></p>
<p>The FileListTransfer plugin is designed to send a list of files that have been read into the <strong>FileList</strong> class. It is similar to the <a href="directorydeltatransfer.html">DirectoryDeltaTransfer</a> plugin, except that it doesn't send deltas based on pre-existing files or actually write the files to disk. It solely handles the networking part of sending files.</p>
<p>Usage:</p>
<ol>
<li>Server: Call SetupReceive(...) with the address of a system to allow a file send, and a <strong>FileListTransferCBInterface</strong> derived callback handler to be called as the data arrives.</li>
<li>Client: Encode the files you want to send into the <strong>FileList</strong> class.</li>
<li>Client: Call Send(...) with your <strong>FileList</strong> class instance, a user-defined ID to identify this set of files, parameters to pass to RakPeerInterface::Send() or TCPInterface::Send(), and a boolean indicating if the files should be compressed or not. Compression is not very fast, so it's best to leave that off unless bandwidth is at a premium.</li>
</ol></td>
</tr>
</tbody>
</table>

|-------------------|
| FileList Overview |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>The FileList class stores a list of files and data, and also contains utility functions for dealing with the harddrive in general. It was originally written for the <a href="autopatcher.html">Autopatcher</a>, but can be used for your own purposes as well.</p>
<p>See FileList.h for a complete description of all functions and parameters.</p>
<p>/// Add all the files at a given directory.<br /> void AddFilesFromDirectory(const char *applicationDirectory, const char *subDirectory, bool writeHash, bool writeData, bool recursive, unsigned char context);</p>
<p>/// Deallocate all memory<br /> void Clear(void);</p>
<p>/// Write all encoded data into a bitstream<br /> void Serialize(RakNet::BitStream *outBitStream);</p>
<p>/// Read all encoded data from a bitstream. Clear() is called before deserializing.<br /> bool Deserialize(RakNet::BitStream *inBitStream);</p>
<p>/// Given the existing set of files, search applicationDirectory for the same files.<br /> /// For each file that is missing or different, add that file to missingOrChangedFiles. Note: the file contents are not written, and only the hash if written if alwaysWriteHash is true<br /> void ListMissingOrChangedFiles(const char *applicationDirectory, FileList *missingOrChangedFiles, bool alwaysWriteHash, bool neverWriteHash);</p>
<p>/// Return the files that need to be written to make \a input match this current FileList.<br /> void GetDeltaToCurrent(FileList *input, FileList *output, const char *dirSubset, const char *remoteSubdir);</p>
<p>/// Assuming FileList contains a list of filenames presumably without data, read the data for these filenames<br /> void PopulateDataFromDisk(const char *applicationDirectory, bool writeFileData, bool writeFileHash, bool removeUnknownFiles);</p>
<p>/// Write all files to disk, prefixing the paths with applicationDirectory<br /> void WriteDataToDisk(const char *applicationDirectory);</p>
<p>/// Add a file, given data already in memory<br /> void AddFile(const char *filename, const char *data, const unsigned dataLength, const unsigned fileLength, unsigned char context);</p>
<p>/// Add a file, reading it from disk<br /> void AddFile(const char *filepath, const char *filename, unsigned char context);</p>
<p>/// Delete all files stored in the file list<br /> void DeleteFiles(const char *applicationDirectory);</p></td>
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
<td align="left"><p><a href="index.html">Index</a><br /> <a href="directorydeltatransfer.html">Delta Directory Transfer</a><br /></p></td>
</tr>
</tbody>
</table>
