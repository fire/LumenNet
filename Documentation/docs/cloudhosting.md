<span style="background-color: rgb(255, 255, 255);">![Oculus VR,
Inc.](RakNet_Icon_Final-copy.jpg){width="150" height="150"}</span>\
\
  -----------------------------------------------------------------------------
  ![](spacer.gif){width="8" height="1"}How to setup cloud hosting with RakNet
  -----------------------------------------------------------------------------

+--------------------------------------------------------------------------+
| <span class="RakNetBlueHeader">Cloud hosting through Rackspace</span>\   |
| \                                                                        |
| Services such as the [autopatcher](autopatcher.html) require a server    |
| running RakNet. Technically,                                             |
| [NATPunchthroughServer](natpunchthrough.html) does as well, although we  |
| do [provide a free                                                       |
| server](http://www.jenkinssoftware.com/forum/index.php?topic=5006.0).    |
| While it is possible to run your server using a traditional host, such   |
| as [Hypernia](http://www.hypernia.com/) these services will cost you     |
| about \$150 USD a month. Scaling up the services also requires           |
| time-consuming installation of the codebase and it is not possible to do |
| so programmatically.                                                     |
|                                                                          |
| RakNet was tested with two cloud hosting providers: [Amazon              |
| AWS](http://aws.amazon.com/) and [Rackspace                              |
| Cloud](http://www.rackspace.com/cloud/). I could not get Amazon AWS to   |
| work with incoming UDP connections. Furthermore, peformance on loopback  |
| using the BigPacketTest project was very bad. Rackspace did not have     |
| these problems and the cost for their lowest-end Linux servers is low. I |
| also provide a [C++ interface to Rackspace](rackspaceinterface.html)     |
| allowing you to programatically control your servers, so it's a good     |
| starting point.                                                          |
|                                                                          |
| **Signing up**                                                           |
|                                                                          |
| ![](cloudhosting1.jpg)                                                   |
|                                                                          |
| \                                                                        |
| **Creating the server**                                                  |
|                                                                          |
| The first step is to create a server, using Hosting / Cloud Servers /    |
| Add Server. This will bring up a menu asking if you want Linux or        |
| Windows, and how much RAM. The low-end Linux servers are cheaper than    |
| the Windows servers. RakNet should work with either. Cloud server and    |
| NAT punchthrough server both take minimal RAM. The Autopatcher takes a   |
| lot of RAM however, I recommend four gigabytes to serve 256 concurrent   |
| users or eight gigabytes to serve 512.                                   |
|                                                                          |
| ![](cloudhosting2.jpg)                                                   |
|                                                                          |
| Once your server has been created, you will get an email telling you the |
| password and login IP.                                                   |
|                                                                          |
| ![](cloudhosting3.jpg)                                                   |
|                                                                          |
| For Windows 7, enter the username, password, and login IP using Remote   |
| Desktop, found under Start / Accessories.                                |
|                                                                          |
| ![](cloudhosting4.jpg)                                                   |
|                                                                          |
| **Setting up the server**                                                |
|                                                                          |
| Once logged in, server setup is the same as any computer.                |
|                                                                          |
| 1.  The default installation contains Internet Explorer, which you can   |
|     use to download [Firefox](http://www.mozilla.com/en-US/firefox/) or  |
|     [Chrome](http://www.google.com/chrome).                              |
| 2.  Use that webbrowser to [download                                     |
|     RakNet](http://www.jenkinssoftware.com/download.html).               |
| 3.  On Windows, you can download Visual C++ 2010 Express for free.       |
|     Installation will require a reboot.                                  |
|                                                                          |
| ![](cloudhosting5.jpg)                                                   |
|                                                                          |
| Once your server is setup, open the RakNet solution and compile          |
| normally. You now have a working server, using the IP address you        |
| connected to using Remote Desktop.                                       |
|                                                                          |
| **Backing up and scaling the server**                                    |
|                                                                          |
| Once you have the server setup the way you want it, you can create an    |
| image of the server, which is essentially a harddrive backup. This is    |
| important for scalability, because you can create a new instance of the  |
| server with the same configuration as your image.                        |
|                                                                          |
| ![](cloudhosting6.jpg)                                                   |
|                                                                          |
| When you are not running your server, be sure to delete the instance and |
| leave the much cheaper image. Unlike Amazon AWS which only charges for   |
| usage, Rackspace charges as long as your server exists at all. To start  |
| your server again, or to start multiple instances of the same server,    |
| use the Cloud Servers menu under My Server Images.                       |
|                                                                          |
| ![](cloudhosting7.jpg)                                                   |
+--------------------------------------------------------------------------+

  -----------------------------------------------
  ![](spacer.gif){width="8" height="1"}See Also
  -----------------------------------------------

  ------------------------------------------------
  [Index](index.html)\
  [Autopatcher](autopatcher.html)\
  [Cloud Computing](cloudcomputing.html)\
  [NAT punchthrough](natpunchthrough.html)\
  [Rackspace interface](rackspaceinterface.html)
  ------------------------------------------------


