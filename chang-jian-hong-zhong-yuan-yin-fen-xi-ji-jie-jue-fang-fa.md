# 常见红种原因分析及解决方法

 如果你是第一次使用该软件推荐阅读UT官方常见问题解答中文版：  
[http://www.utorrent.com/faq/index](https://www.utorrent.com/faq/index)  
  
红种问题产生的原因有很多，一般短时间的红种（几分钟或十几分钟）是因为连接不上Tracker服务器，一般稍等一会或更新一下Tracker（对该种子点右键，选择“更新Tracker”）  
就可以了。如果长时间的红种，请查看Tracker服务器返回的信息，Tracker信息会帮助我们判断红种产生的原因。然后对症下药，从而有效的解决红种问题。  
查看方法：  
（点击产生红种的种子，然后点下面的“伺服”，“伺服”里有站点的Tracker服务器返回的信息。）  
值得注意的是，tracker状态为“更新中”的种子不会红种，但是也和红种一样不会向tracker服务器反馈信息。  
  
下面给出了一些红种原因，如果能不能解决问题，请跟帖寻求帮助，我们将尽力帮助解决，最好有截图。  
  
以下为常见红种原因分析及解决办法：



**错误：1.失败:You already are downloading the same torrent.You may only leech  
from one location at a time.**  
  
原因：意外断网，意外重启，非法关闭客户端软件造成的服务器数据不同步，通常表现为：任务重复。  
  
解决方法：耐心等待半小时即可。  
  
**错误：2.Sorry，minimum announce interval=30 sec.Plesae retry after 30 sec**  
  
原因：频繁更新TRACKER地址导致，在手工更新TRACKER地址后，停止任务，且马上重新开始任务，就会造成minimum announce interval=30 sec的红种  
  
解决方法：勿频繁更新TRACKER，停止任务后不要急于手工更新tracker  
  
**错误：3.connection limit exceeded=Connection limit execeeded!You may only leech  
from one location at a time**  
  
原因：连接受限，据分析与最大连接数，以及TRACKER能承受的最大连接数有关  
  
解决方法：手动更新TRACKER即可解决  避免多IP下载同一种子。  
  
**错误：4.Invalid passkey!Please re-download .torrent file**  
  
原因：PASSKEY泄露或被盗，或者重置自己的PASSKEY之后没有重新下载种子  
  
解决方法：重置自己的PASSKEY之后右键你的任务属性中填入修改后的PASSKEY即可。  
  
**错误：5.被用户关闭连接**  
  
原因：估计和ISP封锁P2P软件有关，Tracker 被ISP劫持时容易出现此信息  
解决方法：手动将tracker地址更换为：  
[https://tracker.hdchina.club/announce.php?passkey=你的passkey](https://tracker.hdchina.club/announce.php?passkey=%E4%BD%A0%E7%9A%84passkey)  
若无效，点UT设置→任务 →协议加密 传出连接 中选强制  
  
**错误：6.torrent not register with this tracker**  
  
原因：已下载的种子文件由于各种原因被管理员删除。如果是剧集，可以搜索该剧集查看是否集中发布了合集。  
  
解决方法：删除任务，重新下载新的合集种子文件，查看合集中的任务文件是否与自己之前下的文件相同如果相同，  
可将已有的任务文件放入合集的文件夹中，在UT里指定文件位置，效验即可继续做种。  
  
**错误：7.离线\(超时\)..更新中...**  
  
原因：原因不明,估计和ISP封锁P2P软件有关，以及防火墙，路由器中封BT等设置有关  
  
解决方法：手动将tracker地址更换为：[https://tracker.hdchina.club/announce.php?passkey=你的passkey](https://tracker.hdchina.club/announce.php?passkey=%E4%BD%A0%E7%9A%84passkey)  
若无效，点UT设置→任务 →协议加密 传出连接 中选强制  
其他站点遇到此类问题可尝试改为https  
  
**错误：8.HTTP错误403/404**  
  
原因：原因不明。  
  
解决方法：可尝试停止任务30分钟后启动，或者手工更新TRACKER。  
  
**错误：9.HTTP错误500/502**  
  
原因：服务器临时维护或者负载过重。  
  
解决方法：  
  
等待客户端自动重试，或者手工更新TRACKER。  
  
**错误：10.TRACKER长时间提示 更新中. . .**  
  
原因：原因不明。  
  
解决方法：停止任务，退出UT，断网，重启电脑，联网，打开UT，启动任务。  
  
**错误：11. Windows socket error:由于系统缓冲区空间不足或列队已满,不能执行套接字上的操作.  
\(10055\),On API connect**  
  
原因：（官方解释）  
  
Norton GoBack 主要是其附带的 GBTray.exe 程序会导致出现这一问题. 你可以在运行 µTorrent 之前关闭 GBTray.exe 即可,  
如果仍然出现问题请禁用 Norton GoBack. 更新到最新的 v4.1 版本的 Norton Goback 可以完全解决这一问题.  
  
这一问题也有可能是由 mIRC 崩溃并退出而导致的. 当它崩溃并退出时会使 CPU 使用达到 99%. 建议使用任务管理器手动终止其进程.  
  
Windows 2000/XP/2003 的一项注册表设置也可能会导致出现这一问题, 请读取微软的下列文章并使用其提供的解决方案. KB196271  
  
这一问题还可能出现在没有破解 TCPIP.SYS 但将 µTorrent 中 net.max.halfopen 参数的值设置为超过 8 的情况下.  
请使用 EventID 4226 补丁\\` \(XP SP2 和 2003\). 即使已经破解过该文件, 也应该经常检查以免被微软新的更新补丁覆盖而导致破解失效.  
  
1、虚拟内存太小或者C盘满了。设置虚拟内存为更大，并保证C盘还有充足的空间。  
  
2、是你电脑里面某个软件的问题。  
  
这个错误可能是你计算机的Socket句柄资源用尽导致的，能够造成这种现象的一种情况就是你的计算机的某个程序不断的向某个连接发出连接申请，  
但是始终没能连上，没连上就会引发一个错误，如果编程的人没有写释放资源的代码，那么这个连接就始终占据着着一个句柄，  
于是由于不断的连接，最终导致 Socket句柄资源耗尽。  
  
3、注册表中的以下二项出现错误  
  
HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Services\Winsock  
  
HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Services\Winsock2  
  
解决办法：备份，然后找一台相同系统的机器，将以下注册表分支导出存为二个文件，  
  
HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Services\Winsock  
  
HKEY\_LOCAL\_MACHINE\SYSTEM\CurrentControlSet\Services\Winsock2  
  
最后将这二个导出的注册表文件导入到有问题的机器中即可。  
  
解决方法：推荐重装系统。  
  
**错误：12.Client is banned，Please read the FAQ for a list of allowed clients!**  
  
原因：与hdchina支持的客户端软件版本不符。  
  
解决方法：请安装hdchina支持的客户端软件  
  
**错误：13.tracker发出无效的数据**  
  
原因：有可能是服务器故障。  
  
解决方法：等待服务器恢复。  
  
**错误：14.The operation timed out**  
  
原因：与你的IP地址有关。  
  
解决方法：尝试与你的网络提供商联系解决办法。  
  
**错误：15.Your client is not allowed,or too old! Read the FAQ!＝This client is not allowed,  
or too old! Please read the FAQ!**  
  
原因：与hdchina支持的客户端软件版本不符。  
  
解决方法：请安装hdchina支持的客户端软件  
  
**错误：16.通常每个套接字地址\(协议/网络地址/端口\)只允许使用一次...**  
  
原因：互联网中的信息传输都是这样建立端口与端口的连接完成的，通过程序可以打开一些端口。但是每个端口只能同时给一个程序使用。  
出现这个问题就说明μTorrent.exe准备使用的端口被其他程序占用了。  
  
解决方法：关闭占用这个端口的程序。在UT端口设置中,换一个端口。  
  
**错误：17.Proxy connect error:由于目标机器积极拒绝，无法连接。**  
  
原因：  
您网络的DNS不稳定, 也会导致无法连接到服务器. 如果无法正确解释IP地址. 请您联系您的网络管理员.  
  
如果您的网络不通畅, 也可能导致此错误的发生, 请检查您的网络是否通畅. 有许多网络仅仅开放网页浏览, 对下载则不开放,  
对这种错误,您需要向您的网络服务商或者管理员咨询。  
  
解决方法：  
检查您是否安装了反病毒软件, 并打开了防火墙. 许多此类软件有可能会关闭UT的端口.  
因此要解决这些问题,请您重新设置您的软件, 或者直接关闭他们。  
  
**错误：18.无法找到主机名..**  
  
原因：  
1.ISP封锁了TRACKER  
2.DNS故障  
3.本地网络故障  
  
解决方法：  
1.首先确定本地网络有无问题，直接访问或者PING到TRACKER的连接能否贯通。  
2.换个DNS服务器，或者使用代理软件  
3.封锁了tracker的参考一下 （4.被用户关闭连接）中的处理方式。  
4.个别种子出现无法找到主机名..错误时，手工更新一下tracker地址一般可以解决。  
  
**错误：19.向一个无法连接的网络尝试了一个套接字操作**  
  
原因：（官方解释）  
  
这可能是由 BitDefender 导致的, 不是 µTorrent 的问题. 很遗憾的是, 禁用 BitDefender 是没有用的,  
你必须卸载 BitDefender 才能解决这一问题. 你可以使用其它反病毒软件来代替 BitDefender.  
  
解决方法：  
  
如果你确信这一问题不是由 BitDefender 引起的, 请尝试卸载或禁用防火墙程序. 如果能够正常工作的话,  
请到 µTorrent 论坛 发帖说明你使用的防火墙的名称.  
  
如果你没有使用防火墙软件的话, 请确认你已经安装了网卡的最新驱动, 因为这一问题也有可能是由错误的网卡驱动造成的.  
其它原因还包括网络连接被断开或是调制解调器/路由器被关闭。  
  
**错误：20.connection with the server could not be established...无法建立到服务器的连接**  
  
原因：有可能为自己网络故障。  
  
解决方法：检查一下自己的网络，看看是否是有网络的故障导致无法连接。或与网络提供商联系  
  
**错误：21.could not parse bencoded data..**  
  
原因：  
往往是无效的客户端身份验证（密钥，IP地址等），或者输入的下载地址、种子文件无效造成的。  
  
解决方法：  
重新下载种子文件，或者尝试网页前加S多试几次就可解决。如：[https://hdchina.club](https://hdchina.club/)  
  
**错误：22.失败：Tracker error\(120\).**  
  
原因：多为服务器问题。  
  
解决方法：也可尝试重置passkey。  
  
**错误：23. regular expression err for:%C2%B5Torrent/20277.please ask sysop to fix this**  
  
原因：与hdchina支持的客户端软件版本不符。  
  
解决方法：请安装hdchina支持的客户端软件  
  
**错误：24.browser access blocked**  
  
原因：此错误为路由器刷新固件导致。  
  
解决方法：尝试刷回固件，特别是特别版固件【也就是破除部分地区禁止共享网络限制的固件】更加容易这样  
  
**错误：25.you may only leech from one location at a time.**  
  
  
可能的原因：  
  （1）用户从一台以上电脑下载同一种子  
  解决方法：请检查你是否有此行为。  
  （2）用户PASSKEY泄露，其他人盗用该PASSKEY下载同一种子  
  解决方法：请重置PASSKEY，记得你正在上传或者下载的种子需到网站重新下载对应的种子文件再进行上传或下载。  
否则会产生PASSKEY错误的信息。重新下载该种子后，用UT打开该种子的时候，保存目录选择原来你下载设置的目录，  
然后UT就会进行校验。校验之后就会继续下载（或做种）。  
  （3）计算机发生意外，导致重启、UT突然退出，重新打开UT连接Tracker服务器的时候，由于Tracker服务器的延时，  
之前下载的种子还没从Tracker服务器里消失，这些种子重新连接时会被Tracker服务器认为有多个相同的种子文件。  
   
解决方法：一般过一段时间后会恢复正常。你可以尝试点击自己id，清除冗余种子，然后更新下ut中的伺服，一般过会就好。  
  
**错误：26 .Error 12057**  
  
原因：\(微软官方解释\)  
Indicates that revocation cannot be checked because the revocation server was offline \(equivalent to CRYPT\_E\_REVOCATION\_OFFLINE\).  
该服务器证书吊销不能被检验，因为吊销服务器脱机（相当于CRYPT\_E\_REVOCATION\_OFFLINE）指示。  
  
解决方法：  
Internet Explorer&gt;“工具&gt;”Internet选项&gt;“高级”选项卡&gt;安全选项组,取消选中框旁边的选项，“检查服务器证书吊销”。  
（需要Internet Explorer的重新启动才能生效）  
μTorrent官方论坛回答:  
[http://forum.utorrent.com/viewtopic.php?id=38186](https://forum.utorrent.com/viewtopic.php?id=38186)  
  
**错误： 27.失败：anti-cheater：you can not use this agent**  
  
原因：客户端被认为作弊。  
解决方法：  
更换其他版本客户端，并向网站程序员反映该版本客户端问题。  
  
**错误： 28.下载的文件，路径改变变成红种了**  
  
原因：  
  
解决方法：停止该种子，右键——高级——设置下载位置。  
选择你的文件新位置，确定。如果问你覆盖就选否（如果没问就忽略这一步）。  
右键——强制校验。  
校验结束后再开始就行了。  
  
**错误：29.失败：Tracker error 2006**  
  
原因：  
  
解决方法：删掉种子和数据，重新加载种子，重复多次  
个人经验是多更新几次Tracker，过一会也能变好。  
   
  
**错误： 30.失败:malformed announce**  
  
原因：发布种子后直接用本地制作的种子在ut种做种  
  
解决方法：在种子页面下载种子做种  
  
  
**未完待续......**  
  
以上内容均为转载汇总

