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
    
    
