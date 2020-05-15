# 1. TCP协议
## 1.1 介绍
TCP为传输控制协议(Transmission Control Protocol)

## 1.2 特点
* 相对稳定, 确保对方准确都到数据.
* 相对UDP而言TCP要慢
* web服务器都是使用tcp创建的
* 不能发送广播
## 1.3 报文信息
* **发送的字节控制**
局域网: 1518(以太网帧) - 16(mac地址) - 帧类型(2) - IP数据报(20) - TCP头部信息(20) = 1460字节
Internet: 标准MTU值(576) - IP数据报(20) - UDP头部信息(20) = 536字节
事实上tcp发送是没有上限控制的, 如果数据较大的话, 则会底层会自动分片发送. 数据越大标志着底层需要处理的时间越长, 可能会引起超时异常
## 1.3 握手

# 2. 发送TCP
## 2.1 TCP服务端
接受客户端请求信息
```python
# -*- coding:utf-8 -*-
# author:HPCM
# datetime:2020/5/15 15:37
import socket


ip_port = "0.0.0.0", 1050
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 端口复用
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
# 绑定ip及端口
sock.bind(ip_port)
# 配置连接上限
sock.listen(128)
print("服务已启动: {}".format(ip_port))
while True:
    client_sock, client_ip_port = sock.accept()
    print("用户 {} 已接入!".format(client_ip_port))
    msg = client_sock.recv(1024)
    print(msg.decode())
    if msg == b"q":
        break
    client_sock.send(msg)
    client_sock.close()
sock.close()
```
## 2.2 TCP客户端
发往服务端客户信息
```python
# -*- coding:utf-8 -*-
# author:HPCM
# datetime:2020/5/15 15:43
import socket

ip_port = "19.19.19.52", 1050
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(ip_port)
sock.send("11你好xx".encode())
sock.close()
```
