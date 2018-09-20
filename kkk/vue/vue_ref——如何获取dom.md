# 如何在vue中获取dom元素

1. 在组件dom的部分，任意元素写上ref="thisDom"
2. 通过组件对象this.$refs.thisDom 获取到元素
 


---
下面来看例子：
###### （如果对vue周期不是十分清楚，[可以点击查看这篇文章](https://github.com/KamyoChae/Notes/blob/master/vue/vue_%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.md)）
```javascript

        var App = {
            template: `
                <div>
                    <button ref="btn">我是按钮</button>
                </div>`,
            data: function () {
            },
            beforeCreated: function () {
                // 这里不能操作数据，只是初始化了事件
                console.log(this.$refs.btn) // 这里不会有任何输出
            },
            created: function () {
                // 这里可以操作数据了
                console.log(this.$refs.btn) // undefined 虽可以操作数据，但无法操作未进入dom结构的数据
            },
            beforeMount: function () {
                // new Vue装载前 即将替换<div id="app"></div>
                console.log(this.$refs.btn) // undefined 虽可以操作数据，但无法操作未替换dom结构的数据
            },
            mounted: function () {
                // 装载new Vue之后
                // 只有在这里才能进行dom操作
                
                // 着这里我们通过this.$refs.btn获取到了这个dom元素
                console.log(this.$refs.btn) // <button>我是按钮</button>
                console.log(this) // this指向VueComponent{...}
               
                })
            },

        }

```
```javascript
        new Vue({
            el: "#app",
            components: {
                app: App
            },
            template: "<app/>",
        })


```

总结：只有在new Vue装载之后，才能进行dom操作。如需进行dom操作，首先要在html元素中设置属性"**ref='el**'" , 在vue内部通过"**this.$refs.el**"来获取dom元素

---

## 然而这里还有一个十分隐蔽的知识点，一般人我不告诉他...

请看下面代码：

```javascript

        var App = {
            template: `
                <div>
                
                    <input type="text" v-if="isShow" ref="input" />
                </div>`,
            data: function () {
                return {
                    isShow: false
                }
            },
            // 省略部分无关代码...
            mounted: function () {
                // 装载new Vue之后
                // 只有在这里才能进行dom操作
                console.log(this) // VueComponent{...}

                this.isShow = true;
                this.$refs.input.focus()  // 如果直接使用这句 则会报错
                
                
            },

        }

```

在上面的代码里面，可以看到input的默认情况是删除的（isShow: false），当new Vue装载完毕，我们把元素插入，没有问题！可是下一步获取输入框的焦点时，惊奇的事情发生了——“this.$refs.input.focus()”居然报错了！
> ###### Error in mounted hook: "TypeError: Cannot read property 'focus' of undefined"

要解决这个问题，其实很简单，这里就是关于实战开发的技巧了

将上面的
```javascript
this.$refs.input.focus()  
```
改成下面的代码段即可
```javascript
// 我们希望vue真正渲染dom到页面以后，才获取输入框的焦点
                this.$nextTick(function () {
                    this.$refs.input.focus()
                })
```
