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

node.js搭建

```shell
https://nodejs.org/

npm-Node Package Manager
换源

npm install cnpm -g --registry=https://registry.npm.taobao.org

```

nodejs一般操作

```shell 
安装
npm install xxx
npm i xxx

删除
npm uninstall xxx
npm un xxx

cnpm i xxx
cnpm un xx

```

卸载低版本
	1.卸载node本身、删除nodejs目录
	2.手动删除C:\Program Files\nodejs\node_modules\
	3.手动删除C:\users\你\node_modules\

运行nodejs程序：
1.目录
  cd
  x:

2.运行
  node x.js



nodejs中的包：

```shell
1.安装
  cnpm i xxx

2.引入
  const multer=require('multer');

3.用
```

node.js服务器

```shell
npm -> cnpm
npm install xxx
const xx=require('xxx');
```

http 包

```javascript
//引入http包
const http=require('http')
//直接创建http服务；里面参数为一个回调函数；为请求后返回的函数

let server=http.createServer(()=>{
    console.log('my request')
    
});
//监听-即等待连接；给一个端口;不给端口接受不到
server.listen(8080)

```

监听：等待客户端的连接
端口：数字

server.js

```javascript
const http=require('http');

let server=http.creatServer(function(req,res){
    res.write('sending')
    res.end()
})
server.listen(8080)
```

+ response.write()
+ response.end()

fs.js-系统自带的用来操作文件的

```javascript
const fs=require('fs')
//写入
fs.writeFile('./a.txt','ashuvg',err=>{
    if(err){
        console.log('fail')
    }else{
        console.log('succeed')
    }
}
//读文件
fs.readFile('./a.txt',(err,data)=>{
    if(err){
        cosole.log('fail'，err)
    }else{
        console.log('succeed',data)
       //出现buffer数据——原始的二进制数据，缓冲区 
       //node不仅会处理文本数据还会处理其他类型的数据，按照二进制进行保存，不能随便转格式，否则对数据的完整性造成损害
       //若均为文本数据，可以直接toString()转成字符串进行显示
        //console.log('succeed',data.toString)
    }
})            
```

+ fs.writeFile(path,data,callback)
+ fs.readFile(path,callback)

均为异步操作

同步版本： fs.writeFileSync/readFileSnyc

server2.js

```javascript
const http=require('http')
const fs=require('fs')

let server=http.createServer(function(req,res){
    //req.url=>'./1.html'=>'www/1.html'
    fs.readFile('www${req.url}',(err,buffer){
                if(err){
        res.writeHeader(404)//对机器
        res.write('Not Found')//对人
        res.end()
    }else{
        res.write(buffer)
        res.end
    }
    })
})
server.listen(8080)
```

+ http 
+ fs

服务器：

1.响应请求

2.数据交互

3.数据库



### 数据交互

#### HTTP协议

浏览器与服务器的交互规则

1. 发展历史：

HTTP1.0  RFC-1945;规定通信规则

HTTP1.1 RFC-2616：持久连接

HTTPS     RFC-2818: 安全协议 非对称加密与对称加密结合

HTTP2.0 RFC-7450: 加密 、头部压缩、 服务器推送、 管线操作 、多路复用

2. HTTP报文结构：

   header报文头:信息	 <=32K 

   body报文体:数据  	<=2G

3. 状态码

   1xx 信息

   2xx 成功

   3xx 重定向

   4xx 请求错误

   5xx 服务器错误

4. 请求方式

   | GET  | 获取     | 数据放在URL里面传输 ，header里 | 容量小<32K |
   | ---- | -------- | ------------------------------ | ---------- |
   | POST | 发送数据 | 数据放在body里，也可放在URL里  | 容量大     |

接收浏览器的GET数据

