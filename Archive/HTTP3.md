---
aliases: []
area: 计算机网络
project: 
date: 2023-06-13 16:44
tags: []
---
---
#### Description
- HTTP/2 队头阻塞的问题是因为 TCP，所以 HTTP/3 把 HTTP 下层的 TCP 协议改成了 UDP
- 但是 UDP 是不可靠的传输协议，因此需要在它的上层实现可靠的传输
- 基于 UDP 的 **<font color="#0593A2">QUIC</font>** 协议可以实现类似 TCP 的可靠性传输
##### 优点
1. 无队头堵塞
当某个 stream 发生丢包后只会堵塞这一个 stream
1. 更快的连接建立
基于连接 ID 可以实现更快的连接建立，QUIC 内部包含了 TLS1.3，因此不用和 TCP 一样先建立 TCP 连接在进行 TLS 握手
1. 链接迁移
通过连接 ID 来标记通信的两个端点，可以无缝复用连接，消除重连的成本
---
#### Source
- [3.1 HTTP 常见面试题 | 小林coding](https://xiaolincoding.com/network/2_http/http_interview.html#http-3-%E5%81%9A%E4%BA%86%E5%93%AA%E4%BA%9B%E4%BC%98%E5%8C%96)