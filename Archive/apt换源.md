---
aliases: []
date: 2024-01-10 10:13
---
```bash
# 备份原文件
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
# 编辑文件，加入新源
vim /etc/apt/sources.list
# 更新系统源
sudo apt update
```