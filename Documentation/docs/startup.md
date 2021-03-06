![Oculus VR, Inc.](RakNet_Icon_Final-copy.jpg){width="150"
height="150"}\
\
  -----------------
  Starting RakNet
  -----------------

+--------------------------------------------------------------------------+
| <span class="RakNetBlueHeader">StartupResult RakPeer::Startup( unsigned  |
| short maxConnections, SocketDescriptor \*socketDescriptors, unsigned     |
| socketDescriptorCount, int threadPriority );\                            |
| </span>\                                                                 |
| The first thing you should do is call RakPeerInterface::Startup().       |
| Startup() will                                                           |
|                                                                          |
| 1.  Generate RakNetGUID, a unique identifier identifying this instance   |
|     of RakPeerInterface. You can get it with <span                       |
|     class="RakNetCode">RakNetGUID g =                                    |
|     rakPeer-&gt;GetGuidFromSystemAddress(UNASSIGNED\_SYSTEM\_ADDRESS);</ |
| span>                                                                    |
| 2.  Allocate an array of reliable connection slots, defined by the       |
|     maxConnections parameter. This may be the maximum number of players  |
|     for your game, or you may want to allocate extra to keep some buffer |
|     slots open, and manually control who enters your game or not.        |
| 3.  Create 1 or more sockets, described by the socketDescriptors         |
|     parameter                                                            |
|                                                                          |
| Before calling Startup(), generally only raw UDP functions are           |
| available, including Ping(), AdvertiseSystem() and SendOutOfBand().      |
|                                                                          |
| maxConnections                                                           |
|                                                                          |
| RakNet preallocates most memory used for connecting to other systems.    |
| Specify maxConnections to be the maximum supported number of connections |
| (both incoming and outgoing) between this instance of RakPeerInterface   |
| and another instance. Note that if you want other systems to connect to  |
| you, you also need to call SetMaximumIncomingConnections() with a value  |
| lower than or equal to maxConnections.                                   |
|                                                                          |
| <span class="RakNetBlueHeader">socketDescriptors parameter</span>        |
|                                                                          |
| In 95% of the cases, you can pass something like:                        |
|                                                                          |
| SocketDescriptor(MY\_LOCAL\_PORT, 0);                                    |
|                                                                          |
| For MY\_LOCAL\_PORT, if running a server or peer, you must set this to   |
| whatever port you want the server or peer to run on. This is the         |
| remotePort parameter you will pass to Connect(). If running a client,    |
| you can set it if you want to, or use 0 to automatically pick a free     |
| port. Note that on Linux you need admin rights to use a value lower than |
| or equal to 1000. Some ports are also reserved, although there is        |
| nothing stopping you from using them. See                                |
| <http://www.iana.org/assignments/port-numbers>                           |
|                                                                          |
| If desired, you can also create an array of socket descriptors:          |
|                                                                          |
| <span class="RakNetCode">SocketDescriptor sdArray\[2\];\                 |
| sdArray\[0\].port=SERVER\_PORT\_1;\                                      |
| strcpy(sdArray\[0\].hostAddress, "192.168.0.1");\                        |
| sdArray\[1\].port=SERVER\_PORT\_2;\                                      |
| strcpy(sdArray\[1\].hostAddress, "192.168.0.2");\                        |
| if (rakPeer-&gt;Startup( 32, 30, &sdArray, 2 )\                          |
| OnRakNetStarted(); </span>                                               |
| This is for advanced users that want to bind multiple network cards. For |
| example one card to go to the secure server behind the LAN, and another  |
| to the internet. To access the different bindings, you would pass the    |
| index of the binding to use to functions in RakPeerInterface that have a |
| connectionSocketIndex parameter.                                         |
|                                                                          |
| IPV6 is the newer internet protocol. Instead of addresses such as        |
| 127.0.0.1, you may have an address such as fe80::7c:31f7:fec4:27de%14.   |
| Encoding takes 16 bytes instead of 4, so IPV6 is less efficient for      |
| games. On the positive side, NAT Punchthrough is not needed and should   |
| not be used with IPV6 because there are enough addresses that routers do |
| not need to create address mappings.                                     |
|                                                                          |
| IPV6 is disabled by default. To enable IPV6 support, set the socket      |
| family to AF\_INET6. For example:\                                       |
| <span class="RakNetCode">socketDescriptor.socketFamily=AF\_INET6;</span> |
|                                                                          |
| IPV6 sockets can only connect to other IPV6 sockets. Similarly, IPV4     |
| (The default) can only connect to other IPV4 sockets.                    |
|                                                                          |
| threadPriority                                                           |
|                                                                          |
| For Windows, this is the priority of the RakPeer update thread passed to |
| \_beginthreadex(). For Linux, it is passed to                            |
| pthread\_attr\_setschedparam() for pthread\_create(). The default        |
| parameter is -99999, which on Windows means use priority 0               |
| (NORMAL\_PRIORITY) and on Linux means use priority 1000. For Windows,    |
| the default parameter is fine. For Linux, you'll want to set this to be  |
| whatever value you normally use for normal priority threads.             |
+--------------------------------------------------------------------------+

  ----------
  See Also
  ----------

  ----------------------
  [Index](index.html)\
  [FAQ](faq.html)\
  ----------------------


