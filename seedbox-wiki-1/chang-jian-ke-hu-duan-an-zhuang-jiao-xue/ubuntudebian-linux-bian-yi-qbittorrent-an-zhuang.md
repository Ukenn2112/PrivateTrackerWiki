---
description: 'by 神北小毬 https://npchk.info/ubuntu-debian-install-qbittorrent/'
---

# Ubuntu/Debian Linux编译qBittorrent安装

## 前言

以下为安装最新或指定版本qBittorrent教学。

 适用于Ubuntu18或更新版本，Debian10或更新版本 

适用于qBittorrent3.3.11-4.2.5或更新版本

## 安装须知

[libtorrent](https://github.com/arvidn/libtorrent)是qBittorrent必要的后端程序，对软件性能有直接影响。

* libtorrent 1.0.11: 非常稳定，适合长时间使用，是普遍Seedbox商家/脚本使用的版本。 
* libtorrent 1.1.13: 性能更好，支援异步磁盘I/O，对高速种子比较友好，修复了1.1系列的各种问题。
* libtorrent 1.2.3 : 问题多，暂时不建议使用 
* libtorrent 1.0.11: 适用于qBittorrent3.3.11-4.1.3 
* libtorrent 1.1.13: 适用于qBittorrent4.0.0或更新版本 
* libtorrent 1.2.3 : 适用于qBittorrent4.2.0或更新版本

\(qBittorrent 4.1.4或更新版本: 要求libtorrent ≥ 1.1.10\)

下面请根据qBittorrent版本安装所需的libtorrent，看不懂的就安装libtorrent 1.1.13吧

## 安裝libtorrent

先安裝依賴包：

```text
apt update
apt install build-essential pkg-config automake libtool git libgeoip-dev python3 python3-dev
apt install libboost-dev libboost-system-dev libboost-chrono-dev libboost-random-dev libssl-dev
apt install qtbase5-dev qttools5-dev-tools libqt5svg5-dev zlib1g-dev
```

安裝libtorrent 1.1.13:

```text
wget https://github.com/arvidn/libtorrent/releases/download/libtorrent-1_1_13/libtorrent-rasterbar-1.1.13.tar.gz
tar xf libtorrent-rasterbar-1.1.13.tar.gz
cd libtorrent-rasterbar-1.1.13
./configure --disable-debug --enable-encryption --with-libgeoip=system
make -j$(nproc)
make install
ldconfig
```

安裝libtorrent 1.2.7:

```text
wget https://github.com/arvidn/libtorrent/releases/download/libtorrent_1_2_7/libtorrent-rasterbar-1.2.7.tar.gz
tar xf libtorrent-rasterbar-1.2.7.tar.gz
cd libtorrent-rasterbar-1.2.7
./configure --disable-debug --enable-encryption --with-libgeoip=system CXXFLAGS=-std=c++14
make -j$(nproc)
make install
ldconfig
```

## 安裝qBittorrent

https://github.com/qbittorrent/qBittorrent/releases  
可自行选择版本 qBittorrent4.2.5为例

```text
wget https://github.com/qbittorrent/qBittorrent/archive/release-4.2.5.tar.gz
tar xf release-4.2.5.tar.gz
cd qBittorrent-release-4.2.5
./configure --disable-gui --disable-debug
make -j$(nproc)
make install
```

## 设置开机自启

```text
nano /etc/systemd/system/qbittorrent.service
```

输入以下內容：

```text
[Unit]
Description=qBittorrent Daemon Service
After=network.target

[Service]
LimitNOFILE=512000
User=root
ExecStart=/usr/local/bin/qbittorrent-nox
ExecStop=/usr/bin/killall -w qbittorrent-nox

[Install]
WantedBy=multi-user.target
```

启用以上设置：

```text
systemctl enable qbittorrent.service
```

启动启动qBittorrent\(首次启动请按y确认条款\)

```text
qbittorrent-nox
```

按Ctrl+C退出

后台运行qBittorrent:

```text
systemctl start qbittorrent.service
```

## 安裝完成

访问WebUI：[http://你的IPADDRESS:8080/](http://你的IPADDRESS:8080/)   
默认用户名：admin 默认密码：adminadmin   
关闭qBittorrent命令: systemctl stop qbittorrent.service   
启动qBittorrent命令: systemctl start qbittorrent.service   
重启qBittorrent命令: systemctl restart qbittorrent.service

## 创建下载文件夹和设置权限

```text
mkdir /home/Downloads
chmod 777 /home/Downloads
```

把下载路径设置到/home/Downloads就OK了！

