# mac 终端常用命令

* 创建文件（夹）
```code
// 创建dir文件夹
mkdir dir
// 创建文件
touch index.js
```
* 删除文件（夹）

```code
// 删除空的dir文件夹,不可删除文件
rmdir dir
// 递归删除文件夹，可以删除文件
rm -rf dir

```
`rm -rf`的功能明显很强大，`r` 标识递归 ，`f` 标识强制，不提示

# 查询服务
```
// 查看端口 6800的服务
sudo lsof -i tcp:6800
// 启动服务
aria2c --conf-path="/Users/caiyi/.aria2/aria2.conf"
// 查看服务占用端口号
ps aux|grep aria2c
// 杀掉服务
kill -9 6800

```