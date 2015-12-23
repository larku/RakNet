<span style="background-color: rgb(255, 255, 255);">![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg)</span>
|-------------------------------------------------------|
| ![](spacer.gif)How to setup cloud hosting with RakNet |

<table>
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><span class="RakNetBlueHeader">Cloud hosting through Rackspace</span><br /><br /> Services such as the <a href="autopatcher.html">autopatcher</a> require a server running RakNet. Technically, <a href="natpunchthrough.html">NATPunchthroughServer</a> does as well, although we do <a href="http://www.jenkinssoftware.com/forum/index.php?topic=5006.0">provide a free server</a>. While it is possible to run your server using a traditional host, such as <a href="http://www.hypernia.com/">Hypernia</a> these services will cost you about $150 USD a month. Scaling up the services also requires time-consuming installation of the codebase and it is not possible to do so programmatically.</p>
<p>RakNet was tested with two cloud hosting providers: <a href="http://aws.amazon.com/">Amazon AWS</a> and <a href="http://www.rackspace.com/cloud/">Rackspace Cloud</a>. I could not get Amazon AWS to work with incoming UDP connections. Furthermore, peformance on loopback using the BigPacketTest project was very bad. Rackspace did not have these problems and the cost for their lowest-end Linux servers is low. I also provide a <a href="rackspaceinterface.html">C++ interface to Rackspace</a> allowing you to programatically control your servers, so it's a good starting point.</p>
<p><strong>Signing up</strong></p>
<p><img src="cloudhosting1.jpg" /></p>
<p><br /> <strong>Creating the server</strong></p>
<p>The first step is to create a server, using Hosting / Cloud Servers / Add Server. This will bring up a menu asking if you want Linux or Windows, and how much RAM. The low-end Linux servers are cheaper than the Windows servers. RakNet should work with either. Cloud server and NAT punchthrough server both take minimal RAM. The Autopatcher takes a lot of RAM however, I recommend four gigabytes to serve 256 concurrent users or eight gigabytes to serve 512.</p>
<p><img src="cloudhosting2.jpg" /></p>
<p>Once your server has been created, you will get an email telling you the password and login IP.</p>
<p><img src="cloudhosting3.jpg" /></p>
<p>For Windows 7, enter the username, password, and login IP using Remote Desktop, found under Start / Accessories.</p>
<p><img src="cloudhosting4.jpg" /></p>
<p></p>
<p><strong>Setting up the server</strong></p>
<p>Once logged in, server setup is the same as any computer.</p>
<ol>
<li>The default installation contains Internet Explorer, which you can use to download <a href="http://www.mozilla.com/en-US/firefox/">Firefox</a> or <a href="http://www.google.com/chrome">Chrome</a>.</li>
<li>Use that webbrowser to <a href="http://www.jenkinssoftware.com/download.html">download RakNet</a>.</li>
<li>On Windows, you can download Visual C++ 2010 Express for free. Installation will require a reboot.</li>
</ol>
<p><img src="cloudhosting5.jpg" /></p>
<p>Once your server is setup, open the RakNet solution and compile normally. You now have a working server, using the IP address you connected to using Remote Desktop.</p>
<p><strong>Backing up and scaling the server</strong></p>
<p>Once you have the server setup the way you want it, you can create an image of the server, which is essentially a harddrive backup. This is important for scalability, because you can create a new instance of the server with the same configuration as your image.</p>
<p><img src="cloudhosting6.jpg" /></p>
<p>When you are not running your server, be sure to delete the instance and leave the much cheaper image. Unlike Amazon AWS which only charges for usage, Rackspace charges as long as your server exists at all. To start your server again, or to start multiple instances of the same server, use the Cloud Servers menu under My Server Images.</p>
<img src="cloudhosting7.jpg" /></td>
</tr>
</tbody>
</table>

|-------------------------|
| ![](spacer.gif)See Also |

|-------------------------------------------------|
| [Index](index.html)                             
  [Autopatcher](autopatcher.html)                 
  [Cloud Computing](cloudcomputing.html)          
  [NAT punchthrough](natpunchthrough.html)        
  [Rackspace interface](rackspaceinterface.html)  |
