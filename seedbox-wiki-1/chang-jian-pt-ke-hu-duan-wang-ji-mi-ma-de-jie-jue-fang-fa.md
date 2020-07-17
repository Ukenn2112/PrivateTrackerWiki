---
description: 'https://ymgblog.com/2018/03/28/458/'
---

# 常见PT客户端忘记密码的解决方法

## qBittorrent

QB 的配置文件一般在如下位置

```text
/root/.config/qBittorrent/qBittorrent.conf
```

打开后我们找到这一项

> WebUI\Password\_ha1=@ByteArray\(**5ebe2294ecd0e0f08eab7690d2a6ee69**\)

上面小括号内的内容就是你 webui 密码的 MD5 加密格式

你可以去 [http://www.md5online.org/](https://ymgblog.com/go.html/?url=http://www.md5online.org/) 这里解密你的密码，如果提示不存在的话，你可以将密码直接改成我上面的这个，这个解密后就是 **secret** 

![&#x5E38;&#x89C1; PT &#x5BA2;&#x6237;&#x7AEF;&#x5FD8;&#x8BB0;&#x5BC6;&#x7801;&#x7684;&#x89E3;&#x51B3;&#x65B9;&#x6CD5;](https://pic.ymgblog.com/images/wp/ymgblog_com/458-1.png)

改好后保存文件，然后重启下 QB 的客户端就完事了！

## ruTorrent

RT 说白了一般都是用 nginx 或者 apache 作为网页服务器运行的，你只需要找到他的密码配置文件就可以了。

比如如果是通过这个文章安装的 RT [https://ymgblog.com/2017/09/27/170/](https://ymgblog.com/2017/09/27/170/) ，可以直接执行以下命令修改密码

```text
htpasswd –c /etc/nginx/.htpasswd 新用户名
```

回车输入新密码就可以了。

如果是用别的环境安装的，没有 htpasswd 工具的话，安装一个就可以了

```text
apt-get install apache2-utils
```

然后还是用上面的命令再进行密码的修改！

当然如果你不想用这个 htpasswd 修改的话，你也可以直接在这个 [在线 htpasswd 生成器](https://ymgblog.com/go.html/?url=http://tool.oschina.net/htpasswd) 里生成，然后再修改 **.htpasswd** 文件就可以了！

## Deluge

deluge 忘了密码很简单，执行一条命令就行了

首先你需要停止运行的 **deluged deluge-web** 这两个进程

你可以运行命令

```text
ps -ef|grep deluge
```

用 **kill** 命令结束返回的对应的 deluge 进程的 ID 就可以了。

结束了 deluge 进程后，执行以下命令

```text
rm -rf /root/.config/deluge/web.conf*
```

然后重新启动 deluge 就可以了，这时候你的 webui 密码已经被重置为了 **deluge**

## Transmission



首先停止运行 Transmission

如果是通过这个教程安装 [https://ymgblog.com/2017/05/07/26/](https://ymgblog.com/2017/05/07/26/) 的，可以先运行命令

```text
/etc/init.d/transmission-daemon stop
```

这样 TR 就停止了

然后直接编辑文件

> /var/lib/transmission-daemon/.config/transmission-daemon/settings.json

将其中的 **“rpc-password”:** 的双引号内的内容改成你想设置的密码，这里不用管原来的是什么，直接输入你想设置的密码原文就行了，TR 启动后才会加密这个密码！

