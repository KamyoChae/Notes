# vue生命周期（高清图）

### vue生命周期

- 组件创建
> 1. beforeCreate
> 2. created
 

- 组件装载
> 3. beforeMount
> 4. mounted

- 组件改动
> 5. beforeUpdate
> 6. updated

- 组件销毁
> 7. beforeDestroy
> 8. destroyed

```JavaScript
var App = {
        template:`<h1>组件app</h1>`,
        data:function(){
            return {
                text:"hello world"
            }
        },
            
```
#### 创建组件。
此处可以操作数据 并且可以实现vue-html的影响 例如发起ajax请求获取数据
        
```JavaScript

        berforeCreate:function(){
            // 组件创建之前
        },
        created:function(){
            // 组件创建之后

            // 可以操作数据 并且可以实现vue-html的影响 例如发起ajax请求获取数据
        },

```

#### 装载组件。
此处可以进行组件装载 但装载组件只能执行一次 
```javascript

            
        beforeMount:function(){
            // vue起作用 装载数据到dom之前
        },
        mounted:function(){
            // 装载数据到dom之后 且只执行一次
        },
```

#### 更改组件。
此处可进行组件的数据测试，每次更改数据都会执行这两个函数
```JavaScript

        // 一下两个基于数据改变而触发
        // 可以用于测试、开发
        beforeUpdate:function(){
            // dom数据改变前 可以获取原dom
        },
        updated:function(){
            // dom数据改变后 可以获取新dom
        },
```
#### 组件销毁。
对应 remove和append | v-if false和true 。组件的销毁都会执行这两个函数
```JavaScript
        // 对应父组件 v-if false 销毁之前
        beforeDestroy:function(){
            // 销毁之前
        },
        destroyed:function(){
            // 销毁之后
        }

    }
```

![vue](https://cn.vuejs.org/images/lifecycle.png)
