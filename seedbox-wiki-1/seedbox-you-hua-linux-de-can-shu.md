---
description: 'https://post.smzdm.com/p/654616/'
---

# SeedBox优化Linux的参数

Linux这个系统，说心里话我也是个小白，对于服务器如何优化，也是google了一大堆文章，最终实测了一下，发现效果还是不错的，亲身经历，法国这台15欧的服务器，原先只能跑到30mb/s的速度，经过优化之后是真的跑到了80mb/s，基本上达到了这台服务器的满速带宽。不过有一个缺点就是不太稳定，我也在尽力的寻找更加妥善的优化速度的方案。所以，如果你觉得你服务器耐折腾，你觉得目前你的服务器速度让你不是很满意的，或者喜欢专研的小伙伴。可以尝试根据我提供的方法进行一些优化设置。**（优化有风险，操作需谨慎，重装是常事）**  


特别提醒：如果你正在用rutorrent进行下载，请先停止下载任务。因为优化后需要重启系统。所以会导致正在下载的文件下次启动的时候会重新校验**！强烈建议在优化的时候不要有任何下载行为**，因为不怕一万就怕万一，如果优化出错，导致系统打不开等等之类的就得不偿失了。

## **1、WinScp编辑文件：/etc/fstab**_**（建议略过该步有严重BUG）**_

[![PT&#x76D2;&#x5B50;&#x670D;&#x52A1;&#x5668;&#x53C2;&#x6570;&#x4F18;&#x5316;&#x4EE5;&#x53CA;Rutorrent&#x8BBE;&#x7F6E;&#x8C03;&#x6574;](https://am.zdmimg.com/201801/24/5a6835661c9f87018.png_e680.jpg)](https://post.smzdm.com/p/aex7kv3/pic_32/)

## **2、插入代码 noatime 到 errors=remount-ro 的前面** 分隔符为逗号（请参考下图）然后保存！_**（建议略过该步有严重BUG）**_ 

[![PT&#x76D2;&#x5B50;&#x670D;&#x52A1;&#x5668;&#x53C2;&#x6570;&#x4F18;&#x5316;&#x4EE5;&#x53CA;Rutorrent&#x8BBE;&#x7F6E;&#x8C03;&#x6574;](https://qnam.smzdm.com/201801/24/5a68375947b71513.png_e680.jpg)](https://post.smzdm.com/p/aex7kv3/pic_33/)

这一步的目的是，让系统不给文件做时间记录，因为我们在进行PT下载的时候，会有大量的硬盘读写行为，而默认情况下linux会把文件访问的时间atime做记录，比如最近一次的访问时间、最近的一次的修改时间等等。所以这里我们设置为 noatime 将会显著提高磁盘 IO 的效率、提升文件系统的性能。 （其实按照google上面的说法 加入data=writeback 也能提高IO，问题是，我加入了这个参数后，部分服务器失败，只有一台成功。因为自己技术有限，也不知道什么原因导致的，所以稳妥起见，还是不要加入这个了，只需要 noatime 这个参数就行。）

## **3、WinScp编辑文件：/etc/sysctl.conf** （把下面的文本复制到这个文件底部，**注意最后要有1个换行符**）

```text
vm.swappiness = 1
fs.file-max = 2000000
kernel.pid_max = 4194303
kernel.sched_migration_cost_ns = 5000000
kernel.sched_autogroup_enabled = 0
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_no_metrics_save = 0
net.ipv4.tcp_abort_on_overflow = 0
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_tw_reuse = 1
vm.dirty_background_ratio = 20
vm.dirty_ratio = 30
net.ipv4.tcp_rfc1337 = 1
net.ipv4.tcp_sack = 1
net.ipv4.tcp_fack = 1
net.ipv4.tcp_workaround_signed_windows = 1
net.ipv4.tcp_timestamps = 1
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_syn_retries = 2
net.ipv4.tcp_synack_retries = 2
net.ipv4.tcp_orphan_retries = 2
net.ipv4.tcp_retries2 = 8
net.ipv4.ip_local_port_range = 1024 65535
net.core.netdev_max_backlog = 3240000
net.core.somaxconn = 50000
net.ipv4.tcp_max_tw_buckets = 1440000
net.ipv4.tcp_max_syn_backlog = 3240000
net.ipv4.tcp_mtu_probing = 1
net.ipv4.tcp_fin_timeout = 15
net.ipv4.tcp_keepalive_time = 600
net.ipv4.tcp_keepalive_intvl = 10
net.ipv4.tcp_keepalive_probes = 9
net.ipv4.ip_no_pmtu_disc = 0
net.core.rmem_default = 16777216
net.core.wmem_default = 16777216
net.core.optmem_max = 16777216
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_rmem = 4096 87380 16777216
net.ipv4.tcp_wmem = 4096 65536 16777216
net.ipv4.tcp_adv_win_scale = 2
```

[![PT&#x76D2;&#x5B50;&#x670D;&#x52A1;&#x5668;&#x53C2;&#x6570;&#x4F18;&#x5316;&#x4EE5;&#x53CA;Rutorrent&#x8BBE;&#x7F6E;&#x8C03;&#x6574;](https://qnam.smzdm.com/201801/24/5a683e8de68a55926.png_e680.jpg)](https://post.smzdm.com/p/aex7kv3/pic_34/)

这一步的目的是修改Linux的内核参数，主要优化TCP等协议参数，调优网络质量。如果你之前就有过这方面的设置，或者用过别的脚本，比如开启BBR的时候顺带优化了这个参数。可以参考如上配置进行适当的修改即可。（What？啥是BBR？ 好吧，那是下一篇文章主要讲解的内容）

值得一提的是 vm.swappiness = 1 这个参数是告诉服务器对SWAP 分区的依赖程度。这样说很难理解，你可以把它理解为windows的虚拟内存。当物理内存不足时，会调用这个虚拟内存作为补充使用，从而使得系统稳定。那么虚拟内存大家都知道是存放在硬盘上的。所以过多的使用SWAP，也就会造成过多的读写硬盘，那么你的硬盘IO速度就降低了。所以，通过调整vm.swappiness这个参数的值，可以告诉linux系统如何使用SWAP。一般默认情况下他的值是60，如果你内存足够大，比如25欧的那款服务器有16G的内存，完全用不掉啊，就可以设置为1。我这里设置的是0，当然不是禁用了SWAP，而是告诉系统能少用就少用，能不用就不用。具体怎么选择看亲们自己定吧

## **4、WinScp编辑文件：/etc/security/limits.conf** （把下面这些参数给复制进去）然后保存！

```text
* soft nofile 2000000
* hard nofile 2000000
root soft nofile 2000000
root hard nofile 2000000
```

[![PT&#x76D2;&#x5B50;&#x670D;&#x52A1;&#x5668;&#x53C2;&#x6570;&#x4F18;&#x5316;&#x4EE5;&#x53CA;Rutorrent&#x8BBE;&#x7F6E;&#x8C03;&#x6574;](https://qnam.smzdm.com/201801/25/5a696169a03db2896.png_e680.jpg)](https://post.smzdm.com/p/aex7kv3/pic_35/)这一步的目的是，增加最大打开的文件数，对于PT下载这种频繁使用到文件读写的。当然得调整了。

## **5、Putty输入命令： reboot** （重启一下服务器，使得我们刚才所有的设置生效）

