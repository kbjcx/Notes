---
aliases: []
area: 计算机网络
project: 
date: 2023-06-14 20:23
tags: []
---
---
#### Description
##### 全连接队列
- 全连接队列，也称 accept 队列
- 服务端收到第三次握手的 ACK 后，内核会把连接从半连接队列移除，然后创建新的完全的连接，并将其添加到 accept 队列，等待进程调用 accept 函数时把连接取出来
- 当全连接队列满了之后, linux 按照 `tcp_abort_on_overflow` 参数来实行策略:
    1. `0`: 如果全连接队列满了, 那么 server 会丢弃 client 发来的第三次握手的 ACK
    2. `1`: 如果全连接队列满了, 那么服务器会发送 RST 客户端, 表示断开此连接

- TCP 全连接队列的最大值取决于 `somaxconn` 和 `backlog` 之间的最小值，也就是`min (somaxconn, backlog)`
##### 半连接队列
- 半连接队列，也称 SYN 队列
- 服务端收到客户端发起的 SYN 请求后，内核会把该连接存储到半连接队列
- **<font color="#0593A2">半连接队列丢弃 SYN 连接的时机:</font>**
    1. 如果半连接队列满了，并且没有开启 `tcp_syncookies`，则会丢弃
    2. 若全连接队列满了，且没有重传 SYN+ACK 包的连接请求多于 1 个，则会丢弃
    3. 如果没有开启 `tcp_syncookies`，并且 `max_syn_backlog` 减去当前半连接队列长度小于 (`max_syn_backlog >> 2`)，则会丢弃

- 开启 syncookies 功能就可以在不使用 SYN 半连接队列的情况下成功建立连接
- 要想增大半连接队列，不能只单纯增大 `tcp_max_syn_backlog` 的值，还需一同增大 `somaxconn` 和 `backlog`，也就是增大 `accept` 队列。否则，只单纯增大 `tcp_max_syn_backlog` 是无效的


---
#### Source
