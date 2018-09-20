# requestAnimationFrame 初步认识
#### request：请求
#### animation：动画
#### frame：帧
看我名字这么长，拆开来看就是“请求动画帧”的意思了

如果浏览器要进行刷新（重排或重绘）就会通知requestAnimationFrame说我要刷新啦，然后requestAnimationFrame就会和它一起刷新了。如果浏览器刷新时间设为10ms，那么requestAnimationFrame就会隔10ms刷新一次，如果浏览器刷新时间设为520ms刷新一次，那么requestAnimationFrame也会毫不客气跟着隔520ms刷新一次

#

# 用计时器做动画存在弊端
在不使用css的情况下，通常只要是做动画，我们都会想到用定时器来做，但是定时器做动画也是有弊端的，比如执行队列的干扰和掉帧等等情况。

弊端：
1. 不是百分百准确。如果执行过程中突然有另一个进程执行，并且执行200毫秒，那么这个计时器就会进入等待，等200毫米之后继续执行。结果出现页面卡顿的样子。
2. 设置定时时间不准。设置0毫秒会延迟4毫秒左右执行，计时器放到计时器异步执行当中，然后放到事件队列当中，然后主线程访问事件队列，如果有函数那就拿过来执行
3. 如果设置时间过短，就会掉帧。原因：浏览器大概16毫秒刷新一次（根据电脑性能及当时运行环境有所改变），如果时间过短也不会动

验证延迟方式：
```javascript
var lastTime = new Date().getTime()
setTimeout(function(){
var a = new Date().getTime() - lastTime
    console.log(a) // 2~4
},0)

```
#
# 针对定时器做动画不准的解决方案

我们知道，浏览器是16毫秒左右刷新一次，如果我们能在浏览器刷新的时候执行动画，那么就能完美解决不掉帧的情况了。
幸运的是，H5 ```requestAnimationFrame```的方法的就是在浏览器刷新时执行，我们只需要传一个函数进去就行了，不需要手动设置时间。
---

## requestAnimationFrame特性

1. 页面刷新前执行一次
2. 用法和setTimout相似
3. 可以用cancelAnimationFrame取消动画
4. 但是存在兼容性问题 仅限ie10以上的浏览器（css3动画也是。如果要兼容，只能优雅降级用定时器做了）


当需要兼容低版本浏览器做动画的时候，我们只需要用定时器来实现即可，那么高版本的浏览器我们是不是还要做一套css？
#### 答案：并不是。我们从始至终都用requestAnimationFrame来做就行了。requestAnimationFrame本身就是给ie10以上的各种高版本浏览器使用的，因此我们直接封装一个requestAnimationFrame兼容性写法即可。

#

## 封装兼容性的requestAnimationFrame函数


调用这个函数
```javascript

window.requestAnimationFrame = (function(){
    return window.requestAnimationFrame ||
            window.webkitRequestAnimationFrame ||
            window.mozRequestAnimationFrame ||
            function(callback){
                window.setTimeout(callback,1000/60)
            }
}())

```

清除这个函数

```javascript
window.cancelAnimationFrame = (function(){
    return window.cancelAnimationFrame ||
            window.webkitCancelAnimationFrame ||
            window.mozCancelAnimationFrame ||
            function (id){
                clearTimeout(id)
            }
}())
```
使用方法
```javascript
var timer = requestAnimationFrame(move)
var i = 0
function move(){
    if(i < 10){
        i++
        console.log("页面刷新了")
    }else{
        cancelAnimationFrame(timer)
    }
}
```
搞定~~ 就是这么简单啦 
