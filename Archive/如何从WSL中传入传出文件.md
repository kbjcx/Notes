wsl 中会将 windows 中的文件挂载在 /mnt 文件夹下，因此可以通过直接访问 /mnt 文件夹的方式直接传递文件
```bash
# 查看windows所有的挂载磁盘
sudo ls /mnt/*
```