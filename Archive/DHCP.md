---
aliases: []
area: 计算机网络
project: 
date: 2023-06-16 11:16
tags: []
---
---
#### Description
- 客户端首先发起 DHCP 发现报文（DHCP DISCOVER） 的 IP 数据报，由于客户端没有 IP 地址，也不知道 DHCP 服务器的地址，所以使用的是 UDP 广播通信，其使用的广播目的地址是 255.255.255.255（端口 67） 并且使用 0.0.0.0（端口 68） 作为源 IP 地址。DHCP 客户端将该 IP 数据报传递给链路层，链路层然后将帧广播到所有的网络中设备
- DHCP 服务器收到 DHCP 发现报文时，用 DHCP 提供报文（DHCP OFFER） 向客户端做出响应。该报文仍然使用 IP 广播地址 255.255.255.255，该报文信息携带服务器提供可租约的 IP 地址、子网掩码、默认网关、DNS 服务器以及 IP 地址租用期
- 客户端收到一个或多个服务器的 DHCP 提供报文后，从中选择一个服务器，并向选中的服务器发送 DHCP 请求报文（DHCP REQUEST 进行响应，回显配置的参数
- 最后，服务端用 DHCP ACK 报文对 DHCP 请求报文进行响应，应答所要求的参数

- 如果租约的 DHCP IP 地址快期后，客户端会向服务器发送 DHCP 请求报文：
    - 服务器如果同意继续租用，则用 DHCP ACK 报文进行应答，客户端就会延长租期。
    - 服务器如果不同意继续租用，则用 DHCP NACK 报文，客户端就要停止使用租约的 IP 地址

-  DHCP 中继代理. 有了 DHCP 中继代理以后，对不同网段的 IP 地址分配也可以由一个 DHCP 服务器统一进行管理 

---
#### Source
- [Fetching Title#8o0x](https://xiaolincoding.com/network/4_ip/ip_base.html#dhcp)