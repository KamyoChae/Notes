## 子组件向父组件传值方式
[父组件向子组件传参的方式在这里](https://github.com/KamyoChae/Blog/issues/38)
### 首先，子组件中要在元素中绑定一个点击事件

template：

 ```html
 <div class='list-item ' @click="sendTofather">  
 ...
 </div>
 ```  
 
### 然后，在子组件中，要通过这个点击事件，设置一个自定义的事件
 

```JavaScript
methods:{
    sendTofather:function(){
        console.log(this.display )
        this.$emit("getHomeList",this.display)
    }
}
```
```this.$emit("getHomeList",this.display)```就是我们自定义事件的地方。通过this.$emit("getHomeList",this.display)

getHomeList：自定义的事件

this.display：给这个事件传递的参数，从子组件data里面获取的

### 接着，在父组件调用一下自定义的这个事件，在监听的同时设置一个处理函数dealHomeList
```
  <ul class="nav" @getHomeList="dealHomeList">
  ...
  </ul>

```

### 最后，在父组件中的methods中获取到了子组件的值

```
methods:{
    dealHomeList:function(res){
        console.log(5555555)
        console.log(res)
    }
}
```

## 划重点：意外的坑
如果是使用router-link的方式传参，一定要注意，传回来的参数是绑定在组件入口的，如果监听的地方不是组件入口，就不可能有任何反应！

例如：

父组件：
```javascript
var App = {
    template: `
    <div>
    <router-view v-on:getHomeList="dealHomeList"></router-view>
    <ul class="nav" >
        <router-link :to='{name:"home"}' ><img :src='homeSrc'>首页</router-link>
    </ul>
    </div>
    `,
    data: function () {
        return {
           
        }
    },
    methods:{
        dealHomeList:function(res){
            console.log(5555555)
            console.log(res)
        }
    }

}
```
子组件：
```
var Home = {
template:`
 <router-link :to='{name:"article"}' >
    <div class='list-item ' @click="sendTofather">    
    ...
    </div>
</router-link>`
```
路由配置：
```javascript
var router = new VueRouter({
    routes: [
        { name: 'home', path: '/home', component: Home },
    ...
    ]
})
```
#
### 在父组件中，请注意这个部分：
```
template: `
    <div>
    <router-view v-on:getHomeList="dealHomeList"></router-view>
    <ul class="nav" >
        <router-link :to='{name:"home"}' ><img :src='homeSrc'>首页</router-link>
    </ul>
    </div>
    `
```
如果把```v-on:getHomeList="dealHomeList"```写在了router-link里面，就不会有任何作用。原因是router-view才是真正的组件入口
