![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)
|----------------------------------------------|
| ![](spacer.gif)Windows bluetooth integration |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Can be supported in C using sockets and native libraries</span><br /><br /> Bluetooth support is relatively easy to add.</p>
<ol>
<li>Include &quot;Ws2bth.h&quot;</li>
<li>Modify socket calls in SocketLayer.cpp, refer to <a href="http://msdn.microsoft.com/en-us/library/aa362928%28v=vs.85%29.aspx">http://msdn.microsoft.com/en-us/library/aa362928%28v=vs.85%29.aspx</a></li>
<li><a href="http://www.winsocketdotnetworkprogramming.com/winsock2programming/winsock2advancedotherprotocol4j.html">Source example</a></li>
<li>There is also an <a href="http://www.broadcom.com/support/bluetooth/sdk.php">API from Broadcom</a> for Windows although I'm not sure what the difference is between that and the native Windows system calls.</li>
</ol></td>
</tr>
</tbody>
</table>

|--------------------------------------------|
| ![](spacer.gif)Linux bluetooth integration |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Can be supported in C using the BlueZ library</span><br /><br /> Linux uses the <a href="http://www.bluez.org">BlueZ</a> library to interface with Bluetooth devices. There is a great resource on BlueZ here: <a href="http://people.csail.mit.edu/albert/bluez-intro/" class="uri">http://people.csail.mit.edu/albert/bluez-intro/</a>.</p></td>
</tr>
</tbody>
</table>

|------------------------------------------|
| ![](spacer.gif)Mac bluetooth integration |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Indirectly send through IOBluetoothL2CAPChannelRef?</span><br /><br /> <a href="http://developer.apple.com/library/mac/documentation/DeviceDrivers/Conceptual/Bluetooth/Bluetooth.pdf">Mac Bluetooth support</a> is supported through the IOBluetooth library, written in Objective-C. C equivalents are available by suffixing Ref to the name, for example IOBluetoothObjectRef contains the interface in C. You are expected to create instances of <a href="http://developer.apple.com/library/mac/#documentation/DeviceDrivers/Reference/IOBluetooth/IOBluetoothL2CAPChannel_h/Classes/IOBluetoothL2CAPChannel/">IOBluetoothL2CAPChannel</a> which represent a communication channel. L2CAP is an unreliable communications channel. The equivalent reliable communications channel uses <a href="http://developer.apple.com/library/mac/#documentation/DeviceDrivers/Reference/IOBluetooth/IOBluetoothRFCOMMChannel_h/Classes/IOBluetoothRFCOMMChannel/index.html">RFCOMM</a> <a href="http://developer.apple.com/library/mac/#documentation/DeviceDrivers/Reference/IOBluetooth/">The full framework of methods</a> It doesn't appear to be possible to get direct socket access to Bluetooth on the Mac. However, it may be possible to use RakNet and IOBluetoothL2CAPChannel together by using RakNet's SocketLayer::SetSocketLayerOverride(), and thereby changing RakNet's sendto and recvfrom calls to use IOBluetoothL2CAPChannel instead.</p></td>
</tr>
</tbody>
</table>

|---------------------------------------------|
| ![](spacer.gif)iPhone bluetooth integration |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Bluetooth exposed through Gamekit</span><br /><br /> The only interface for Bluetooth communications is through the higher level framework <a href="http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html">GameKit</a>. Gamekit uses Objective-C.</p>
Similar to the Mac, it may be possible to indirectly send through RakNet using SocketLayer::SetSocketLayerOverride() through the <a href="http://developer.apple.com/library/ios/#documentation/GameKit/Reference/GKSession_Class/Reference/Reference.html#//apple_ref/occ/instm/GKSession/sendData:toPeers:withDataMode:error:">sendData:toPeers</a> method exposed by GKSession, and sending that data unreliabily.</td>
</tr>
</tbody>
</table>

|----------------------------------------------|
| ![](spacer.gif)Android bluetooth integration |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">BlueZ used for underlying support, however not accessible to implementation</span><br /></p>
<ul>
<li>Android uses BlueZ (w/ l2cap, etc.) as the underlying Bluetooth API (at the Linux OS level, underneath the Android Dalvik VM). All of the Android SDK (Java) Bluetooth classes end up using this API. The Android NDK cross-compiler provides access to many of the Android system's underlying libraries. However, Bluetooth does NOT appear to be exposed by the NDK. I.e.: The NDK's headers do not include Bluetooth-specific headers, tokens, etc. (I performed a find-in-files on the NDK headers and other files for various Bluetooth related/specific tokens like: BTPROTO_RFCOMM, l2cap, etc.)</li>
<li>For a list of supported native libraries, see: <a href="http://developer.android.com/sdk/ndk/overview.html" class="uri">http://developer.android.com/sdk/ndk/overview.html</a></li>
<li>There is indication in the forums and stackoverflow that Bluetooth cannot be accessed directly via the NDK (see: <a href="http://groups.google.com/group/android-ndk/browse_thread/thread/bd0834426b4264b9" class="uri">http://groups.google.com/group/android-ndk/browse_thread/thread/bd0834426b4264b9</a> and <a href="http://groups.google.com/group/android-ndk/browse_thread/thread/a2e3b5133f4a7a4b" class="uri">http://groups.google.com/group/android-ndk/browse_thread/thread/a2e3b5133f4a7a4b</a> and <a href="http://groups.google.com/group/android-ndk/msg/fe9b846a7ee37ba5" class="uri">http://groups.google.com/group/android-ndk/msg/fe9b846a7ee37ba5</a> and accepted answer at <a href="http://stackoverflow.com/questions/4205468/create-an-android-rfcomm-socket-without-any-input-from-the-user-how" class="uri">http://stackoverflow.com/questions/4205468/create-an-android-rfcomm-socket-without-any-input-from-the-user-how</a>)</li>
<li>It looks like Bluetooth support via the NDK was possible at one point via a hack involving the HTC released BlueZ sources: <a href="http://blog.blackwhale.at/2009/08/android-bluetooth-on-steroids-with-the-ndk-and-bluez/" class="uri">http://blog.blackwhale.at/2009/08/android-bluetooth-on-steroids-with-the-ndk-and-bluez/</a></li>
<li>A possible work around is to use the Android SDK's Java Bluetooth libraries for discovery, establishing connections, etc. and delegating the actual communications streams to RakNet by passing the in/out streams to RakNet via a JNI bridge.</li>
</ul></td>
</tr>
</tbody>
</table>

|-----------|
|  See Also |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><a href="index.html">Index</a><br /> <a href="autopatcher.html">Autopatcher</a><br /> <a href="cloudcomputing.html">CloudComputing</a><br /> <a href="connectiongraph.html">ConnectionGraph2</a><br /> <a href="crashreporter.html">CrashReporter</a><br /> <a href="replicamanager3.html">ReplicaManager3</a><br /> <a href="fullyconnectedmesh2.html">FullyConnectedMesh2</a><br /> <a href="natpunchthrough.html">NATPunchthrough</a><br /> <a href="packetlogger.html">PacketLogger</a><br /> <a href="readyevent.html">ReadyEvent</a><br /> <a href="RPC3Video.htm">RPC3</a><br /> <a href="teambalancer.html">TeamBalancer</a><br /> <a href="sqlite3loggerplugin.html">SQLite3LoggerPlugin</a>
<p><a href="packetlogger.html"></a><br /></p></td>
</tr>
</tbody>
</table>
