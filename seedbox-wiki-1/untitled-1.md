---
description: >-
  常用的RSS管理Web by https://github.com/Aniverse/WiKi/ &
  https://ymgblog.com/2018/04/30/396/
---

# Flexget 安装&配置 入门级教程

## 一、安装 Flexget

在星大脚本里选择了安装Flexget无需再次安装

安装 pip

```text
apt-get updateapt-get install python-pip python-setuptools nano
```

更新 pip

```text
pip install --upgrade pip
```

安装 flexget，因为现在 pip 已经更新到 10.0.X 版本了，名称变成了 pip2。

```text
pip2 install flexget
```

这样 Flexget 就安装完成了！

## 二、配置 Flexget

首先说明一下，我这里目前只提供最基本的 flexget 的使用方法。

创建 flexget 的配置目录

```text
mkdir /root/.flexget
```

编辑 flexget 的配置文件

```text
nano /root/.flexget/config.yml
```

这里提供几个示例供使用

注意： 1. 新手最好在 SSH 或者 WebUI （WebUI如何开启见后文）下编辑 Flexget 配置文件；在 Windows 下也不要用系统自带的记事本编辑，容易出错（换行问题之类的）  
2. Flexget 配置文件遵循 YAML 格式，请注意空格和缩进  
3. `inexistence` 脚本安装完后的 Flexget 后默认不启用 RSS 功能

如果需要使用 RSS 功能，主要有两个办法（如何使用见后文）： 1. 使用 daemon 模式的 schedules 2. 使用 cron

### 配置文件示例

```yaml
# 预设模板
templates:
# 剩余空间模板，当 path 对应的路径的剩余空间小于 space 规定的数值的时候停止 RSS 下载
  freespace:
    free_space:
      path: /home/SCRIPTUSERNAME
      space: 10240
# qb 的模板，之后写 qb 就是指把种子推送到 qb 进行下载；下面 tr de rt 也是如此
# 星大脚本里账号密码都帮你写好了，除非你自己改了账号、密码或者端口，不然以下这些客户端设置不用修改
  qb:
    qbittorrent:
      path: /home/SCRIPTUSERNAME/qbittorrent/download/
      host: localhost
      port: 2017
      username: SCRIPTUSERNAME
      password: SCRIPTPASSWORD
  tr:
    transmission:
      path: /home/SCRIPTUSERNAME/transmission/download/
      host: localhost
      port: 9099
      username: SCRIPTUSERNAME
      password: SCRIPTPASSWORD
  de:
    deluge:
      path: /home/SCRIPTUSERNAME/deluge/download/
      host: localhost
      port: 58846
      username: SCRIPTUSERNAME
      password: SCRIPTPASSWORD
# 体积过滤模板，min 是符合条件的最小种子体积，max 是符合条件的最大种子体积，单位均为 MB
# strict 默认是 yes，表示在无法确定大小的情况下就不下载，这里把它改成 no 了
# 也就是说，这段 size 的意思是，只下载体积为 6000-666666 MB 的种子，其他不满足条件的种子不下载
  size:
    content_size:
      min: 6000
      max: 666666
      strict: no

# 任务
tasks:
# Web-HDSky 是任务名称，基本上随便起
  Web-HDSky:
  # RSS 链接请自己修改成你实际的链接
    rss: https://hdsky.me/torrentrss.php
    # 因为 HDSWEB 发单集的时候用的标题是一样的， 因此下过一次后
    # 之后新发出来的单集由于标题一样，flexget 会当成是以前已经下过的种子
    # 为了避免这个问题，对 seen 插件设定为只检查 url 是否一致
    seen:
      fields:
        - url
  # 正则表达式；标题带 HDSWEB 的种子就下载（accept，接受），不想下载的话就写拒绝（reject）
    regexp:
      accept:
        - HDSWEB
  # 调用上边的 de 模板
    template: de
  # 可以不使用模板的体积过滤，针对每个任务单独设置体积过滤
    content_size:
      min: 3000
      max: 500000
      strict: no
  # 以下设定实现的效果：对这个任务加载到 deluge 的种子，自动添加 WEB-DL 的标签
  # 自动限制上传速度到 100MB/s（防止超速 ban），下完后自动移动到 /mnt/HDSky/HDSWEB
    deluge:
      label: WEB-DL
      # Limit upload speed to 100 MiB/s in case of being auto-banned
      max_up_speed: 102400
      move_completed_path: /mnt/HDSky/HDSWEB
  ADC-AnimeBD-JPN:
    rss: http://asiandvdclub.org/rss.xml
    if:
      - "'Anime' and 'AVC' in title": accept
      - "'subs only' in title": reject
      - "'Custom' in description": reject
      # 这三个过滤条件组合起来就是，下载标题里带 Anime 和 AVC 且不含 subs only 的种子
      # 并排除掉 描述页 里含有 Custom 字眼的种子
      # 这也就约等于，RSS 日版动画蓝光碟（非日版、DIY 碟、DVD 都过滤掉）
    # RSS ADC 需要 Cookies，这里我们用 headers 插件来加上 cookies
    # 如何获取 Cookies 请看另外一篇教程
    headers:
      Cookie: "uid=12345; pass=abcdefg"
    # 转换 RSS 链接，将原本形如 http://asiandvdclub.org/details.php?id=123456 的种子描述页面链接
    # 替换为形如 http://asiandvdclub.org/download.php?id=123456 的种子下载链接
    urlrewrite:
      sitename:
        regexp: 'http://asiandvdclub.org/details.php\?id=(?P<id>\d+)'
        format: 'http://asiandvdclub.org/download.php?id=\g<id>'
    qbittorrent:
      label: ADC
      # 刷 ADC 不用限速，我这里写这个限速模板只是想告诉你
      # Flexget 支持添加种子到 qBittorrent 的时候自动设定单种限速
      maxdownspeed: 30000

# Flexget WebUI 设定 （WebUI如何开启见后文）
web_server:
  port: 6566
  web_ui: yes
# base_url: /flexget
# base_url 是为了反代设置的，需要使用反代的话就取消这个的注释，然后在安装了 rTorrent 的情况下（不装 rt 的话没有 nginx）
# Flexget WebUI 地址就变成了 https://你盒子的 IP 地址/flexget

# 这里关闭 schedules 功能，也就是说没有启用 RSS，如何启用请看下文
schedules: no
```

### 配置 WebUI

上面示例配置文件**的web\_server**参数，这个是用来开启 Flexget 的 WebUI 的，需要执行以下命令！

设置 Web 密码，注意不太太简单，否则不成功！用户名默认是 flexget

```text
flexget web passwd 要设置的密码
```

开启 WebUI 后台运行

```text
flexget daemon start --daemonize
```

这时你打开 http://服务器 IP:6566 就能看到 flexget 的 web 界面了，登录后即可在 config 那里直接修改配置文件了！

### 测试

输入这一句命令测试：

```text
flexget --test execute
```

如配置文件存在语法错误，会提示你在第几行有什么错误  
如果配置文件没问题的话则会测试执行，不会真的下种子，可以用来测试配置文件写得是否合乎预期

第一次使用 RSS 的时候难免会下载到一些已经出种的老种，为了解决这个情况，第一次执行 flexget 的时候，可以用

```text
flexget execute --learn
```

这样子不会下载种子，但是会把这次 RSS 到的种子标记为已下载，这样之后就不会下到老种了

### 开启 RSS

#### 方法 1 ：使用 schedules

tasks 处写要执行 RSS 的任务名称，`minutes: 3` 表示每隔 3 分钟执行一次上述任务  
可以针对不同的 tasks 采用不同的 RSS 周期

```yaml
schedules:
  - tasks: [HDChina,TTG]
    interval:
      minutes: 3
  - tasks: [Gods]
    interval:
      minutes: 1
```

#### 方法 2 ：使用 cron

使用 cron 的话，schedules 最好改成 no，正如脚本里默认的那样（`schedules: no`）  
首先在 SSH 输入 crontab -e，选择一个文本编辑器，默认的 nano 即可 （nano 使用教程：[http://man.linuxde.net/nano）](http://man.linuxde.net/nano）)  
设置 2 分钟执行一次 RSS，就在文件里（写在哪个位置无所谓，顶部也行、底部也行）输入：

```text
*/2 * * * * /usr/local/bin/flexget --cron execute
```

（这里的就是 2 分钟执行一次的意思）  
保存、退出，之后可以在 WebUI 中看 Log 来判断 Flexget 是否有在正常工作，或者在 SSH 中输入 `flexget status` 查看状态

以上两个办法都是最短 1 分钟执行 1 次 RSS，如果你想实现更高的频率得用别的办法，这里不作介绍了

**提示**：一般情况下不建议 RSS 频率太高，一是因为 Flexget 本身执行过程中会消耗一些系统资源，频率太高可能服务器撑不住；二是因为 RSS 频率太高可能有些站点认为你是在做恶意攻击。至于多久一次算是频率太高，你自己看着办吧……

### 如何抓取免费种：

[https://github.com/Alvinnfly/promotion](https://github.com/Alvinnfly/promotion)

[https://github.com/Juszoe/flexget-nexusphp](https://github.com/Juszoe/flexget-nexusphp)

星大脚本有安装此插件

#### [flexget-nexusphp](https://github.com/Juszoe/flexget-nexusphp) 完整配置示例

#### 免费热种

```text
tasks:
  my-free-task:
    rss: 
      url: https://www.example.com/rss.xml
      other_fields:
        - link
    nexusphp:
      cookie: 'a=xxx; b=xxx'
      discount:
        - free
        - 2xfree
      seeders:
        min: 1
        max: 3
      leechers:
        min: 5
        max_complete: 0.5
    download: ~/flexget/torrents/
```

#### 热种

```text
tasks:
  my-hot-task:
    rss: 
      url: https://www.example.com/rss.xml
      other_fields:
        - link
    nexusphp:
      cookie: 'a=xxx; b=xxx'
      seeders:
        min: 1
      leechers:
        min: 20
    download: ~/flexget/torrents/
```

#### 避免HR

```text
tasks:
  no-hr-task:
    rss: 
      url: https://www.example.com/rss.xml
      other_fields:
        - link
    nexusphp:
      cookie: 'a=xxx; b=xxx'
      hr: no
    download: ~/flexget/torrents/
```

