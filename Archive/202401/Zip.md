# 解压文件
```bash
# 不加-d默认解压到当前文件夹
unzip file.zip [-d dest_path]
```
# 压缩文件
```bash
#  |压缩文件夹|Password |dest filename|  来源文件（可以多个） |
zip    -r     -P 123456   Test_2.zip    Test1.txt Test2.txt
```