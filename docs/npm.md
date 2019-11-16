# npm

##npx

```javascript
// node-modules/.bin/koa --version
npx koa --version
```
看服务器日志（pm2）：
```javascript
// 日志
sudo -iu sankuai
cd /opt/meituan/keeper
npx pm2 log
```
npx 的原理很简单，就是运行的时候，会到node_modules/.bin路径和环境变量$PATH里面，检查命令是否存在。

由于 npx 会检查环境变量$PATH，所以系统命令也可以调用。

```javascript
// 等同于 ls
$ npx ls
```

##gitbook


```javascript
npm install -g gitbook-cli
mkdir es6
cd es6
gitbook init
把下载的项目文件复制来
gitbook serve   // 指定端口：gitbook serve --port 2333
gitbook serve --lrport 35730 --port 4001  //启动多个电子书

执行 gitbook serve，获取服务的端口号：
Live reload server started on port: 35729
执行 lsof -i:35729，查看端口的进程信息：
COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME
node 2771 dd01 23u IPv6 0x7953faeb24d2c63d 0t0 TCP *:35729 (LISTEN)
我们可以获取到 PID 就是 2771
执行 kill -9 2771，可以关闭这个进程

pm2 start gitbook --name es6 -- serve --lrport 35288 --port 4001

pm2 startup
pm2 save
```

build 命令可以指定路径：
```javascript
// gitbook build
gitbook build [书籍路径] [输出路径]
```
还可以生成 PDF 格式的电子书：
```javascript
gitbook pdf ./ ./mybook.pdf
```