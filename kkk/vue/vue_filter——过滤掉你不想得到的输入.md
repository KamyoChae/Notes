假设有个需求
==需要对输入框输入的字符串进行过滤==
那么我们可以同过vue的****filter****来实现这个需求*

---
# filter使用方法


html:

```html
<div id="app"></div>
```
#### 方式一：组件内创建局部过滤器
javascript:

```javascript
（1）var App = {
        template:`
            <div>
                <input type="text" v-model="myText" />
                {{myText | reverse}} // 注意该处写法
            </div>
        `,
        
        // 创建局部过滤器 将输入的字符串翻转
        filter:{
            reverse:function(dataStr){
                 // 变字符串 翻转 拼接
                 return dataStr.split("").reverse().join("")
            }
        }
    }
    
    

---


（1）var App = {
        template:`
            <div>
                <input type="text" v-model="myText" />
                {{myText | reverse("这里传一个字符串")}} // 注意该处写法
            </div>
        `,
        
        // 创建局部过滤器 将输入的字符串翻转
        filter:{
            reverse:function(dataStr,args1){ // args1="这里传一个字符串"
                 // 变字符串 翻转 拼接
                 return dataStr.split("").reverse().join("")
            }
        }
    }
```
总结：通过v-model="myText"对myText实现双向绑定，“|”表示在该处使用filter过滤器。myText会成为reverse的实参传进形参dataStr中。若将template中的reverse写成reverse("这里传一个字符串")，则在函数reverse中，"这里传一个字符串"即为arguments[1]，而“myText”则为arguments[0]。



---


#### 方式二：组件外创建公共过滤器
JavaScript：
```JavaScript
var App = {
    template:`
            <div>
                <input type="text" v-model="myText" />
                {{myText | reverse}} // 注意该处写法
            </div>
        `,
    data:function(){
        return {
            myText:""
        }
    }
}

// 创建公共过滤器 将输入的字符串翻转
Vue.filter("reverse", function(data){
    return data.split("").reverse().join("")
})


```
加上下面这段代码即可将结果渲染到页面

```javascript
new Vue({

        el:"#app",
        components:{
            app:App,
        },
        template:`<app />`
    })

```
总结：Vue通过调用filter函数实现全局的过滤器。 由Vue.filter(“函数名”，“函数方法”)实现

**过滤器使用总结**：过滤器只作为过滤需求，不应在过滤器实现过多的功能，以方便后期维护



