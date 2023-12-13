Ubuntu开机卡在 A start job is runing for wait for Network to be configured (1min 23s / no limit)解决方法
问题现象：
Ubuntu开机卡在这里迟迟无法开机，要等倒计时完以后才会顺利开机。原因可能是系统开机初始化网络配置出错，加上系统默认配置有等待时间，导致系统会一直进行一些无用的尝试，直到超过等待时间，这样无形之中加长了开机的时间。

解决思路及方法：（两种）
首先想到的解决方法是修复配置上的错误使初始化顺利完成，第二是调整的等待时间，使其快速跳过。
（我这里提供两种解决方法，大家酌情取用）

一、修改网络配置
网络配置文件：
查看网络配置文件如下:

# This is the network config written by 'subiquity'
network:
  ethernets:
    ens33:
      dhcp4: true
    ens34:
      dhcp4: true
  version: 2


这里我虚拟机的ens34网卡是VMvera Host only模式下虚拟的一张网卡，是没有开启DHCP的，我是准备配置静态IP地址与宿主机通信的，但是这里我选择的是DHCP,怀疑是这里的问题，遂修改配置文件。
提示：修改的配置如下

root@k8s-master1:~# vim /etc/netplan/00-installer-config.yaml
# This is the network config written by 'subiquity'
network:
    version: 2
    ethernets:
        ens33:
            dhcp4: yes
            nameservers:
                search: [mydomain, otherdomain]
                addresses: [192.168.121.1, 8.8.8.8]
            dhcp4-overrides:
                route-metric: 0
        ens34:
            addresses:
                - 192.168.10.14/24
            nameservers:
                search: [mydomain, otherdomain]
                addresses: [192.168.10.254, 1.1.1.1]
            dhcp4-overrides:
                route-metric: 100

root@k8s-master1:~# netplan apply
root@k8s-master1:~#
加载配置，没有报错说明加载成功。
重启后就没出现过等待的现象了。

二、修改初始化失败后的尝试及等待时间
这种方法是大家普遍使用的，网上此类相同问题的处理方法都是这种。
直接修改相关配置文件：
在【service】代码快最末尾加上TimeoutStartSec=2sec

root@k8s-master1:~#S
root@k8s-master1:/etc/systemd/system/network-online.target.wants# ll
total 8
drwxr-xr-x  2 root root 4096 Aug 24  2021 ./
drwxr-xr-x 19 root root 4096 Mar 24 08:11 ../
lrwxrwxrwx  1 root root   56 Aug 16  2021 systemd-networkd-wait-online.service -> /lib/systemd/system/systemd-networkd-wait-online.service


修改完如下所示：

#  SPDX-License-Identifier: LGPL-2.1+
#
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.

[Unit]
Description=Wait for Network to be Configured
Documentation=man:systemd-networkd-wait-online.service(8)
DefaultDependencies=no
Conflicts=shutdown.target
Requires=systemd-networkd.service
After=systemd-networkd.service
Before=network-online.target shutdown.target

[Service]
Type=oneshot
ExecStart=/lib/systemd/systemd-networkd-wait-online
RemainAfterExit=yes
TimeoutStartSec=2sec
[Install]
WantedBy=network-online.target



https://blog.csdn.net/weixin_42300866/article/details/123712774