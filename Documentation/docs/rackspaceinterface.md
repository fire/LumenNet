<span style="background-color: rgb(255, 255, 255);">![Oculus VR,
Inc.](RakNet_Icon_Final-copy.jpg){width="150" height="150"}</span>\
\
  ---------------------------------------------------------------------------------------
  ![](spacer.gif){width="8" height="1"}C++ Interface to the Rackspace Cloud Servers API
  ---------------------------------------------------------------------------------------

+--------------------------------------------------------------------------+
| <span class="RakNetBlueHeader">Programatically manage application        |
| servers</span>\                                                          |
| \                                                                        |
| Rackspace provides an HTTPS based API described at                       |
| <http://docs.rackspacecloud.com/servers/api/v1.0/cs-devguide-20110112.pd |
| f>.                                                                      |
| RakNet supports HTTPS when using the class                               |
| [TCPInterface](tcpinterface.html) and compiled with                      |
| OPEN\_SSL\_CLIENT\_SUPPORT 1, defined in RakNetDefines.h.                |
|                                                                          |
| **Setup:**                                                               |
|                                                                          |
| <span class="RakNetCode">\#include "Rackspace.h"\                        |
| \#include "TCPInterface.h" </span>                                       |
|                                                                          |
| RakNet::Rackspace rackspaceApi;\                                         |
| RakNet::TCPInterface tcpInterface;\                                      |
| RakNet::RackspaceEventCallback\_Default callback;\                       |
| tcpInterface.Start(0, 0, 1);\                                            |
| rackspaceAPI.Authenticate(&tcpInterface, "myAuthURL",                    |
| "myRackspaceUsername", "myRackspaceAPIKey");\                            |
| while (1) {\                                                             |
| for (RakNet::Packet \*packet=tcpInterface.Receive(); packet;             |
| tcpInterface.DeallocatePacket(packet), packet=tcpInterface.Receive())    |
| rackspaceApi.OnReceive(packet);\                                         |
| rackspaceApi.OnClosedConnection(tcpInterface.HasLostConnection());\      |
| }                                                                        |
|                                                                          |
| The first three lines create an instance of TCPInterface, Rackspace, and |
| the event handler for Rackspace respectively. The fourth line starts the |
| TCPInterface class so outgoing connections can be initiated. The call to |
| Authenticate logs into Rackspace and internally stores a key that can be |
| used for subsequent operations. See the comments in Rackspace.h with the |
| Authenticate() function for more details on these parameters. The two    |
| lines in the inner loop pass connection events to the Rackspace class.   |
|                                                                          |
| Provided that authentication succeeds, Authenticate will return          |
| something other than UNASSIGNED\_SYSTEM\_ADDRESS to show that the        |
| connection completed. You will shortly thereafter get the callback       |
| OnAuthenticationResult with eventType set to RET\_Success\_204. After    |
| that you can call the other various functions exposed by the Rackspace   |
| class.                                                                   |
|                                                                          |
| If the connection is immediately closed with no response from the        |
| server, you probably forgot to compile with OPEN\_SSL\_CLIENT\_SUPPORT   |
| set to 1 in RakNetDefines.h or RakNetDefinesOverrides.h.                 |
|                                                                          |
| **Example of listing images:**                                           |
|                                                                          |
| rackspaceApi.ListImages();                                               |
|                                                                          |
| Wait for callback OnListImagesResult(RackspaceEventType eventType, const |
| char \*htmlAdditionalInfo)                                               |
|                                                                          |
| As described in the                                                      |
| [devguide](http://docs.rackspacecloud.com/servers/api/v1.0/cs-devguide-2 |
| 0110112.pdf),                                                            |
| the callback will return HTML event codes 200, 203, 400, 500, 503, 401,  |
| 400, or 413. 200 and 203 indicate success.The event code will be         |
| returned in the eventType parameter. The actual HTML will be returned in |
| htmlAdditionalInfo. Example output:                                      |
|                                                                          |
| <span class="RakNetCode">HTTP/1.1 200 OK\                                |
| Server: Apache-Coyote/1.1\                                               |
| vary: Accept, Accept-Encoding, X-Auth-Token\                             |
| Last-Modified: Tue, 29 Mar 2011 22:41:36 GMT\                            |
| X-PURGE-KEY: /570016/images\                                             |
| Cache-Control: s-maxage=1800\                                            |
| Content-Type: application/xml\                                           |
| Content-Length: 1334\                                                    |
| Date: Sat, 02 Apr 2011 15:15:19 GMT\                                     |
| X-Varnish: 601046147\                                                    |
| Age: 0\                                                                  |
| Via: 1.1 varnish\                                                        |
| Connection: keep-alive </span>                                           |
|                                                                          |
| &lt;?xml version="1.0" encoding="UTF-8" standalone="yes"?&gt;&lt;images  |
| xmlns="http://doc\                                                       |
| s.rackspacecloud.com/servers/api/v1.0"&gt;&lt;image name="Windows Server |
| 2008 R2 x64 -\                                                           |
| MSSQL2K8R2" id="58"/&gt;&lt;image name="Fedora 14" id="71"/&gt;&lt;image |
| name="Red Hat Ente\                                                      |
| rprise Linux 5.5" id="62"/&gt;&lt;image name="Windows Server 2003 R2 SP2 |
| x86" id="29"/\                                                           |
| &gt;&lt;image name="Oracle EL Server Release 5 Update 4"                 |
| id="40"/&gt;&lt;image name="Windows\                                     |
| Server 2003 R2 SP2 x64" id="23"/&gt;&lt;image name="Gentoo 10.1"         |
| id="19"/&gt;&lt;image name\                                              |
| ="Windows Server 2008 SP2 x86" id="31"/&gt;&lt;image name="Windows       |
| Server 2008 SP2 x64\                                                     |
| - MSSQL2K8R2" id="57"/&gt;&lt;image name="Ubuntu 9.10 (karmic)"          |
| id="14362"/&gt;&lt;image na\                                             |
| me="Ubuntu 10.04 LTS (lucid)" id="49"/&gt;&lt;image name="Arch 2010.05"  |
| id="55"/&gt;&lt;imag\                                                    |
| e name="Oracle EL JeOS Release 5 Update 3" id="41"/&gt;&lt;image         |
| name="Ubuntu 8.04.2 L\                                                   |
| TS (hardy)" id="10"/&gt;&lt;image name="CentOS 5.4"                      |
| id="187811"/&gt;&lt;image name="Fedora 1\                                |
| 3" id="53"/&gt;&lt;image name="Windows Server 2008 SP2 x64"              |
| id="24"/&gt;&lt;image name="Cent\                                        |
| OS 5.5" id="51"/&gt;&lt;image name="Ubuntu 10.10 (maverick)"             |
| id="69"/&gt;&lt;image name="Win\                                         |
| dows Server 2008 R2 x64" id="28"/&gt;&lt;image name="Windows Server 2008 |
| SP2 x86 - MSS\                                                           |
| QL2K8R2" id="56"/&gt;&lt;image name="Red Hat Enterprise Linux 5.4"       |
| id="14"/&gt;&lt;image nam\                                               |
| e="Debian 5.0 (lenny)" id="4"/&gt;&lt;image                              |
| name="Ubuntu256MBBigPacketTest" id="943818\                              |
| 4"/&gt;&lt;image name="RakNet-setup" id="9019637"/&gt;&lt;/images&gt;    |
|                                                                          |
| RakNet does not currently parse the XML for you. There is an XML parser  |
| found at DependentExtensions\\XML if you do not have your own.           |
|                                                                          |
| **Executing custom commands:**                                           |
|                                                                          |
| Rackspace provides an [XML                                               |
| Schema](http://docs.rackspacecloud.com/servers/api/v1.0/) with the list  |
| of operations and parameters for the API. Some commands are more         |
| complicated than what the Rackspace class provides. For these commands,  |
| you can call Rackspace::AddOperation() directly. For example, here is    |
| the command to create a server:                                          |
|                                                                          |
| RakNet::RakString xml(\                                                  |
| "&lt;?xml version=\\"1.0\\" encoding=\\"UTF-8\\"?&gt;"\                  |
| "&lt;server xmlns=\\"http://docs.rackspacecloud.com/servers/api/v1.0\\"  |
| name=\\"%s\\" imageId=\\"%s\\" flavorId=\\"%s\\"&gt;"\                   |
| "&lt;/server&gt;"\                                                       |
| ,name.C\_String() ,imageId.C\_String(), flavorId.C\_String());\          |
| AddOperation(RO\_CREATE\_SERVER, "POST", "servers", xml);                |
|                                                                          |
| You could extend the XML to support loading an image with files, for     |
| example.                                                                 |
|                                                                          |
| **Examples of server management with RakNet:**                           |
|                                                                          |
| Reboot crashed servers - Run one master server that monitors the other   |
| servers. The master server can be at a fixed IP address, or you can use  |
| [DynDNS](http://www.dyndns.com/) (There is also a C++ DynDNS class under |
| /Source). When you start a Rackspace server, have the Rackspace server   |
| connect to the master server. If the master server at some point gets    |
| ID\_CONNECTION\_LOST, have the master server call                        |
| Rackspace::RebootServer.                                                 |
|                                                                          |
| Matching capacity - Again run a master server. Using [CloudServer and    |
| CloudClient](cloudcomputing.html), inform the master server of the       |
| current connection load of each Rackspace server. If all servers are     |
| full or nearly so, use Rackspace::CreateServer to create a new server    |
| instance based on an image of your application. Similarly, if a server   |
| is completely empty, you can use Rackspace::DeleteServer to not incur    |
| the extra cost. Note that for game usage, we offer a hosted service      |
| [MasterServer2](http://masterserver2.raknet.com/).                       |
+--------------------------------------------------------------------------+

  -----------------------------------------------
  ![](spacer.gif){width="8" height="1"}See Also
  -----------------------------------------------

  -----------------------------------------
  [Index](index.html)\
  [Cloud Computing](cloudcomputing.html)\
  [Cloud Hosting](cloudhosting.html)\
  [TCP Interface](tcpinterface.html)
  -----------------------------------------


