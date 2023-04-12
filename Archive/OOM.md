---
aliases: [Out of Memory]
area: 操作系统
project: 
date: 2023-04-12 16:21
---
---
#### Description
OOM Killer 机制会根据算法选择一个物理内存占用较高的进程, 然后将其杀死, 释放其内存资源, 如果物理内存依然不足, 那么 OOM 会继续杀掉内存, 直到释放出足够的内存位置. 

---
#### Source
