# 基础储备
在学习flex弹性盒模型子元素样式之前，我们先复习一下父元素的样式

[点击查看上一篇：flex弹性盒模型总结-容器篇](https://github.com/KamyoChae/Blog/issues/26)

---

### 看到这里，为了防止知识点忘记，我们有必要 看一看上一篇讲的六个父元素样式

1. flex-direction 决定主轴的方向
        
        row columns row-reverse columns-reverse

1. flex-wrap 是否换行
        
        wrap wrap-reverse nowrap

1. flex-flow flex-direction和flex-wrap的简写
        
        row columns row-reverse columns-reverse | wrap wrap-reverse nowrap

1. justify-content 项目在主轴上的对齐方式
        
        flex-start flex-end center space-between space-around

1. align-items 在侧轴上如何对齐

        flex-start flex-end center baseline stretch

1. align-content 多根轴线的对齐方式
        
        flex-start flex-end center space-between space-around stretch


### 上片文章的内容

style| ---|---|---|---|---|---
---|---|---|---|---|---|---
flex-direction| row|columns|row-reverse|columns-reverse
flex-wrap| wrap|wrap-reverse|nowrap|
flex-flow| row|columns|row-reverse|columns-reverse| wrap|wrap-reverse|nowrap|
justify-content| flex-start|flex-end|center|space-between|space-around|
align-items：| flex-start|flex-end|center|baseline  |  stretch |
align-content|flex-start|flex-end|center|space-between|space-around|stretch
---

# 开始正题

flex为复核属性，且必须配合父元素display:flex使用。

### 以下6个属性设置在项目（子元素）上:
1. flex-grow：放大比例
1. flex-shrink：缩小比例
1. flex-basis：伸缩基准值
1. flex：是flex-grow, flex-shrink 和 flex-basis的简写
1. order：排列顺序
1. align-self ：单个项目对齐方式

#
### 子元素（子项目）：

### 1.flex-grow 将剩余空间按比例分割并添加到对应的子元素中


按默认样式计算完该子元素宽度后，再将该行的剩余宽度按照子元素设置的flex-grow的比例添加给子元素

#

我们通过下面这个demo加深理解：
```css
.box{
    display: flex;
    width: 500px;
    height: 500px;
    font-size: 0;
    border: 1px solid #000;
}
.item{
    box-sizing: border-box;
    flex-grow:1;
    height: 150px;
    background: red;
}
.item:nth-child(2){
    flex-grow: 4;
    width: 200px;
    background: rgb(41, 103, 236);
    border: 1px solid #fff;
}
```
```html
<div class="box">
    <div class="item">123</div>
    <span class="item">456</span>
</div>
```
#
在box里面，为了方便计算，我们设置了字体大小为0 ```    font-size: 0;```

当前情况是第一个item没有设置宽度，第二个宽度是200px，父元素box是500px。

剩余空间 = 500 - 200 = 300 

flex-grow 会将剩余空间按照这个比例添加到对应的子元素里面，由此，我们看到第一个item设置了flex-grow：1，第二个item设置了flex-grow：4；也就是说，将剩余空间分割成5分，分别按照这个比例添加到宽度里面。

因此，第一个item的宽度 = 300 / 5 = 60 

第二个item的宽度 = 200 + 300 / 5 * 4 = 440 


#

### flex-shrink 超出盒子的部分 按照这个比例 在子元素中删除对应比例的宽度
默认情况下，我们在父元素不设置```flex-wrap:wrap;```，子元素会全部压缩排列成一行，这是为什么捏？实际上是，每一个子元素都会有一个默认的值—— ``` flex-shrink:1 ```。

那么是否可以认为，与flex-shrink相对的flex-grow也是一个默认的值1呢？答案是可以的。

#
#### 我们来对比一下这两个属性

- flex-grow 将剩余的部分按照这个比例添加到对应的子元素宽度
- flex-shrink 将多余的部分按照这个比例删减掉对应的子元素宽度



- grow 增长
- shrink 收缩

#### ++那么这样就很好理解了，flex布局中的子元素默认都有一个flex-grow 和一个flex-shrink 并且都等于1，前者是将子元素按比例拉伸，后者是将子元素按比例缩小。++

#

### flex-basis 在主轴上的宽度

这个样式真的是个大坑！！

为了描述得更准确，我查了不少资料，发现别人解释的都是啥，越看越迷糊！！

#### 要了解这个样式，我们有必要先回顾一下flex-direction样式：
flex-direction有以下参数

1. row 默认参数 从左到右 主轴是x轴 也就是横轴
1. row-reverse 反转左右 主轴是x轴 也就是横轴
1. column 从上到下 主轴是y轴 也就是竖轴
1. column-reverse 反转上下 主轴是y轴 也就是竖轴

#### basis：基础 初始的意思

++flex-basie 实际上是设置主轴上面的一个基础宽度或高度，而这个宽度或高度完全取决于主轴是横轴还是竖轴。++
#### 例如：

1. **主轴在横轴** （即 flex-direction:row | row-reverse）
    
    - 设置了flex-basis：200px；那么这条主轴（横轴）上面的子元素的**宽度**都是200px（注意关键词：宽度），如果子元素设置了width（注意是width，不是min-width或max-width），flex-basis也会把这个width覆盖掉（也就是只有flex-basis生效的意思）。
    - 但如果还设置了min-width或是max-width，那么这个flex-basis就会人性化地选择退让，把宽度width的设置权限给min-width或是max-width（也就是min-width或max-width会生效的意思）。
1. **主轴在竖轴** （即 flex-direction：column | column-reverse）
 
    - 设置了flex-basis：200px；那么这条主轴（横轴）上面的子元素的**高度**都是200px（注意关键词：高度），和上面主轴在横轴的意思一样，如果我们设置了min-height或max-height，还是会生效的。不然就是覆盖掉高度height。

为了方便理解，这里放个源码demo：

```css
.box{
    display: flex;
    flex-direction: column;
    width: 500px;
    height: 500px;
    font-size: 0;
    border: 1px solid #000;
}
.item{
    box-sizing: border-box;
    flex-grow:1;
    flex-basis: 200px; 
    height: 100px;
    max-height: 150px;
    background: red;
}
.item:nth-child(2){
    flex-grow: 4;
    width: 200px;
    background: rgb(41, 103, 236);
    border: 1px solid #fff;
}
```

```html
<div class="box">
    <div class="item">123</div>
    <span class="item">456</span>
</div>
```
so ~ ~ ~ ~

就是这么简单的意思，可别被网上各种文章误导了哈！

#

### flex 悄悄咪咪的简化写法

**常用简化写法**:

#### flex : flex-grow flex-shrink flex-basis;

- flex:1 —>  flex:1 1 0%;
- flex:3 —> flex:3 1 0%;

**注意**:
1. flex 布局和原来的布局是两个概念
1. 部分css属性在flexbox里面不起作用
1. eg：float， clear， column,vertical-align 等等


#### 拓展知识（需要一定的数学运算能力）
**真实宽度的求法**：

1. 第一步 计算加权值：
    - son1 = (flex-shrink) * flex-basis；
    - son2 = (flex-shrink) * flex-basis；
    - …..
    - sonN = (flex-shrink) * flex-basis；
     
    加权值 = son1 + son2 + …. + sonN；

2. 第二步 计算压缩的宽度 w 
    - w = (子元素flex-basis值 * (flex-shrink)/加权值) * 溢出值
    - 缩减值1：(flex-basis1 * 1/ 加权值) * 溢出值
    - 缩减值2：(flex-basis2 * 2/ 加权值) * 溢出值
    - 缩减值3：(flex-basis3 * 3/ 加权值) * 溢出值

3. 第三步 最后son1、son2、son3，的实际宽度为：

    - flex-basisn– 缩减值n  = sonn 真实宽度；

#
###  order 自定义排序顺序

我们知道，元素渲染时是按照dom数来进行渲染的，dom顺序是怎样的，显示的结果就是怎样。
但是如果我们有个特殊需求，希望将dom树的第三个节点元素放到最前面 再怎么做呢？常规办法是进行绝对定位，将周围的往后移，自己往前靠。要不就是进行dom节点从操作，复制这个节点到最前面，然后删除这个节点。当然还有其他更复杂的操作，这里我们不做讨论。

可见，常规办法虽然可以生效， 但操作起来并不是十分方便。

值得高兴的是，flex布局中的order样式提供了完美的自定义排序方式。

#### order：number 


- number 可以是正数、0、负数。
- order只会从小到大将这几个子元素排列起来，要想随便乱排，休想！


number定义项目的排列顺序。数值越小，排列越靠前，默认为0

### demo:
```css
.box{
    display: flex;
    width: 500px;
    height: 500px;
    font-size: 0;
    border: 1px solid #000;
}
.item{
    box-sizing: border-box;
    flex-grow:1;
    height: 100px;
    border: 1px solid #ffffff;
    font-size: 16px;
    color: #fff;
    max-height: 150px;
    background: red;
}
.item:nth-child(2){
    flex-grow: 4;
    order: 6;
    background: #2967ec;
}
.item:nth-child(3){
    flex-grow: 4;
    order: 1;
    background: #d529ec;
}
.item:nth-child(4){
    flex-grow: 4;
    order: 3;
    background: #eccf29;
}
.item:nth-child(5){
    flex-grow: 4;
    order: 2;
    background: #ec29a1;
}
.item:nth-child(6){
    flex-grow: 4;
    order: 9;
    background: #29ec8a;
}
.item:nth-child(7){
    flex-grow: 4;
    order: 4;
    background: #29ece2;
}
.item:nth-child(8){
    flex-grow: 4;
    order: 8;
    background: #ec2933;
}
```
```html
    <div class="box">
            <div class="item">1</div>
            <span class="item">2</span>
            <div class="item">3</div>
            <span class="item">4</span>
            <div class="item">5</div>
            <span class="item">6</span>
            <div class="item">7</div>
            <span class="item">8</span>
            <div class="item">9</div>
            <span class="item">10</span>
    </div>
```
在这个demo里面，我们可以看到，dom节点是从小到大排序的，但是尽了兴order自定义排序之后，渲染出来的效果就不一样了。

#
### 最后一个知识点： align-self——搞特殊的淘气孩子
 
#### align-self属性允许单个项目有与其他项目不一样的对齐方式，

++可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。++

#### align-self: auto | flex-start | flex-end | center | baseline | stretch;

其实和align-items一样的，只是这里只允许在子元素设置


# 总复习：

看完这两篇笔记,算是学了一遍flex布局。但要彻底掌握熟练，还需要大量的练习。

下面回顾一下我们学了什么。

## 在父元素设置的样式
style| ---|---|---|---|---|---
---|---|---|---|---|---|---
flex-direction| row|columns|row-reverse|columns-reverse
flex-wrap| wrap|wrap-reverse|nowrap|
flex-flow| row|columns|row-reverse|columns-reverse| wrap|wrap-reverse|nowrap|
justify-content| flex-start|flex-end|center|space-between|space-around|
align-items：| flex-start|flex-end|center|baseline  |  stretch |
align-content|flex-start|flex-end|center|space-between|space-around|stretch
---

## 在子元素设置的样式

style| ---|---|---|---|---|---
---|---|---|---|---|---|---
flex-grow|0|n|
flex-shrink|0|n|
flex-basis| 像素值
flex| **1** (flex-grow)|**1** (flex-shrink)|**0%**  (flex-basis)
order| -n|0|n|
align-self|auto | flex-start | flex-end | center | baseline | stretch;
---
