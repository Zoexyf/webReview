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

1. #####发展历史：

HTTP1.0  RFC-1945;规定通信规则

HTTP1.1 RFC-2616：持久连接

HTTPS     RFC-2818: 安全协议 非对称加密与对称加密结合

HTTP2.0 RFC-7450: 加密 、头部压缩、 服务器推送、 管线操作 、多路复用

2. #####HTTP报文结构：

   header报文头:信息	 <=32K 

   body报文体:数据  	<=2G

3. #####状态码

   1xx 信息

   2xx 成功

   3xx 重定向

   4xx 请求错误

   5xx 服务器错误

4. #####请求方式get与post

   | GET  | 获取     | 数据放在URL里面传输 ，header里 | 容量小<32K |
   | ---- | -------- | ------------------------------ | ---------- |
   | POST | 发送数据 | 数据放在body里，也可放在URL里  | 容量大     |

GET数据一般一次性会传输；POST数据不行，因为体积过大，需要分几次传输，切成段的大小由具体环境决定

POST数据是异步的

接收浏览器的GET数据；

```javascript
const http=require('http')
const querystring=require('querystring')
//用来切割传参的&与=；直接引入模块就行

let server=http.creatServer(function(req,res){
    let [url,query]=req.url.split('?');
    //表单所提交的内容会传到URL里面，所以只需要解析URL内容
    let get=querystring.parse(query)
    //将字符串转化为需要的格式JSON格式，get就得到了表单提交的信息；
    
})
server.listen(8080)

//2.使用URL直接处理数据
const http=require('http')
const url=require('url')

let server=http.creatServer(function(req,res){
    let result=url.parse(req.url,true)
    //得到URL解析后的一串数据；加true会解析得到需要的JSON数据,即连query也进行了切割
    let {pathname,query}=url.parse(req.url,true)
})


```

表单数据GET

```html
<form action="http://localhost:8080/aaa" method="get">
      用户：<input type="text" name="username" /><br>
      密码：<input type="password" name="password" /><br>
      <input type="submit" value="提交" />
</form>
```

接收浏览器的POST数据

```javascript
const http=require('http')
const url=require('url')

let server=http.creatServer(function(req,res){
    let arr=[]
    req.on('data'，buffer=>{
           arr.push(buffer)
           })
    //类似前台的事件;data 表示有数据过来，发生的次数取决于对方的数据到底有多大，data给的buffer参数，
	//需要对buffer这种二进制的文件进行拼接；准备一个数组，每当有数据来的时候，将数据存储起来
	//等到end的时候将存的一组的buffer对象通过自身属性concat进行连接
    
    req.on('end',()=>{
        let buffer=Buffer.concat(arr)
        console.log(buffer.toString())
    })
})
server.listen(8080)

//当文件仅为字符串时候（无图片音频等转格式有影响的数据时）
// 引入模块 let querystring=require('querystring')
// 在end事件中添加 let post=querystring.parse(buffer.toString())

```



总结：

接收浏览器的GET数据：

+ url模块
  + url.parse(req.url,true)=>{ pathname,query}
+ GET=>"/aaa/b?xx=xx&xxx=xxx"

接收浏览器的POST数据

+ body内；分几次传输

  ```javascript
  req.on('data',buffer=>{
  arr.push(buffer)
  })
  
  req.on('end',()=>{
  buffer=Buffer.concat(arr)
  })
  ```

+ POST=> "xx=xxx&xxx=xx"
+ querystring.parse('xx')

get post 服务器整合

```javascript
const http=require('http')
const url=require('url')
const querystring=require('querystring')
const fs=require('fs')
let path='', get={}, post={};

let server=http.creatServer((req,res)=>{
    if(req.method=='GET'){
        let {pathname,query}=url.parse(req.url,true)
         path=pathname
         get=query
         complete();
    }else if(req.method=='POST'){
        path=req.url;
        let arr=[]
        req.on('data',buffer=>{
            arr.push(buffer)
        })
        req.on('end',()=>{
            let buffer=Buffer.concat(arr)
            post=querystring.parse(buffer.toString())
        })
         complete();
        
    }
    function complete(){
    console.log(path, get, post);
  }
}).listen(8080)
```

eg：登录注册接口

```javascript
const http=require('http')
const url=require('url')
const querystring=require('querystring')
const fs=require('fs')
let path='', get={}, post={};
let users={}

let server=http.creatServer((req,res)=>{
    if(req.method=='GET'){
        let {pathname,query}=url.parse(req.url,true)
         path=pathname
         get=query
         complete();
    }else if(req.method=='POST'){
        path=req.url;
        let arr=[]
        req.on('data',buffer=>{
            arr.push(buffer)
        })
        req.on('end',()=>{
            let buffer=Buffer.concat(arr)
            post=querystring.parse(buffer.toString())
        })
         complete();
        
    }
    function complete(){
        //注册
    if(path=='/reg'){
        let {username,password}=get;
        
        if(users[username]){
            res.write(JSON.stringfy({error:1,msg:'此用户已存在'}))
            res.end()
        }else{
            users[username]=password
            
            res.write(JSON.stringfy({error:0,msg:''}))
            res.end();
            
        }
        //登录
    }else if(path=='/login'){
        let {username,password}=get;
        if(!users[username]){
            res.wirte(JSON.stringfy({error:1,msg:'找不到此用户'}))
            res.end()
        }else{
            if(users[username]!=password){
                res.write(JSON.stringfy({error:1,msg:'用户密码错误'}))
                res.end()
            }else{
                res.write(JSON.stringfy({error:0,msg:'登录成功'}))
            }
        }
    }else{
        fs.readFile(`www${path}`,(err,buffer)=>{
            if(err){
                res.writeHeader(404)
                res.write('Not Found');
      		    res.end();
            }else{
                res.write(buffer)
                res.end()
            }
        })
    }  
  }
}).listen(8080)
```

注册接口

/reg?username=xxx&password=xxx

=>{error:1,msg:'why'}

登录接口

/login?username=xxx&password=xxx

=>{error:1}

eg：实际操作一个注册登录界面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>登录注册页面</title>
</head>
<script src='jquery.js' charset="utf-8"></script>
<script>
    $(function(){
        $('#btn1').click(()=>{
            $.ajax({
                url:'/reg',
                data:{
                    username:$('#user').val(),
                    password:$('#password').val()
                },
                dataType:'json',
            }).then(json=>{
                if(json.error){
                    alert(json.msg)
                }else{
                    alert("注册成功")
                }
            },err=>{
                alert("注册失败")
            }
            )
        })

        $('#btn2').click(()=>{
            $.ajax({
                url:'login',
                data:{
                    username:$('#user').val(),
                    password:$('#password').val()
                },
                dataType:'json'
            }).then(()=>{
                if(json.error){
                    alert(json.mag)

                }else{
                    alert("登录成功")
                }
            },
            err=>{
                alert("登录失败")
            })
        })

    })
</script>
<style>
    input{
        height: 30px;
        margin:5px;
    }

</style>
<body>
    用户：<input type="text" id="user" /><br>
    密码：<input type="text" id="password" /><br>
    <input type="button" value="注册" id="btn1">
    <input type="button" value="登录" id="btn2">
</body>
</html>
```

node运行上面的server代码

服务器：请求文件——结果 ； 请求接口——操作

完整服务器：1.处理文件2.处理接口3.存储数据



### node模块系统

模块  ： 系统模块 + 第三方模块

eg： CMD   AMD   ESM等 

Node.js的模块系统
1.定义模块

+  module    批量导出
+   exports   一个一个导出
+   require 
  +   如果带有路径——去路径下面找
  +   如果没有：
        node_modules文件夹
        系统node_modules

eg：mod.js ——将它放置在node_modules文件夹中

```javascript
exports.a=12; //单个可导出，即对外暴露
exports.b=5;

//批量对外暴露
module.exports={
  a: 12, b: 5
};

//封装成函数对外暴露
module.exports=function (){
  console.log('aaa');
};

//封装成类对外暴露
module.exports=class {
  constructor(name){
    this.name=name;
  }

  show(){
    console.log(this.name);
  }
};

//因为没有对外暴露，所以对外为undifined
let c=33;

```

1.js

```javascript
//引入
/*const mod1=require('mod1');

console.log(mod1.a);
console.log(mod1.b);
console.log(mod1.c);*/


const mod1=require('mod1');

//函数直接使用
//mod1();

//类的使用
let p=new mod1(99);

p.show();

```

