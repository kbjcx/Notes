---
aliases: []
area: 计算机网络
project: 
date: 2023-06-13 15:30
tags: []
---
---
#### Description
##### 1XX
- 属于提示信息，表示目前是协议处理的中间状态，还需要后续的操作

##### 2XX
- 表示服务端成功处理了客户端的请求
- **<font color="#0593A2">[200 OK]</font>**: 表示一切正常，如果是非 `HEAD` 请求，一般会带有 body 数据
- **<font color="#0593A2">[204 No Content]</font>**: 表示请求成功，但是响应头里面没有 body 数据
- **<font color="#0593A2">[206 Partial Content]</font>**: 表示请求成功，但是响应体里面的数据不是完整的资源，一般应用于 Http **分块下载或者断点续传**

##### 3XX
- 这类状态码表示客户端请求的资源发生了变动，需要重定向再请求资源
- **<font color="#0593A2">[301 Move Permanently]</font>**: 表示永久重定向，请求的资源已不存在，需要使用新的 URL 进行访问
- **<font color="#0593A2">[302 Found]</font>** : 表示临时重定向，请求的资源还在，但暂时需要使用另一个 URL 来进行访问
- **<font color="#0593A2">[304 Not Modified]</font>**: 表示资源未更改，重定向已存在的缓存文件

##### 4XX
- 这类状态码表示客户端的报文有误。服务器无法处理
- **<font color="#0593A2">[403 Forbidden]</font>**: 表示服务器资源禁止访问
- **<font color="#0593A2">[404 Not Found]</font>**: 表示所请求的资源在服务器上未找到或不存在

##### 5XX
- 这类状态码表示请求报文正确，但服务器处理时内部发生了错误
- **<font color="#0593A2">[501 Not Implemented]</font>**: 表示服务端请求的功能还不支持
- **<font color="#0593A2">[502 Bad Gateway]</font>**: 表示服务器作为网关或代理时访问后端服务器发生错误
- **<font color="#0593A2">[503 Service Unavailable]</font>** : 表示服务器当前正忙，无法响应客户端的请求

---
#### Source
- [3.1 HTTP 常见面试题 | 小林coding](https://xiaolincoding.com/network/2_http/http_interview.html#http-%E5%B8%B8%E8%A7%81%E7%9A%84%E7%8A%B6%E6%80%81%E7%A0%81%E6%9C%89%E5%93%AA%E4%BA%9B)