# router-view
### 前端路由核心原理

在学习vue的路由配置之前，我们先简单回忆一下前端路由的核心原理

>   1. 前端路由核心原理：锚点值页面不跳转 innerHTML改变
>   2. hashchange事件:url上面的部分锚点数据（#xxx）改变 可以触发到这个事件

#### 例如：
html:
```html
    <a href="#login">登录</a>
    <a href="#register">注册</a>
    <div id="content"></div>
```
javascript:
```javascript

    var div = document.getElementById("content")

    window.addEventListener("hashchange", function(){
        switch(location.hash){
            case "#login": div.innerHTML = "登录页面"; break;
            case "#register": div.innerHTML = "注册页面"; break;
        }
    })
```
##### 概括：当浏览器url地址改变时，页面会进行重排重绘，此时会进行数据请求并将该页面全局刷新，有可能产生白屏或卡顿，对用户产生不友好的体验。为了改进这一数据请求带来的影响，可以通过路由配置来实现局部刷新功能，减少重排重绘


---
# 如何使用router-view配置路由

- ### 分6部走

> 1. 引入vue-router(核心插件)对象
> 2. 安装插件
> 3. 创建一个路由对象
> 4. 配置路由对象
> 5. 指定路由改变局部的位置
> 6. 将配置好的路由对象关联到vue实例中
> 


---
1. 引入vue-router(核心插件)对象

```html
<head>
    <script src="https://unpkg.com/vue-router@2.0.0/dist/vue-router.js"></script>
</head>
```

2. 安装插件

```javascript
    
    Vue.use(VueRouter);

```

3. 创建一个路由对象

```javascript
var router = new VueRouter({
     // 这里可以直接传 也可以不传

});
```
4. 配置路由对象

```javascript
var router = new VueRouter({
     // 这里可以直接传 也可以不传
     // 4. 配置路由对象
     routes:[{path:"/login",component:login}] //login 是一个组件对象 
     // 注意单词routes的写法 区别router 
     // /login就是需要在浏览器url输入的字符串
});
```

5. 指定路由改变局部的位置

```javascript
var App = {
    template:`
        <div>
            <router-view>点我</router-view>
        </div>
    `
}
```


6.将配置好的路由对象关联到vue实例中

```javascript
new Vue({
    el:'#app',
    
    router:router, // 将配置好的路由对象关联到vue实例中
    components:{
        app:App
    },
    template:`<app />`
    
})
```

当代码运行完毕，浏览器url末尾出现了“#/”并且控制台没有报错，则表示路由配置成功啦！
