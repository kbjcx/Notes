# 协议设计
```cpp
+------+-------+------+------+------+------+------+
| MAGIC|VERSION| TYPE |      SEQUENCE ID          |
+------+-------+------+------+------+------+------+
|      CONTENT LENGTH        |  CONTENT ...
+------+-------+------+------+------+------+------+
```
- 私有通信协议: 
    1. (1 字节) `MAGIC`: 魔法数 (默认值 `0xcc`)
    2. (1 字节) `VERSION`: 版本号, 以便对协议进行扩展 (默认值 `0x01`)
    3. (1 字节) `TYPE`: 请求类型, 如心跳包, RPC 请求等
    4. (4 字节) `SEQUENCE ID`: 序列号
    5. (4 字节) `CONTENT LENGTH`: 消息长度
    6. (CONTENT LENGTH) `CONTENT`: 要接收的消息, 长度由 `CONTENT LENGTH` 规定

- 请求类型定义: 
    ```cpp
    enum class MessageType : uint8_t {
        HEARTBEAT_PACKET,  // 心跳包
        RPC_PROVIDER,      // 向服务中心声明为provider
        RPC_COMSUMER,      // 向服务中心生命为comsumer
    
        RPC_REQUEST,   // 通用请求
        RPC_RESPONSE,  // 通用响应
    
        RPC_METHOD_REQUEST,   // 请求方法调用
        RPC_METHOD_RESPONSE,  // 响应方法调用
    
        RPC_SERVICE_REGISTER,  // 向中心注册服务
        RPC_SERVICE_REGISTER_RESPONSE,
    
        RPC_SERVICE_DISCOVER,  // 向中心请求服务发现
        RPC_SERVICE_DISCOVER_RESPONSE,
    
        RPC_SUBSCRIBE_REQUEST,  // 订阅
        RPC_SUBSCRIBE_RESPONSE,
    
        RPC_PUBLISH_REQUEST,  // 发布
        RPC_PUBLISH_RESPONSE
    };
    ```

# RpcServer
- RpcServer 是服务提供方, 其预先注册支持的函数调用服务, 其本身可以接受客户端的连接, 支持直接调用, 也支持向服务中心注册函数服务, 由服务中心分配客户端服务
- 在接受客户端连接时, 会执行 `handle_client` 的回调函数, 根据客户端请求报文的类型进行处理, 支持心跳包, 函数调用以及订阅请求, 其余请求为非法请求, 不予处理.
- 对于服务中心, 采取定时发送心跳包的方式确认对方存活; 对于客户端, 定时采取清除策略, 清除断开连接的客户端, 并响应心跳包进行保活, 若是超时没收到心跳包, 则关闭该客户端的连接
- 心跳定时器的发送间隔为 30 秒, 定时器的检测断连的间隔为 40 秒, 预留了 10 秒的包延迟, 重发等带来的延迟
## 支持的服务
1. 函数调用请求: RPC_METHOD_REQUEST
2. 订阅请求: RPC_SUBSCRIBE_REQUEST
3. 心跳包请求: HEARTBEAT_PACKET, 接收客户端的心跳包
# 需要的服务
1. 服务发布: RPC_PUBLISH_REQUEST, 向客户端发布服务变更消息
2. 服务提供方请求: RPC_PROVIDER, 向服务中心声明为服务提供方
3. 心跳包: HEARTBEAT_PACKET, 向服务中心发送心跳包
4. 服务注册: RPC_SERVICE_REGISTER, 向服务中心注册服务

