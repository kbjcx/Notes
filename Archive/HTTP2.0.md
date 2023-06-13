---
aliases: []
area: 计算机网络
project: 
date: 2023-06-13 16:23
tags: []
---
---
#### Description
##### 优点
1. 头部压缩
如果同时发出的多个请求，他们的头部是相似的，那么会对头部字段进行压缩，用服务端维护的静态表、动态表来实现对常用头部字段和特殊字段的压缩。
1. 二进制格式
HTTP2 全面采用二进制方式传输头部和数据体，增加了传输的效率
1. 并发传输
HTTP2 通过 stream 来解决了 HTTP1.1 的 [[HTTP1.1#^819602|队头堵塞]]的问题，1 个 TCP 连接可以包含多个 stream，一个 stream 可以包含一个或多个请求，但是一个请求必须在同一个 stream 里
同时，stream 传输可以是无序的，可以通过 stream ID 来组合 stream，但是 stream 内的消要保持有序。
还可以对每个 stream 设置不同的优先级
1. 服务器主动推送资源
客户端和服务器双方都可以建立 Stream， Stream ID 也是有区别的，客户端建立的 Stream 必须是奇数号，而服务器建立的 Stream 必须是偶数号
服务端可以主动推送客户端可能需要的一些资源

---
#### Source
- [3.1 HTTP 常见面试题 | 小林coding](https://xiaolincoding.com/network/2_http/http_interview.html#http-2-%E5%81%9A%E4%BA%86%E4%BB%80%E4%B9%88%E4%BC%98%E5%8C%96)