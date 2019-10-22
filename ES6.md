-------

## ES6

ECMAScript

常用ES2015(ES6.0)

最新2019(ES10)

ES6的标准使用起来更加方便有利于前端的工程性

方便-->工程性

### 变量

var：

1. 可以重复声明
2. 没有块级作用域
3. 不能限制

let   声明变量

const 声明常量

1. 禁止重复声明

2. 控制修改

3. 支持块级作用域 

eg：三者区别

```JavaScript
//1 可以正常赋值	
	var a=12;

    var a=5;

    alert(a);
//2 无法重复赋值(a已经被声明过)    
   let a=12;

    let a=5;

    alert(a);
//3 无法给常量赋值
    const a=12;

    a=5;

    alert(a);

```

eg：let优势-->块级作用域方面

```javascript
//1 var声明i变量，此时i在内部执行完后对外暴露的是最后一次的计算结果 
window.onload=function (){
      var aBtn=document.getElementsByTagName('input');

      for(var i=0;i<aBtn.length;i++){
        aBtn[i].onclick=function (){
          alert(i);
        };
      }
    };
//2. let声明i保证每次作用域独立，保证i对外的正常需求表现
window.onload=function (){
      var aBtn=document.getElementsByTagName('input');

      for(let i=0;i<aBtn.length;i++){
        aBtn[i].onclick=function (){
          alert(i);
        };
      }
    };
//3. 闭包方法解决这种问题
window.onload=function (){
      var aBtn=document.getElementsByTagName('input');

      for(var i=0;i<aBtn.length;i++){
        aBtn[i].onclick=(function (i){
          return function (){
            alert(i);
          }
        })(i);
      }
    };
```

### 作用域

1. 传统
     函数级

2. ES6
     块级 {}

     外层作用域无法读取内层作用域的变量

```javascript
//1)ES6允许块级作用域的任意嵌套。外层作用域无法读取内层作用域的变量。

{{{{
	{let i = 6;}
	console.log(i);  // 报错
}}}}

//2)内层作用域可以定义外层作用域的同名变量。

{{{{
	let i = 5;
	{let i = 6;}
}}}}

//3)块级作用域的出现使得立即执行函数不再需要。

//立即执行函数：

(function() {
	var i = 5;
})();

//块级作用域：

{
	let i = 5;
}
```

### 解构赋值

现今的变量声明语法十分的直接：左边是一个变量名，右边可以是一个数组：[]的表达式或一个对象：{ } 的表达式，等等。解构赋值允许我们将右边的表达式看起来也像变量声明一般，然后在左边将值一一提取

1. 左右两边一样
2. 右边得是个东西

正常情况

let {a,b,c}={a: 12, b: 55, c: 99};

let [a,b,c]=[12,5,8];

错误情况

let {a,b,c}={12,5,8};（忽略了右边是个四不像）

主要从三个方面讲述：

1. 数组式的解构赋值

```JavaScript

// 1. 数组解构赋值
// 1.1 传统方法: 为数组中元素起一个 alias
   var value = [1,2,3,4,5];
   var el1 = value[0],el2 = value[1], el3 = [2];
// 1.2 es6 方法
    let value = [1,2,3,4,5];
    let [el1,el2,el3] = value; // 对应前三个元素有了 alias
// 1.3 两个变量交换值的时候 不再需要临时变量了
    [a,b] = [b,a] // 感觉和 python 的路线有点像
// 1.4 解构赋值的套欠
    let [el1,el2,[el3,el4]] =  [1, 2, [3, 4, 5]];
// 1.5 指定位置的赋值
	let [el1,,el3,,el5] = [1,2,3,4,5];
// 1.5.1 example
	var  [,firstName, lastName] = "Zoe Xu".match(/^(\w+) (\w+)$/);
// 1.6 指定默认值:需要注意的是默认值只会在对undefined值起作用(先检测左侧是否有赋值)下面值为默认值
	var [firstName = 'Zoe', lastName = 'Xu'] = [];
// 1.6.1 下面的值 是 null
	var [firstName = "Zoe", lastName = "Xu"] = [null, null];

// 1.7 rest 参数，休止符,tail 变量将会接收 数组的剩余元素(注意：tail 变量后面不能有其他变量了)
	var vlaue = [1,2,3,4,5];
	var [el1,el2,el3,...tail] = value
```

2. 对象式的解构赋值

```JavaScript
// 2. 对象的解构赋值
// 与数组解构赋值的方式几乎完全一样
// 可以这样理解： 数组是通过位置 序号来赋值的，plainObj 是通过键名来赋值的。（本质上数组的位置，序号也就是键名）
// 2.1 对象的解构赋值
    var person = {firstName: 'Zoe', lastName: 'Xu'};
    var {firstName, lastName} = person;
// 2.2 ES6允许变量名与对应的属性名不一致。这里相当于声明了name 和 lastName 两个变量
    var person = {firstName: 'Zoe', lastName: 'Xu'};
    var  {firstName: name,lastName} = person; 
	//  此时name->Zoe;lastName->Xu 
	
    // 2.3 深层套欠对象赋值, 
    //  此时，name 变量并没有声明

    var person = {name: {firstName: 'Zoe', lastName: 'Xu'}};
    var {name: {firstName, lastName}} = person;

    // 2.4.1 对象包裹数组
    var person = {dateOfBirth:[1,1,1980]};
    var {dataOfBirth:[day,month,year]} = person;
    // 2.4.2 array is around Object 

    var person = [{dateOfBirth: [1,1,1980]}];
    var [{dateOfBirth}] = person;

    // 2.5 obj deconstruction uses default value
    // note: 默认值使用的是 赋值符号不是 冒号
    var { firstName = "Zoe", lastName: userLastName = 'Xu'}= {};

   // 默认值同样也只会在对undefined值起作用，下面的例子中firstName和lastName也都将是null：

        var { firstName = "Zoe", lastName = "Xu" } = { firstName: null, lastName: null };

```

3. 函数中的解构赋值


```JavaScript
// 3.0 函数参数的解构赋值
// ES6中，函数的参数也支持解构赋值。这对于有复杂配置参数的函数十分有用。可以结合使用数组和对象的解构赋值。
// 3.1传统写法
    function findUser (userId, options) {
        if (options.includeProfile) {}
        if (options.includeHistory) {}
    }
    // 3.2 es6 写法(简洁)
    function findUser (userId, {includeProfile, includeHistory}) {
        if (includeHistory) {}
        if (includeProfile) {}
    }
    // supplement
    // note: 我们是对变量赋值 所以即使是对象，左边 一般也是只有键没有 value 的, 如果 对象 deconstruction 时候，value 处有值，那么 该键对应的变量是不会被声明的
    // 有了数组的解构赋值，多变量同时赋值的写法改变
    // s1.1.1 传统 
    var a = 1, b = 2, c =3;
    // s1.1.2 es6
    let [a,b,c] = [1,2,3];
    // 总结： 1. 可以同时声明多个变量 并且对多个变量进行赋值 2. 对于返回的数组或者 plainObj 可以直接一次性取值 3. 对于复杂的 对象包裹 取值也不需要不断的链式，直接字面量取值
```

### 函数

#### 箭头函数

function (参数){

}

(参数)=>{}

如果，有且仅有1个参数，()也可以省
如果，有且仅有1条语句-return，{}可以省
注意：a=>({a})

修复this

eg:

```javascript
//1
document.onclick=ev=>{
      alert(ev.clientX+','+ev.clientY);
    };
//2
let sum=(a,b)=>a+b;

    alert(sum(12, 88));
//3
/*function show(){
      return {a: 12, b: 5};
    }*/
 let show=()=>({a: 12, b: 5});
 console.log(show());
```



#### 参数展开

剩余参数——必须是最后一个
展开数组——就跟把数组的东西写在那儿一样

eg：

```javascript
	let arr=[12,5,8,99,27];
    //...arr
    //12,5,8,99,27

    function sum(a,b,c,d,e){
      return a+b+c+d+e;
    }

    //alert(sum(arr[0], arr[1], ...))
    //alert(sum(...arr));
    //alert(sum(12,5,8,99,27));
```

eg:

```javascript
	let arr1=[1,2,3];
    let arr2=[4,5,6];

    let arr=[...arr1, ...arr2];
    //let arr=[1,2,3, 4,5,6];
```

### 系统对象

#### Array

+ map      映射   1对1
+ forEach 遍历   循环一遍
+  filter      过滤
+ reduce   减少  多对1

eg：

```javascript
//1.map
let arr=[100, 98, 37, 28, 19, 96, 56, 67];
    /*let res=arr.map(function (item){
      if(item>=60){
        return true;
      }else{
        return false;
      }
    });*/
    let res=arr.map(item=>item>=60);
//此时能return本省为true
    console.log(arr, res);

//2.forEach
	let arr=[12, 5, 8, 99];
    arr.forEach((item, index)=>{
      //alert('第'+index+'个：'+item);
      alert(`第${index}个：${item}`);
        //字符串模板--反单引号
    });

//3.filter
	let arr=[12, 88, 19, 27, 82, 81, 100, 107];
    let arr2=arr.filter(item=>item%2==0);
    console.log(arr);
    console.log(arr2);
	//.filter(item=>item.loc==cur.loc&&item.price>60)

//4.reduce（求和，求平均数）
 	let arr=[12, 66, 81, 92];
    let res=arr.reduce(function (tmp, item, index){
      alert(`第${index}次，${tmp}+${item}`);
      return tmp+item;
    });	
 	let revg=arr.reduce((tmp, item, index)=>{
      if(index<arr.length-1){
        return tmp+item;
      }else{
        return (tmp+item)/arr.length;
      }
    });

```

#### String

+ 字符串模板

+ startsWith   endsWith

eg:

```javascript
	let url='http://www.bing.com/a';
    if(url.startsWith('http://') || url.startsWith('https://')){
      alert('是网址');
    }else{
      alert('不是');
    }
```

#### JSON

+ 标准写法`{"key":"value","zoe":121}`
+ JSON对象
  + JSON.stringify(json)  JSON--->字符串
  + JSON.parse(str)         字符串--->JSON

eg：

```javascript
//	JSON.stringify(json)
	let json={a: 12, b: 5, c: 'blue'};
    //不传输不加引号直接在js里面可以使用；传输的时候直接转成JSON的字符串
    let str=JSON.stringify(json);
    console.log(str);
//JSON.parse(str)
	let str='{"a":12,"b":5,"c":"blue"}';
    let json=JSON.parse(str);
	//解析字符串为JSON
    console.log(json);
```

### 异步处理（重要）

#### 异步操作

异步——多个操作可以一起进行，互不干扰
同步——操作一个个进行

eg:

```javascript
//ajax异步操作（注意使用的jQuery）
$.ajax({
  url: 'data/1.json',
  dataType: 'json',
  success(data1){
    $.ajax({
      url: 'data/2.json',
      dataType: 'json',
      success(data2){
        $.ajax({
          url: 'data/3.json',
          dataType: 'json',
          success(data3){
            console.log(data1, data2, data3);
          }
        });
      }
    });
  }
});
let data1=$.ajax('data/1.json');
let data2=$.ajax('data/2.json');
let data3=$.ajax('data/3.json');

//同步操作一条一条执行
```

#### Promise

Promise本质上是一个 对象,对异步函数的一个封装

```javascript
//1.普通的Promise
let p=new Promise(function(resolve,reject)){
    $.ajax({
        url:'data/1.json',
        dataType:'json',
        success(data){
		    resolve(data);
		},
        error(res){
            resolve(res)
        }
    })
})
p.then(function(data){
    alert('yes')
    console.log(data)
};
      function(res){
    alert('no')
    console.log(res)
})

//2.jQuery中封装了Promise异步函数-直接写
	let res=$.ajax({
        url:'data/1.json',
        dataType:'json',
        success(data){
            resolve(data)
        },
        error(res){
            reject(res)
        } 
    })
    console.log(res)
```

#### Promise.all

```javascript
Promise.all([
      $.ajax({url: 'data/1.json', dataType: 'json'}),
      $.ajax({url: 'data/2.json', dataType: 'json'}),
      $.ajax({url: 'data/3.json', dataType: 'json'}),
    ]).then((arr)=>{
      let [data1, data2, data3]=arr;
      console.log(data1, data2, data3);
    //then(([data1, data2, data3])=>{
    //console.log(data1, data2, data3);
    //}
    }, (res)=>{
      alert('错了');
    });

```

#### async/await

同步书写异步，本质是语法糖

```javascript
async function show(){
      let data1=await $.ajax({url: 'data/1.json', dataType: 'json'});
      let data2=await $.ajax({url: 'data/2.json', dataType: 'json'});
      let data3=await $.ajax({url: 'data/3.json', dataType: 'json'});
      console.log(data1, data2, data3);
    }
    show();

 async function show(){
      let data1=await $.ajax({url: 'data/1.json', dataType: 'json'});
      if(data1.a<10){
        let data2=await $.ajax({url: 'data/2.json', dataType: 'json'});
        alert('a');
      }else{
        let data3=await $.ajax({url: 'data/3.json', dataType: 'json'});
        alert('b');
      }
    }

    show();
```









