# ShadowSocksR
ShadowSocksR-Install-Debian-BBR


## TCP-BBR 开启教程 —— 比锐速还强的 TCP拥塞控制技术
  
  **注意：** TCP-BBR和锐速一样，不支持Openvz，请先确定你的VPS的虚拟化技术！
  
  **注意：** TCP-BBR必须是 2016-12-05 21:00 更新的 4.9.0-rc8 内核及以后的版本 才能开启，而锐速并不支持这个最新的内核版本，所以TCP-BBR和锐速是不能共存的。

**BBR 简单介绍** 

BBR 是一个由谷歌社区开发的 TCP拥塞控制技术，目前处于开发初期，但是前景很棒，大家可以持续关注，同时BBR是集成与Linux最新版本的内核中的。
BBR官方项目地址：https://github.com/google/bbr

**启动步骤** 

Debian 9 x64 系统的内核为4.9.0-3, 不用更换内核。

  **开启bbr**
  ```
  echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
  echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
  ```
  _执行这个保存生效更改_
  
  `sysctl -p`
  
  **关闭bbr**
  ```
  sed -i '/net\.core\.default_qdisc=fq/d' /etc/sysctl.conf && sed -i '/net\.ipv4\.tcp_congestion_control=bbr/d' /etc/sysctl.conf
  sysctl -p
  ```
  _执行完上面的代码，就使用reboot重启VPS后才能关闭bbr，重启后再用下面的查看bbr状态代码，查看是否关闭了。_
  
  `reboot`
  
  **查看bbr是否开启**
  
  执行下面命令，如果结果中有bbr，即证明你的内核已开启bbr.
    
   `sysctl net.ipv4.tcp_available_congestion_control`
    
  执行下面命令，看到有 tcp_bbr 模块，即说明bbr已启动.
    
   `lsmod | grep bbr`
 
 ---
 
## 一键安装 ShadowsocksR 服务端


### 使用方法

使用root用户登录，运行以下命令：

```
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh 2>&1 | tee shadowsocksR.log
```

安装完成后，脚本提示如下：

```
Congratulations, ShadowsocksR server install completed!
Your Server IP        :your_server_ip
Your Server Port      :your_server_port
Your Password         :your_password
Your Protocol         :your_protocol
Your obfs             :your_obfs
Your Encryption Method:your_encryption_method

Welcome to visit:https://shadowsocks.be/9.html
Enjoy it!
```

### 卸载方法：

使用 root 用户登录，运行以下命令：

```
./shadowsocksR.sh uninstall
```

### 安装完成后即已后台启动 ShadowsocksR ，运行：

```
/etc/init.d/shadowsocks status
```

### 可以查看 ShadowsocksR 进程是否已经启动。
#### 本脚本安装完成后，已将 ShadowsocksR 自动加入开机自启动。

使用命令：

```
启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status

配置文件路径：/etc/shadowsocks.json
日志文件路径：/var/log/shadowsocks.log
代码安装目录：/usr/local/shadowsocks
```

#### 默认配置：

```
服务器端口：自己设定（如不设定，默认为 8989）
密码：自己设定（如不设定，默认为 teddysun.com）
加密方式：自己设定（如不设定，默认为 aes-256-cfb）
协议（Protocol）：自己设定（如不设定，默认为 origin）
混淆（obfs）：自己设定（如不设定，默认为 plain）
```

#### 多用户配置示例:

```
{
"server":"0.0.0.0",
"server_ipv6": "[::]",
"local_address":"127.0.0.1",
"local_port":1080,
"port_password":{
    "8989":"password1",
    "8990":"password2",
    "8991":"password3"
},
"timeout":300,
"method":"aes-256-cfb",
"protocol": "origin",
"protocol_param": "",
"obfs": "plain",
"obfs_param": "",
"redirect": "",
"dns_ipv6": false,
"fast_open": false,
"workers": 1
}
```

#### 更新日志：

2017 年 07 月 27 日：

1、新增：可选协议（protocol）auth_chain_b 。使用该协议需更新到最新版（4.7.0）ShadowsocksR 版客户端；
2、修改：更新 ShadowsocksR 源码下载地址。

2017 年 07 月 22 日：

1、新增：安装时可选 13 种加密方式的其中之一（none 是不加密）。如下所示：

```
none
aes-256-cfb
aes-192-cfb
aes-128-cfb
aes-256-cfb8
aes-192-cfb8
aes-128-cfb8
aes-256-ctr
aes-192-ctr
aes-128-ctr
chacha20-ietf
chacha20
rc4-md5
rc4-md5-6
```

2、新增：安装时可选 7 种协议（protocol）的其中之一。如下所示：

```
origin
verify_deflate
auth_sha1_v4
auth_sha1_v4_compatible
auth_aes128_md5
auth_aes128_sha1
auth_chain_a
auth_chain_b
```

3、新增：安装时可选 9 种混淆（obfs）的其中之一。如下所示：

```
plain
http_simple
http_simple_compatible
http_post
http_post_compatible
tls1.2_ticket_auth
tls1.2_ticket_auth_compatible
tls1.2_ticket_fastauth
tls1.2_ticket_fastauth_compatible
```

2016 年 08 月 13 日：

1、新增多用户配置示例。注意：如果你新增了端口，也要将该端口从防火墙（iptables 或 firewalld）中打开。

2016 年 05 月 12 日：

1、新增在 CentOS 下的防火墙规则设置。

