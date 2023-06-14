---
aliases: []
area: 计算机网络
project: 
date: 2023-06-14 21:10
tags: []
---
---
#### Description
##### 三次
![[Pasted image 20230614211814.png]]
###### 如何绕过三次握手?
- 在 Linux 3.7 内核版本之后，提供了 `TCP Fast Open` 功能，这个功能可以减少 TCP 连接建立的时延
- 客户端发送 SYN 报文，该报文包含「数据」（对于非 TFO 的普通 TCP 握手过程，SYN 报文中不包含「数据」）以及此前记录的 Cookie
- 支持 `TCP Fast Open` 的服务器会对收到 Cookie 进行校验：如果 Cookie 有效，服务器将在 SYN-ACK 报文中对 SYN 和「数据」进行确认，服务器随后将「数据」递送至相应的应用程序；如果 Cookie 无效，服务器将丢弃 SYN 报文中包含的「数据」，且其随后发出的 SYN-ACK 报文将只确认 SYN 的对应序列号
- 如果服务器接受了 SYN 报文中的「数据」，服务器可在握手完成之前发送「数据」，这就减少了握手带来的 1 个 RTT 的时间消耗
##### 如何提升四次挥手的性能?






---
#### Source
- [4.5 如何优化 TCP? | 小林coding](https://xiaolincoding.com/network/3_tcp/tcp_optimize.html#%E5%A6%82%E4%BD%95%E7%BB%95%E8%BF%87%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B)
