今天看到了魔法哥以前的一篇文章，里面有提到一个经典的面试题：当我们输入网址后。。。
[点击查看](https://github.com/cssmagic/blog/issues/76)

由于文章的重点不是讲html页面渲染，于是我就动手写了这篇文章
# js加载时间线与render树渲染过程的关系


    
    思考？
    render树生成的过程和js加载时间线的过程是怎样的一个联系？
    render树是拿到html代码和css代码之后生成的吗？
    js在什么时候才加载的？
    
在我们之前的文章里面，有提到过render树生成的过程。

浏览器拿到服务器传来的html代码，开始解析。

如果遇到了link引入一个css，就会异步加载这个css。
如果遇到了一个js，并且这个js没有显式声明为异步，就会去加载这个js，这个过程会阻止了css和dom的解析

解析dom树：根据html元素节点生成的一个dom树，如果这个元素节点浏览器不认识的话，是不会生成到dom树上面的（例如自己写一个```<asdfa>这是我的元素节点</asdfa>```）。

解析cssom树（也有人称cssdom树）：对于一些设置了不显示的样式，例如display：none，cssom树是不会渲染在上面的。

**注意：dom树和cssom树是异步加载渲染节点的过程。即：浏览器解析出html代码里面引入了css文件，还会向服务器发起请求异步加载。**

等到dom树和cssom树都生成好了，浏览器就会把这两棵树合并到一起，名字叫render树。

    [为什么没有jsdom树？因为js是等浏览器把页面渲染完成之后（document.readyState='complete'）才执行，js和render树结合到一起同时渲染是毫无必要的。]

render树建好了，就开始绘制到页面了。绘制过程还会遇到js、img等引用外部链接的元素，img是统一异步加载。js如果又是没有显式声明为异步（script没有defer、aysnc），那就单线程加载这个js，同时阻止了下面的渲染


# js加载时间线
    
第一个阶段：render树生成阶段
-    
- 创建document对象，开始解析web页面，解析html元素和他们的文本内容。添加element对象和text节点到文档中，这个阶段document.readyState='loading'。逐步生成dom树
- 遇到link外部css，创建线程异步加载。继续解析下面的文件。逐步生成cssom树。

- 遇到img，就把img挂到dom树上面，同时异步加载img内容，继续解析下面的代码。
- 遇到script元素，忽略
- render树生成


第二个阶段：render树渲染
-

- 根据render树计算出样式并绘制页面
- 遇到script引入的js，如果没有显式声明aysnc（w3c标准、ie9以上）、defer（只有ie能用）属性，浏览器直接单线程加载，同时阻塞css以及html代码渲染到cssom树和dom树。这个时候，如果js操作了dom节点，恰巧这些节点没有生成，浏览器就会报错！！

（划重点：值得注意的是，render树生成前不会遇到js执行，遇到js也会无视掉。等第二次render树渲染过程遇到的js就会执行了，两者是不一样的。）


- 遇到script引入的js，如果显式声明aysnc（w3c标准、ie9以上）、defer（只有ie能用）属性，浏览器创建线程，继续解析下面的代码。但只有对于aysnc的js代码，加载完毕立即执行。 

- 页面绘制完毕，此时document.readyState='interactive'。刚才所有遇到的defer属性的js代码都会按顺序执行
- 没有defer或是defer执行完毕，这时候document对象就会触发DomContentLoaded事件，表示dom上下文加载完毕，可以触发事件了。这个阶段等同于jq中的```$(document).ready(function(){...})```。


- 这时候不能忘了刚才异步加载的js，我们怎样才能知道异步加载的资源全部都加载好了呢？其实，当所有资源加载完毕之后，readyState就会触发改变，即```document.readyState='complete'```，这个时候window就触发了load事件。
- 由此，js加载完毕

注意：ready()是在render树渲染完之后就马上出发了，不理会异步加载的资源有没有完全加载完毕，而onload是所有资源（包括异步加载的资源也加载完毕之后才会触发）
