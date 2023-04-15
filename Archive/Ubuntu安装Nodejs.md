---
aliases: [安装Node.js]
area: Ubuntu, Node.js
project: WebRTC
date: 2023-04-15 15:01
---
---
#### Description
1. 下载 node.js
    ```shell
    wget https://nodejs.org/dist/v18.16.0/node-v18.16.0-linux-x64.tar.xz
    ```
2. 解压文件
    ```shell
    # 解压
    tar -xvf node-v18.16.0-linux-x64.tar.xz
    # 更改文件夹名
    mv ./node-v18.16.0-linux-x64 ./node
    # 进入目录
    cd node
    
    # 查看当前目录
    pwd
    ```
3. 链接执行文件
    ```shell
    sudo ln -s /home/llz/webrtc/node/bin/npm /usr/local/bin/
    sudo ln -s /home/llz/webrtc/node/bin/node /usr/local/bin/
    # 查看版本
    npm -v
    node -v
    ```

---
#### Source
- [linux安装nodejs【详细教程】\_菜鸟fox的博客-CSDN博客](https://blog.csdn.net/lh155136/article/details/111194424)
- [2.2 nodejs安装和测试\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1284y1n7Ny/?p=10&spm_id_from=pageDriver&vd_source=50264a0106f7562a87cc7b7908fed99e)