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



