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
- 当全连接队列满了之后, linux 按照 `tcp_abort_on_overflow` 参数来



---
#### Source
