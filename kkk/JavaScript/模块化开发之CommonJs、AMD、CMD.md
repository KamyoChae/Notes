## **CommonJs、AMD、CMD的比较**
首先我们来了解一下模块化

------------

我们经常这样子写代码

1. 函数 （函数模块化）
```javascript
funcion add(a,b){
    return a+b
}
console.log(add(45+45))`
```
*缺点：只能在某个作用域内使用*

1. 对象（对象模块化）
```javascript
var ppt = {
    len:3,
    init:function(){},
    createDom:function(){}
}
```
*优点：简单快速
弊端：外部可影响内部属性*

1. 匿名函数 
```javascript
var obj = (function(){
    var len = 5;
    function add(a,b){
        return a+b'
    }
    return {
        add:add;
    }
}())
console.log(obj.add(1,2))
```
*优点：变量私有化
缺点：要return*

1.  依赖
```javascript
(function (a,b){
    b(a)
})(window, function(window){
    window.jquery = jquery;
    function jquery (){}
})
```
*缺点：只能在该模块中不断添加方法*

1. 模块化开发
```javascript
(function (m){
m.add = function(){
}
})(window.module || {})
```
*优点：能对已有基础添加方法*



#### 可见，如果多个人共同开发某个项目，大家都在用自己喜欢的写法，那么整个项目的代码是非常糟糕的。为了实现js代码战国时代的大一统，彻底终结时代动乱，纵横家“苏秦”出现了，也就是我们现在说的“模块化开发规范”





------------


------------




## 模块化出现的时代背景
1. 最初js并没有模块化机制
2. 各种野生js到处飞 得不到妥善管理
3. 后来前端圈开始制定规范 出现了commonjs 
4. node就是基于commonjs规范的产物（commonjs同步加载 适用于服务器端）
5. 由于commonjs的同步加载更适用于服务器端 
6. 所以迫切需要客户端的规范出台
7. 旋即出现了很多耳熟能详的前端规范 如AMD

------------



### 一、CommonJs：
##### 特点： 一个萝卜一个坑，一个文件一个模块
一个js文件就是一个模块，内部变量属于该模块， 不会对外部暴露， 也就是不会污染全局变量

该规范最初用在node，前端的webpack对CommonJs原生支持

##### 核心思想 
通过require方法同步加载所需要依赖的其他模块
然后通过 exports或者module.exports来导出暴露的接口

如:

###### // index.js
```javascript

var module = require("module.js")

module.add(5) 

```
###### // module.js
```javascript

module.exports = {
    aad:function (n){
       return  n += 5
    }
}
```
如果你觉得只要照葫芦画瓢，把这段代码复制粘贴到项目里面，那就大错特错了哦

###### 浏览器不兼容Commonjs ，因为缺少 module exports require global环境变量。
###### 它需要工具转换才能成功运行

##### commonJs缺点：同步加载，适用服务器端，服务器读取加载时间快，但不适合移动端



------------













###二、AMD
若浏览器采用commonjs同步加载js文件，倘若文件过多，则会导致页面瘫痪
为了解决异步加载 让浏览器有足够的时间去加载文件，出现了AMD规范

##### AMD规范则是异步加载模块 
允许指定回调函数。指定好了就异步加载，加载完成后即可调用函数

AMD的得意之作就是require.js

##### AMD核心思想：通过define定义模块通过require加载模块 模块前置 提前加载

示例：
###### // index.js

```javascript
require(["main1","main2"],function(m1,m2){
    // ["main1","main2"] 变量1 数组里面写上要加载的模块
    // function(m1,m2) 变量2 函数里面的两个形参就是要加载的模块 当变量1加载完毕之后 函数才会执行

})
```

###### //main1
```javascript
define(
    function(){
        ...
    }
)

//main2
define(function(){
        function add(a){
            a += a;
        }
    }
)

```

------------


#### 如何使用require.js
1、去官网下载
2、将require.js文件引入html页面
3、设置主入口文件main.js -> data-main="main.js" 到script标签
    `<script src="require.js" data-main="main.js"></script>`
4、到main.js文件里写入

###### // main.js
```javascript
require.config({
    // 配置路径
    baseUrl:"../tool/"
    paths:{
        "jquery":"jquery路径"
    }
})
require(["jquery","math"],function($,math){ 
    // 数组里面的东西尽量不要写上路径名字
    // 路径名可以在require.config里面统一定义

    math.add(2) // 通过形参名调用对应文件的函数
})

```
*缺点： 无法按需加载*



------------


### 三、CMD
人类的需求是无止境的，有了异步加载我们还不够，还想要个异步按需加载，于是就有了CMD规范

######  CMD 
###### 首先我们需要下载一个sea.js文件，有了sea.js，通过define定义一个模块 使用require来加载一个模块，就能实现 “就近加载 按需加载”

------------


##### 操作方法：
1、在html使用过script元素插入seajs文件 并调用主入口js文件
###### // html
```javascript
<script src="sea.js"></script>
<script>
    seajs.use("main.js")
<script>
```


2、主入口文件通过define一个模块 该模块练使用require加载一个模块
###### // main.js
```javascript
define(function(require, exports, module){
    var module = require("module1.js")
    console.log(module) // m1 2m
})

```
3、封装一个模块 通过exports暴露接口给调用者
###### // module1.js
```javascript
define(function(require, exports, module){

    var arr = [1,2,3,4]
    exports.m1 = "123546789"
    exports.m2 = arr
})
```

