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
> [!important]  Important：只有当强制缓存没有命中时才会发起协商缓存
##### 强制缓存
- 强制缓存指的是当浏览器判断缓存没有过期的话就直接使用本地缓存
- 强制缓存通过相应头部的 `Cache-Control` 和 `Expires` 字段实现的
- 如果 HTTP 响应头部同时有 `Cache-Control` 和 `Expires` 字段的话，`Cache-Control` 的优先级高于 `Expires`
- 如果再次请求该资源，浏览器通过请求时间与这两个字段设置的过期时间比较，如果没有过期则直接使用

##### 协商缓存
- 协商缓存就是在请求发送之后，由服务器告知资源没有更改，可以使用本地缓存
- 有两种实现方式：
    1. `Last-Modified` 和 `If-Modified-Since`
        响应头中的 `Last-Modified` 字段表明了资源的最后修改时间，客户端请求时会在 `If-Modified-Since` 携带上这个时间，由服务器判断资源是否过期
    1. 请求头中的 `If-None_Match` 和响应头中的 `Etag`
        由响应头中的 `Etag` 标识来唯一识别服务器端的资源，一般采取 Hash 算法得到
        发起请求时会将 `If-None_Match` 字段的内容与服务器端的对比，如果相同则资源没有发生变化

> [!important] 
> Etag 的优先级高于 Last-Modified，因为它能解决以下几点问题：
> 1. 没有修改文件的情况下文档的修改时间也可能发生变化，导致重新请求
> 2. 服务器可能在秒级以内对资源进行修改，基于时间的 Last-Modified 没有那么高的细粒度
> 3. 防止服务器时钟改变带来的影响
> 4. 防止服务器无法获取到准确的文件修改时间


---
#### Source
- [3.1 HTTP 常见面试题 | 小林coding](https://xiaolincoding.com/network/2_http/http_interview.html#%E4%BB%80%E4%B9%88%E6%98%AF%E5%8D%8F%E5%95%86%E7%BC%93%E5%AD%98)