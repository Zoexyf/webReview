## node.js

### node用途

+ node.js---服务器

+ node 不会独立开发

用途：
1.中间层

一般：客户端（web client）——服务端（java）

问题：主服务器暴露——安全隐患；数据服务庞大，性能不够好

node解决的问题：

+ 安全性 ；不会将主服务器暴露在外

+ 性能  ；主服务器一般需要假设庞大的安全系统，或其他业务数据的需求，性能较差

+ 降低主服务器复杂度；中间层可以对数据进行缓存，可以对数据进行汇总，以及其他功能

2.小型服务

3.工具

webpack，gulp等

中间层: 一种服务器，用来分散主服务器的数据，

中间件：库；

node.js优势：
1.便于前端入手
2.性能高；运算方面
3.利于和前端代码整合

------

node.js搭建
https://nodejs.org/

npm-Node Package Manager
换源

npm install cnpm -g --registry=https://registry.npm.taobao.org

------

安装
npm install xxx
npm i xxx

删除
npm uninstall xxx
npm un xxx

cnpm i xxx
cnpm un xx

------

npm
cnpm

------

怎么卸载低版本
1.卸载node本身、删除nodejs目录
2.手动删除C:\Program Files\nodejs\node_modules\
3.手动删除C:\users\你\node_modules\

------

运行nodejs程序：
1.目录
  cd
  x:

2.运行
  node x.js

------

包：
1.安装
  cnpm i xxx

2.引入
  const multer=require('multer');

3.用

------

监听：等待客户端的连接
端口：数字

------

node.js服务器
npm -> cnpm
npm install xxx
const xx=require('xxx');

------

http 包

```javascript
//引入http包
const http=require('http')
//直接创建http服务；里面参数为一个回调函数；为请求
http.createServer(()=>{
    console.log('my request')
    
});

```

