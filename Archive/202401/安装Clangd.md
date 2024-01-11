---
aliases: []
date: 2024-01-10 10:16
---
1. 下载对应版本的 clangd<br>
    - [Linux](https://github.com/clangd/clangd/releases/download/17.0.3/clangd-linux-17.0.3.zip)
    - [Mac](https://github.com/clangd/clangd/releases/download/17.0.3/clangd-mac-17.0.3.zip)
    - [Windows](https://github.com/clangd/clangd/releases/download/17.0.3/clangd-windows-17.0.3.zip)
    - [阿里云盘(Linux/Mac/Windows)](https://www.alipan.com/drive/file/backup/659672377c087d3e4a8e43979fdd74515da5c23e)
    
1. 解压并配置环境
```bash
# 解压文件
unzip clangd_[].zip -d clangd
# 将clang移动到防止软件的文件夹
sudo mv clangd /path/clangd
# 将clangd软链接到环境搜索路径
sudo ln -s /path/clangd/bin/clangd /usr/local/bin/clangd
```

