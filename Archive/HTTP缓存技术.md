---
aliases: []
area: 计算机网络
project: 
date: 2023-06-13 15:45
tags: []
---
---
#### Description
- 避免发送大量的重复请求，来提高 HTTP 的性能
- HTTP 缓存的实现方式有两种：**<font color="#0593A2">强制缓存</font>**和**<font color="#0593A2">协商缓存</font>**
##### 强制缓存
- 强制缓存指的是当浏览器判断缓存没有过期的话就直接使用本地缓存
- 强制缓存通过相应头部的 `Cache-Control` 和 `Expires` 字段实现的
- 如果 HTTP 响应头部同时有 `Cache-Control` 和 `Expires` 字段的话，`Cache-Control` 的优先级高于 `Expires`
- 如果再次请求该资源，浏览器通过请求时间与这两个字段设置的


---
#### Source
