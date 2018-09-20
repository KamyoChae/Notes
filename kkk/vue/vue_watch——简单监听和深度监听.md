

何为简单监听 何为深度监听 ？
假如理解了**深度克隆**的概念，简单监听和深度监听就很容易理解了。

---

浅层克隆例如

```JavaScript
var obj = {
    name:"kam",
    age:50,
    hobby:"sleeping",
    arr:[]
}

var newObj = {} // 声明一个新对象
for(prop in obj){
    
    // 浅层克隆
    newObj[prop] = obj[prop];    
}

```
如果去修改newObj的arr属性，因为他们在内存中的地址是一样的，所以obj的arr属性也会受到影响而跟着改变，如果想不受到影响，那就要进行深度克隆。而我们的简单监听和深度监听，也是为了解决类似的地址指向问题而出现的

---

# 如何使用watch

html

```html
<div id="app"></div>
```

#### 方法一 简单监听

```javascript
    var App = {
            template:`
                <div>
                    <input type="text" v-model="userW" />
                    {{userW}}
                </div>
            `,
            
            data:function (){
                return { userW:""}
            },
            
            
            // 使用watch监听简单类型的事件
            watch:{

                userW:function(newV, oldV){
                    console.log(newV + "//" + oldV)
                },


            },

        }


```
总结：当输入值改变 控制台输出两个值 一个是newV 表示新输入的值，另一个是oldV表示旧的值

---


#### 方法二 深度监听
```javascript
    var App = {
            template:`
                <div>
                    <button @click="userD[0].name = 100">
                        改变userD属性name
                    </button>
                </div>
            `,
            
            data:function (){
                return { userD:[{name:456}]}
            },
            
        // 使用watch监听复杂类型的事件
        /* 此处无法进行监听
            watch:{
                userW:function(newV, oldV){
                    console.log(newV + "//" + oldV)
                },
            },
        */
            
            
            // 使用watch监听复杂类型的事件
            watch:{
                userD:{
                    deep:true, // 深度监听
                    handler:function(a){ // handler 不可改
                         console.log(a)
                        console.log("深度监视中") // 监听成功
                    }
                }
            },
        }


```
总结：当监听对象是引用值时，需要用深度监听才能监听到事件的改变。深度监听有三个值得需注意的地方。
>     
> 1. 所要监听的属性 必须作为深度监听的属性名；
> 1. 深度监听对象内需设置deep：true;
> 1. handler关键词不能改

---


```javascript
        
    new Vue({   
        el:"#app",
        components:{
            app:App,
        },
        template:`<app />`
    })
```

**watch总结**：watch只能监听到一个对象，如果要一下子监听到多个对象，还需要用computed
