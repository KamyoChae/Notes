
# router-link 命名路由

如果对路由还不是很理解，[可以点击这里查看文章](https://github.com/KamyoChae/Notes/blob/master/vue/vue_router-view%E2%80%94%E2%80%94%E8%B7%AF%E7%94%B1%E9%85%8D%E7%BD%AE.md)


---
假设我们有一个需求，界面有一个按钮，单独点击该按钮就可以实现页面的刷新。
没有什么前提，我们直接甩个a元素就行了
如果要实现局部刷新，我们就做一个页面路由

至于这个页面路由怎么做才相对合理？就是这篇文章要讲的内容了

- html:
```HTML
<div id="app"></div>
```
###### 下面两段代码我们在上一篇文章已经有所了解，这里不多说
- JavaScript：
```JavaScript
        // 1. 安装插件
        Vue.use(VueRouter);
```
```JavaScript
        // 2. 自定义组件页面
        var Login = {
            template: `<div>登录界面</div>`
        }

        var Register = {
            template: `<div>注册界面</div>`
        }
        
```
在创建路由的时候，我们可以这样做


```JavaScript        
        // 3. 创建路由
        var router = new VueRouter({
           
            // 4. 配置路由对象
            routes: [

                {  path: "/login", component: Login },
                {  path: "/register", component: Register }

            ] 

        });
```
```JavaScript
        var App = {
            template: `
                <div>
                    <router-link to="/login">点我登录</router-link>
                    
                    /*
                    *    <router-link to="/login">点我登录</router-link>
                    *    <router-link to="/login">点我登录</router-link>
                    *    <router-link to="/login">点我登录</router-link>
                    */
                    
                    <router-link to="/register">点我注册</router-link>
                    <router-view></router-view>
                </div>
            `
        }
```
但这样一来，如果多个按钮对应一个路由地址，一旦要修改路由名，就要修改多个linkto地址，即“router-link to="/login"”中的login。这是十分不方便的
##### 为了方便开发人员，就出现了命名路由的概念，如下

```JavaScript        
        // 3. 创建路由
        var router = new VueRouter({
           
            // 4. 配置路由对象
            routes: [

                // 此处多了个命名 name
                { name: "login", path: "/login", component: Login },
                { name: "register", path: "/register", component: Register }

            ] 

        });
```
在数组routes中，给每一个路由对象加上name属性（name: "login"）
```JavaScript
        var App = {
            template: `
                <div>
                    <router-link :to="{name:'login'}">点我登录</router-link>
                    <router-link :to="{name:'register'}">点我注册</router-link>
                    <router-view></router-view>
                </div>
            `
        }
```
路由通过绑定的name属性进行跳转（:to="{name:'login'}"），就无需重复绑定路由了。
##### 注意"to"前需要加上“:”，否则" "会把引号里面的东西转换成字符串。

```JavaScript
        
        new Vue({
            el: '#app',

            router: router, // 将配置好的路由对象关联到vue实例中
            components: {
                app: App
            },
            template: `<app />`

        })
```

总结：router-view是用于装载页面的地方，不能删。给路由设置name属性，链接才能进行对应的路由跳转
