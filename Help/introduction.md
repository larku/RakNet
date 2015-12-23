![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)
|---------------|
|  Introduction |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><span class="RakNetBlueHeader">Installation</span><br /><br /> Please refer to the <a href="compilersetup.html">Compiler Setup</a> page, as all your questions should be answered there. If you have additional problems please refer to the <a href="faq.html">FAQ</a>, <a href="http://www.jenkinssoftware.com" class="uri">http://www.jenkinssoftware.com</a>, or <script type="text/javascript">
<!--
h='&#106;&#x65;&#110;&#x6b;&#x69;&#110;&#x73;&#x73;&#x6f;&#102;&#116;&#x77;&#x61;&#114;&#x65;&#46;&#x63;&#x6f;&#x6d;';a='&#64;';n='&#114;&#x61;&#x6b;&#x6b;&#x61;&#114;';e=n+a+h;
document.write('<a h'+'ref'+'="ma'+'ilto'+':'+e+'" clas'+'s="em' + 'ail">'+'contact us'+'<\/'+'a'+'>');
// -->
</script><noscript>&#x63;&#x6f;&#110;&#116;&#x61;&#x63;&#116;&#32;&#x75;&#x73;&#32;&#40;&#114;&#x61;&#x6b;&#x6b;&#x61;&#114;&#32;&#x61;&#116;&#32;&#106;&#x65;&#110;&#x6b;&#x69;&#110;&#x73;&#x73;&#x6f;&#102;&#116;&#x77;&#x61;&#114;&#x65;&#32;&#100;&#x6f;&#116;&#32;&#x63;&#x6f;&#x6d;&#x29;</noscript>. Advanced users can jump to the <a href="tutorial.html">code tutorial</a>. Beginners or those wishing to learn more about RakNet should keep reading.<br /><br /> API Description<br /><br /> RakNet is a game engine solely managing networking and related services. It includes game-level replication, patching, NAT punchthrough, and voice chat. It allows any application to communicate with other applications that also uses it, whether that be on the same computer, over a LAN, or over the internet. Although RakNet can be used for any networked application, it was developed with a focus on online gaming and provides extra functionality to facilitate the programming of online games as well as being programmed to address the most common needs of online games.<br /><br /> Networking 101<br /><br /> Game network connections usually fall under two general categories: peer to peer and client/server. Each of these are implemented in a variety of ways and with a variety of protocols. However, RakNet supports any topology.<br /><br />
<img src="clientserver.jpg" />
<br /> Generally speaking, the fastest computer with the best connection should act as the server, with other computers acting as the clients.<br /><br /> While there are many types of ways to encode packets, they are all transmitted as either UDP or TCP packets. TCP packets are good for sending files, but not so good for games. They are often delayed (resulting in games with a lot of lag) and arrive as streams rather than packets (so you have to implement your own scheme to separate data). UDP packets are good because they are sent right away and are sent in packets so you can easily distinguish data. However, the added flexibility comes with a variety of problems:<br />
<ul>
<li>UDP packets are not guaranteed to arrive. You may get all the packets you sent, none of them, or some fraction of them.</li>
<li>UDP packets are not guaranteed to arrive in any order. This can be a huge problem when programming games. For example you may get the message that a tank was destroyed before you ever got the message that that tank had been created!</li>
<li>UDP packets are guaranteed to arrive with correct data, but have no protection from hackers intercepting and changing the data once it has arrived.</li>
<li>UDP packets do not require a connection to be accepted. This sounds like a good thing until you realize that games without protection from this would be very easy to hack. For example, if you had a message that said &quot;Give such and such invulnerability&quot; a hacker could copy that message and send it to the server everytime they wanted invulnerability.</li>
<li>The UDP transport does not provide flow control or aggregation so it is possible to overrun the recipient and to send data inefficiently.</li>
</ul></td>
</tr>
</tbody>
</table>

|---------------------------------------------|
|  How does RakNet help me with these issues? |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">At the lowest level, RakNet's peer to peer class, RakPeerInterface provides a layer over UDP packets that handle these problems transparently, allowing the programmer to focus on the game rather than worrying about the engine.<br />
<ul>
<li>RakNet can automatically resend packets that did not arrive.</li>
<li>RakNet can automatically order or sequence packets that arrived out of order, and does so efficiently.</li>
<li>RakNet protects data that is transmitted, and will inform the programmer if that data was externally changed.</li>
<li>RakNet provides a fast, simple, connection layer that blocks unauthorized transmission.</li>
<li>RakNet transparently handles network issues such as flow control and aggreggation.</li>
</ul>
Of course, RakNet would be not much use if it handled these issues inefficiently such as by sending a lot of data, locking up with blocking operations, or making it hard to take advantage of these features. Fortunately, that is not the case.<br /><br /> Unlike some other networking APIs:<br />
<ul>
<li>RakNet adds very few bytes to your data.</li>
<li>RakNet does not incur overhead for features you do not use.</li>
<li>RakNet has nearly instantaneous connections and disconnections.</li>
<li>RakNet does not assume the internet is reliable. RakNet will gracefully handle connection problems rather than block, lock-up, or crash.</li>
<li>RakNet technology has been successfully used in dozens of games. It's been proven to work.</li>
<li>RakNet is easy to use.</li>
<li>RakNet is well-documented.  Every header file has every class and function documented.  There is a Doxygen manual as well as the HTML manual you are looking at.</li>
</ul></td>
</tr>
</tbody>
</table>

|----------------------------------|
|  What else can RakNet do for me? |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>Working at the level of byte streams and packets is bandwidth efficient and gives you a great deal of control but is time consuming. RakNet provides many features to make networking easier, including remote function calls, the BitStream class, and automatic object synchronization.<br /><br /> Most games share a common set of functionality, such as setting player limits, password production, and statistics. RakNet includes all these features and more.  If your game needs it you should check to make sure RakNet doesn't have it integrated already.</p>
<p>Lastly, RakNet includes programs and services that work in conjunction with your game, such as the <a href="http://masterserver2.raknet.com/">master server</a> or real time voice<br /><br /> Here is a partial list of things you can do &quot;out of the box.&quot;<br /></p>
<ul>
<li>Implement low-bandwidth voice communications.</li>
<li>Use our <a href="http://masterserver2.raknet.com/">master server</a> for players to find games on the internet.</li>
<li>Utilize remote function calls, allowing you to call functions on other computers with variable parameters.</li>
<li>Get statistics such as ping, packetloss, bytes sent, bytes received, packets sent, packets received, and more.</li>
<li>Optional per-packet timestamping so you know with a fair degree of accuracy how long ago an action was performed on another system despite ping fluctuations.</li>
</ul>
Next page: <a href="systemoverview.html">System Overview</a></td>
</tr>
</tbody>
</table>

|-----------|
|  See Also |

|---------------------------------------------------------|
| [Index](index.html)                                     
  [System Overview](systemoverview.html)                  
  [Detailed Implementation](detailedimplementation.html)  
  [Tutorial](tutorial.html)                               
  [Compiler Setup](compilersetup.html)                    |
