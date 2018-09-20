## 在理解之前
vue的prop传值是一个很巧妙的方式，就是有点绕，刚学的时候能理解，一段时间后使用起来又有点迷糊，原因大概是理解得不够深刻。这里重新总结一下思路

## 为什么这么绕

首先看一段代码：

```javascript
 var App = {
    props:['mes'],
    template:`
        <div>{{mes}}</div>
    `,
    data(){
        return {
        }
    }
}
new Vue({
    el: "#app",
    data: {
        message: 50
    },
    components:{
        app:App
    },
    template:`<app :mes = "message" />`
})
```
```html
    <div id="app"></div>
```
首先我们要明确一下，props中的“mes”从哪来？
不难看出，全文上下就只有Vue对象中的template里面有：```template:`<app :mes = "message" />```

那么，为什么mes会写在app元素里面？这个问题不好回答，我们先跳过，继续看：

:mes = "message" ？不难看出，我们是从data里面通过v-bind取出了一个message值，存到了mes里面。

再看看data，里面果然有个message:```message：50```
## 发现规律
#### 那么，我们是否可以由此推理出：
我们需要用v-bind从data里面取出数据（message），并存到一个属性（mes）里面，而这个属性必须绑定在tamplate模板上面，用来传递（template:`<app :mes = "message" />`），而这个template又引用自上面的一个App组件，由于在template上面绑定了一个mes属性，我们就需要在这个App组件里面设定一个接收的“仓库”，而不巧的是，props数组就是这个用来接收传值的“仓库”！！！在这个仓库中，我们又可以通过插值表达式来把仓库里的mes渲染出来！！

原来这就是值传递的过程。

## 检验推断

为了验证一下这个思路是不是正确的，我们通过下面的demo检验一下：

```javascript

// 注册全局组件

Vue.component("my-btn", {
    template: `<button style="background:red">漂亮的按钮</button>`
})

// 创建子 声明子 使用子

// 创建子

var myHead = {
    template: `
        <div>
            <h1>这里是头部</h1>
            {{title}}
            <my-btn />
            <span>55555</span>
        </div>
    `,
    props: ["title"]
}
var myBody = {
    template: `<h1>这里是身体<my-btn /></h1>`
}

var myFoot = {
    template: `
<div>
    <h1>这里是脚部</h1>
    <button @click=" addd(5) ">点我</button><my-btn />
</div>    `,
    methods: {
        addd: function (num) {
            alert(num++)
        }
    }
}

// 声明子

var mod = {
    components: {
        myhead: myHead,  // 可以这样写
        "mybody": myBody, // 也可以这样写
        "my-foot": myFoot // 推荐这样写
    },
    template: `
        <div>
            <myhead :title=" xxx " />
            <mybody />
            <my-foot />
        </div>
      `,
    data: function () {
        return {
            xxx: "这是一个mod的title"
        }
    },

    // 下面这样写会报错 The "data" option should be a function that returns a per-instance value in component definitions.
    // 意为 “data”选项应该是一个在组件定义中返回每个实例值的函数。
    // data:{
    //     xxx: "这是一个mod的title"
    // }
}

// 使用子

new Vue({
    el: "#app",
    components: {
        app: mod
    },
    template: "<app/>",
})
```
```html
<div id="app"></div>
```
拿去执行以下， 结果正如我们所推理~~

