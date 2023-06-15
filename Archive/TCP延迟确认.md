---
aliases: [延迟确认]
area: 计算机网络
project: 
date: 2023-06-15 17:05
tags: []
---
---
#### Description
- 当发送没有携带数据的 ACK，它的网络效率也是很低的，因为它也有 40 个字节的 IP 头和 TCP 头，但却没有携带数据报文。为了解决 ACK 传输效率低问题，所以就衍生出了 TCP 延迟确认
- TCP 延迟确认策略:
    1. 当有响应数据时, ACK 会随着响应数据一起立刻发送给对方
    2. 当没有响应数据时, ACK 会延迟一段时间, 以等待是否有响应数据可以一起发送
    3. 如果超过了延迟确认的时间, 那么会立刻发送 ACK
    4. 如果在延迟等待发送 ACK 期间, 对方的第二个数据报文又到达了, 这时就会立刻发送 ACK



---
#### Source
- [4.22 TCP 四次挥手，可以变成三次吗？ | 小林coding](https://xiaolincoding.com/network/3_tcp/tcp_three_fin.html#%E5%AE%9E%E9%AA%8C%E9%AA%8C%E8%AF%81)