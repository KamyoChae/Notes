如果你错过了watch篇，[请点击这里查看watch的使用](https://github.com/KamyoChae/Notes/blob/master/vue/vue_watch%E2%80%94%E2%80%94%E7%AE%80%E5%8D%95%E7%9B%91%E5%90%AC%E5%92%8C%E6%B7%B1%E5%BA%A6%E7%9B%91%E5%90%AC.md)

---
我们知道，通过watch可以通过**简单监听**监听原始值，通过**深度监听**监听到引用值。然而他们都只能监听到单个值，如果我们要实现多值监听，有什么办法吗？

##### 答案是有的，可以通过computed实现多值监听


---
# computed的使用方法

html:
```html
<div id="app"></div>
```

javascript:
```javascript

var myrate = {
    template: `<h1>定义子组件</h1>`
}

var App = {
    components: { // 声明子组件
        "my-rate": myrate
    },
    template: `<div>
    <my-rate></my-rate>
    <input type="text" v-model="n1" />
    +
    <input type="text" v-model="n2" />
    *
    <input type="text" v-model="rate" />
    = {{result}}
</div>`,

    data: function () {
        return {
            n1: 0, n2: 0, rate: 0,
        }
    },

    // 在此处使用computed监听n1,n2,rate。并将计算结果返回给result
    computed: {
        result: function () {
            return ((this.n1 - 0) + (this.n2 - 0)) * this.rate
        }
    }
}

new Vue({
    el: "#app",
    components: {
        app: App,
    },
    template: `<app />`
})

```

总结：不难看出，如果要使用computed监听多值，只需要在组件内computed对象里写上需要绑定的插值函数即可，通过"this." + data里面return的值，可实现多值的监听。

如上实现的是一个计算器功能。
值得注意的是：在输入框双向绑定的值是字符串类型，如果要进行操作，则需要通过“-0”实现隐式转换（或是用Number()实现显式转换），否则“+”连字符会把左右两边的字符串链接起来而无法实现计算功能。
