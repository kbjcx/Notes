---
aliases: []
area: 计算机网络
project: 
date: 2023-06-13 17:01
tags: []
---
---
#### Description
> [!note] TCP 是面向连接的、可靠的、基于字节流的传输层通信协议
##### TCP 头格式
![[Pasted image 20230613170227.png|500]]
- **<font color="#0593A2">序列号</font>**: 解决网络包乱序问题
- **<font color="#0593A2">确认号</font>**: 解决丢包问题

##### TCP 三次握手
![[Pasted image 20230613173041.png|500]]

- 为什么需要三次握手？
    - 确认双方都具备收发能力
    - 同步双方的序列号
    - 阻止重复历史连接
    - 避免资源浪费


---
#### Source
