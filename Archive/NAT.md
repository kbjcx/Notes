---
aliases: [网络地址转换]
area: 计算机网络
project: 
date: 2023-06-16 11:37
tags: []
---
---
#### Description
> [!note] 
> 简单的来说 NAT 就是同个公司、家庭、教室内的主机对外部通信时，把私有 IP 地址转换成公有 IP 地址
> 可以把 IP 地址 + 端口号一起进行转换. 这样，就用一个全球 IP 地址就可以了，这种转换技术就叫网络地址与端口转换 NAPT

- 生成一个 NAPT 路由器的转换表，就可以正确地转换地址跟端口的组合，令客户端 A、B 能同时与服务器之间进行通信

##### NAT/NAPT 缺点
- 外部无法主动与 NAT 内部服务器建立连接，因为 NAPT 转换表还没有生成转换记录
- 转换表的生成与转换操作都会产生性能开销
- 通信过程中，如果 NAT 路由器重启了，所有的 TCP 连接都将被重置




---
#### Source
- [5.1 IP 基础知识全家桶 | 小林coding](https://xiaolincoding.com/network/4_ip/ip_base.html#nat)