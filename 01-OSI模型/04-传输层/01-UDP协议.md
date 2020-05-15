# 1. UDP协议
## 1.1 介绍
UDP是用户数据包协议(User Datagram Protocol)
> 数据报:是通过网络传输的数据的基本单元

## 1.2 特点
* 不稳定, 不能保证对方能准确接受到.
* 相对tcp而言UDP要快
* 能发送广播

## 1.3 报文信息
* **发送字节控制**
局域网: 1518(以太网帧) - 16(mac地址) - 帧类型(2) - IP数据报(20) - UDP头部信息(8) = 1472字节
Internet: 标准MTU值(576) - IP数据报(20) - UDP头部信息(8) = 548字节
UDP发送的字节上限: 65535 - 20(IP头部) - 8(UDP头部) = 65507字节 
*注: 超过上述长度时, 会信息分片传输, 到达目的地后重新组合, 如果中途有丢失情况, 则会抛弃全部数据, 所以包越长越容易丢包.*

# 2. 发送UDP
## 2.1 socket
套接字(socket), 是计算机之间进行通讯的接口. 
*注: 接收数据没有限制上限, 但是一定要大于客户端发送的数据长度, 否则会抛出和发送超限一样的异常的*
## 2.2 UDP服务端
用于接收UDP客户端请求.
```python
# -*- coding:utf-8 -*-
# author:HPCM
# datetime:2020/5/15 9:53
import socket

ip_port = "19.19.19.52", 10010
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.bind(ip_port)
while True:
    msg, client_ip_port = sock.recvfrom(500 * 1024)
    print(len(msg))
    print(msg.decode(), client_ip_port)
sock.close()
```
## 2.3 UDP客户端
用于发送UDP报文的
```python
# -*- coding:utf-8 -*-
# author:HPCM
# datetime:2020/5/15 9:49
import socket

ip_port = "111.230.227.23", 8002
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.sendto("1你好xx".encode(), ip_port)
sock.close()
```
## 2.4 广播
向一个网段广发请求. 只需要指定ip为发送网段的广播IP, 或者直接使用`<broadcast>`
```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
ip_port = "11.11.11.255", 11111
sock.sendto("message 测试!".encode(), ip_port)
```




