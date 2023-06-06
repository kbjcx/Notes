---
aliases: []
area: C++
project: 
date: 2023-06-01 20:02
tags: []
---
---
#### Description
`fwrite` 底层调用的还是 `write`, 但是 `fwrite` 会根据写入的内容大小来判断是否加入缓存, 当缓存达到一定的大小之后才会写入文件. 因此在频繁写入小数据时, 使用 `fwrite` 能一定程度上避免频繁的 IO 带来的性能损失. 但是当写入数据较大时, 直接使用 `write` 的效率更高.

---
#### Source
- [Site Unreachable](https://www.cnblogs.com/starRebel/p/14601173.html)