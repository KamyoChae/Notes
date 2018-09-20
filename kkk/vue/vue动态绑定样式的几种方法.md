
今天看到一篇文章，里面出现了自己看不懂的css绑定方式，于是去vue官方了解了一下样式绑定方式，顺便写下这篇总结

Vue动态绑定CSS

如果我们想通过vue给对应的dom元素添加css样式，可以通过以下几种方式：

# 绑定类名方式

## 1. 潜意识写法
首先，我们知道，如果想将js代码里面的东西渲染到dom元素属性上面，需用v-bind的方式绑定。
因此，我们首先想到的是下面这个写法：

```
<div id="app" >
    <div v-bind:class="myclass"></div>
</div>

```
```
new Vue({
    el:"#app",
    data:{
        myclass:"red"
    }
})
```
生成的结构是下面这样子
```
<div class="red"></div>
```

## 2. 通过对象绑定

除了上面的写法，我们还可以这样写

```
<div id="app" >
    <div v-bind:class="{red : flag，active:false}"></div>
</div>

```
```
new Vue({
    el:"#app",
    data:{
        flag:true,
    }
})
```
生成的结构也是下面这样子
```
<div class="red"></div>
```
请注意两种写法的不同。第一种是通过v-bind把data里面的值拿出来，第二种是把class绑定一个对象，通过这个对象里面的属性true/false来确定要渲染。

## 3. 通过函数方法返回
当然在这里我们还可以通过函数方法计算返回对象属性：
```
<div v-bind:class="classObject"></div>
```

```
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
    }
  }
}
```
生成的结构也是下面这样子
```
<div class="active "></div>
```
## 4. 利用数组的方式

我们也可以利用数组的方式添加多个样式
例如：

```
<div v-bind:class="[active, oactive]"></div>
```
```
data:{
    active:"red",
    oactive:"text-focus"
}
```

生成的结构也是下面这样子
```
<div class="red text-focus"></div>
```
#
请注意，如果不加上左右两个中括号“[...]”，只会绑定第一个样式。
比如：


```
<div v-bind:class="active, oactive"></div>
```
```
data:{
    active:"red",
    oactive:"text-focus"
}
```
生成的结构也是下面这样子
```
<div class="red"></div>
```

# 绑定内联样式

大致上和我们绑定到类名的方式差不多
## 从data里面直接取出来

```
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
```
data: {
  activeColor: 'red',
  fontSize: 30
}
```

## 从一个对象取出来

```
<div v-bind:style="styleObject"></div>
```
```
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

## 从多个对象取出来

```
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```
```
data: {
  baseStyles: {
    color: 'red',
    fontSize: '13px'
  },
  overridingStyles{
      background:'red'
  }
}
```

### vue对样式的兼容还做了个很不错的地方
官方文档只用一句话说清：
> 当 v-bind:style 使用需要添加浏览器引擎前缀的 CSS 属性时，如 transform，Vue.js 会自动侦测并添加相应的前缀。

意思是说，通过style添加的内联样式，可以不用写浏览器引擎的前缀，比如-webkit-trasform，vue会自己检测并自动添加上去

#

##  在组件上绑定样式

其实也是和在dom元素上面绑定样式差不多。来看官方文档里面的写法：
```
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```
```
<my-component class="baz boo"></my-component>
```
结果是这样子的


```
<p class="foo bar baz boo">Hi</p>
```
#
对于带数据绑定 class 也同样适用：

```
<my-component v-bind:class="{ active: isActive }"></my-component>
```

当 isActive 为 truthy[1] 时，HTML 将被渲染成为：

```
<p class="foo bar active">Hi</p>
```


## 小总结：
虽说vue给我们提供了很多种样式绑定的方式，看上去实际自己似乎并不需要这么多，但有时候利用这些奇巧淫技，说不定还能为我们节省很多时间
