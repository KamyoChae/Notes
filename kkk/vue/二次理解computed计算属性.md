## 理解之前
翻了翻之前写的一篇笔记[computed多值监听](https://github.com/KamyoChae/Blog/issues/4)，水分很多。最近有一点小收获，决定重新写一篇对computed的理解


## 关于computed

引用[vue官网](https://cn.vuejs.org/v2/guide/computed.html)的一句话```"不同的是计算属性是基于它们的依赖进行缓存的。只在相关依赖发生改变时它们才会重新求值。 "```

这就意味着只要computed里面的一个函数中的值message没有发生改变，多次访问 这个函数doMessage就会立即返回之前的计算结果，而不必再次执行函数。

demo：

```html
<div id="app">
    <p>{{doMessage}} | now {{now}}<p>
</div>

```

```javascript
new Vue({
    el:"#app",
    data:{
        message:50
    },
    computed:{
        doMessage:function(){ 
            return this.message * 2
        },
        now(){ // es6的另一种写法
            return Date.now()
        } 
    } 
)}
```
在上面的demo里面，只要this.message没有发生改变，多次访问 doMessage就会立即返回之前的计算结果，而不必再次执行函数。
而由于Date.now()是一直改变的，所以每当触发重新渲染时，调用方法now()将总会再次执行函数。

## 使用computed的好处

由于computed的特性，我们可以利用它做一些比较复杂并且耗时的计算，如果不使用计算属性，假设写在一个methods里面，我们点击一下，过去了三秒才把结果计算出来，这对我们的用户来说是及其糟糕的体验。

## computed的一些坑

如果我们想通过一个methods方法去直接改变computed计算属性，回报错
demo：

```html
<div id="app">
    <p>{{doMessage}} | now {{now}}<p>
    <button @click="count">count</button>
</div>

```

```javascript
new Vue({
    el:"#app",
    data:{
        message:50
    },
    computed:{
        doMessage:function(){ 
            return this.message * 2
        },
        now(){ // es6的另一种写法
            return Date.now()
        } 
    },
    methods:{
        count(){
            this.doMessage = 999
        }
    }
)}
```
请注意我们在html中加了个button。这时候点击button会报错，原因是computed默认只有能获取不能设置（计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter ）。如果我们需要进行设置，就需要把computed写成下面这个样子：
demo：
```javascript
new Vue({
    el:"#app",
    data:{
        message:50
    },
    computed:{
        doMessage: {
            get() {
                return this.message * 2
            },
            set(newValue) {
                console.log(newValue)
                this.message = newValue
            }
        },
        now(){ // es6的另一种写法
            return Date.now()
        } 
    },
    methods:{
        count(){
            this.doMessage = 999
        }
    }
)}
```

当然了，如果通过这个methods方法去改变data里面的值而间接改变computed的值还是可以的。比如：
demo：
```javascript
new Vue({
    el: "#app",
    data: {
        message: 50
    },
    computed: {
        doMessage: function () {
            return this.message * 2
        },
        now() { // es6的另一种写法
            return Date.now()
        }
    },
    methods: {
        count() {
            this.doMessage = 999
        }
    }
})
```
#
注：
- 凡是函数内部有this.相关属性，该属性改变都会触发该函数
- 双向绑定的输入框，复制粘贴同样的数据不会触发该计算属性

相关阅读：[watch的深度监听和简单监听](https://github.com/KamyoChae/Blog/issues/5)
