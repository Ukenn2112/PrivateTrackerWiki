# Ubuntu 18.04 手动修改 DNS 方法

使用root权限

```text
nano /etc/systemd/resolved.conf
```

这个文件的默认配置如下：

```text
[Resolve]
#DNS=
#FallbackDNS=
#Domains=
#LLMNR=no
#MulticastDNS=no
#DNSSEC=no
#Cache=yes
#DNSStubListener=yes
```

将 DNS 前面的 `#` 去掉并填入新的 DNS 地址，本文以修改成谷歌 DNS 为例，修改后如下：

```text
[Resolve]
DNS=1.1.1.1 1.0.0.1
#FallbackDNS=
#Domains=
#LLMNR=no
#MulticastDNS=no
#DNSSEC=no
#Cache=yes
#DNSStubListener=yes
```

修改完成后保存重启 `system resolve` 服务生效：

```text
systemctl restart systemd-resolved.service
```

