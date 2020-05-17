# 1. DHCP协议

## 1.1 IP的获取

主机刚开始连接网络时, 是没有IP的.  所以需要先获取IP:

手动配置:

​		静态IP(statc), 自己手动设置

自动获取: 

​		动态IP(dynamic), 通过DHCP(Dynamic Host Configuration Protocol)服务器获取IP

## 1.2 DHCP获取IP

由于主机接入网络时, 没有IP地址不能发送IP数据报. 但是有MAC地址, 所以可以发送以太网帧. 和arp协议寻找指定ip的mac地址很类似:

* 第一步

  主机接入后, 利用mac地址发送到`ff:ff:ff:ff:ff:ff`广播帧. (DHCP DISCOVER 帧)

* 第二步

  DHCP服务器收到广播后, 回复一个帧.(DHCP OFFER 帧), 此帧包含了IP, 掩码, 有时会有默认网关, DNS等.

* 第三步

  主机收到以太网帧后, 广播发送请求帧(DHCP REQUREST 帧). 表示接受这个OFFER的分配.

* 第四部

  DHCP服务器收到广播后, 发送确认帧(DHCP ACK帧). 表示同意这个OFFER的租约.

值得注意的是, 通过DHCP服务器获取到的IP是有时效性的. 到达一定的时长后会被自动回收或者续租.

# 2. DHCP服务器

## 2.1 安装

最常用的DHCP服务器为`isc-dhcp-serve`.执行命令进行安装

## 2.2 配置





