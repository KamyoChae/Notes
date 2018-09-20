整理了一下以前的笔记，决定把flex重新总结一遍
# 弹性盒模型flex布局

第一步，在容器上面写上```display:flex;```这个容器就变成弹性盒模型了

接下来，我们看看这个容器可以设置哪些属性

 ### 一定要注意的是，设置了flex布局的元素，它的子元素都会强制转化成块级元素！！

### 以下6个属性设置在容器上:

1. flex-direction 决定主轴的方向
2. flex-wrap 是否换行
3. flex-flow  flex-direction和flex-wrap的简写
4. justify-content  项目在主轴上的对齐方式
5. align-items  在侧轴上如何对齐
6. align-content  多根轴线的对齐方式

## flex-direction 决定主轴的方向
### direction：方向的意思
**flex-direction：row | row-reverse | column | column-reverse**

flex-direction属性决定主轴的方向,即项目的排列方向

### demo:

```css
 .box{
    display: flex;
    flex-direction: row | row-reverse | column | column-reverse;
    width: 500px;
    height: 500px;
    border: 1px solid #000;
}
.item{
    width: 200px;
    height: 100px;
    background: red;
    border: 1px solid #fff;
}
```

```html
<div class="box">
    <div class="item">123</div>
    <div class="item">456</div>
    <div class="item">789</div>
</div>
```
1. row（默认值）：主轴为水平方向，起点在左端。
1. row-reverse：主轴为水平方向，起点在右端。
1. column：主轴为垂直方向，起点在上沿。
1. column-reverse：主轴为垂直方向，起点在下沿。

#

### row：默认方式，在水平方向左起，类似float浮动

当我们正在父元素设置flex-direction：row时，没有任何变化，因为这是默认的样式，也就是默认情况下，flex方向是从左到右的水平方向。

#
### row-reverse 水平方向反转
#### reverse:反转的意思
如果我们希望布局是从右到左，可以设置flex-direction:row-reverse;

这个设置可以做出排列方式的效果，切换row和row-reverse可以做出左右排布的反转效果，甚至能拓展做成UI菜单栏动画。

#
### column : 从上往下排布，类似多个块级元素从上往下排列
如果希望做成列表那样子的效果，可以考虑一下column属性，它是把在最后面插入最先渲染的元素。

# 
### column-reverse ：反转上下
如果我们想直接通过优化css而减少js逻辑处理，将最新的数据渲染到列表最上面，使用column-reverse是非常不错的方式。由于反转了列表，最新渲染的元素一定会在最上面显示。


## flex-wrap 排不下的换行方式
默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。

### flex-wrap: nowrap | wrap | wrap-reverse;

1. nowrap（默认）：不换行。
1. wrap：换行，第一行在上方。
1. wrap-reverse：换行，第一行在下方。

#
###  nowrap（默认）：不换行。
我们设置flex-wrap，它的默认形式是不换行的，也就是不管有多少个子元素，它都会挤成一行。

#

### wrap 换行 子元素的第一行在最上面，从上往下排
也就是正常的换行样子，排满了一行换下一行排

#

### wrap-reverse 换行 第一行在最下面，从下往上排
倒置正常的换行，也就是类似先在最下面排列然后往上一层层排

#
## flex-flow——flex-direction和flex-wrap的缩写方式

flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。

#### 在这里我们复习一下flex-direction和flex-wrap有哪些属性
> 
> ### flex-direction：row | row-reverse | column | column-reverse
> 1. row（默认值）：主轴为水平方向，起点在左端。
> 1. row-reverse：主轴为水平方向，起点在右端。
> 1. column：主轴为垂直方向，起点在上沿。
> 1. column-reverse：主轴为垂直方向，起点在下沿。
> 
> 
> ### flex-wrap: nowrap | wrap | wrap-reverse;
> 
> 1. nowrap（默认）：不换行。
> 1. wrap：换行，第一行在上方。
> 1. wrap-reverse：换行，第一行在下方。

#

##  justify-content 横轴对齐方式

### justify-content: flex-start | flex-end | center | space-between | space-around;
justify-content属性定义了子元素在主轴上元素的对齐方式。（睁大眼睛看清楚，这是主轴上元素的对齐方式，而不是主轴的对齐方式，这个一定要分清楚，很多人就是在这掉坑了。）
### 注意，如果要使用对齐方式，那就必须设置换行，否则没有作用

```css
.box{
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;  
    justify-content: flex-start;
    width: 500px;
    height: 500px;
    border: 1px solid #000;
}
```


它可取5个值，具体对齐方式与轴的方向有关。下面假设主轴为从左到右。
1. flex-start（默认值）：左对齐
1. flex-end：右对齐
1. center： 居中
1. space-between：两端对齐，项目之间的间隔都相等。
1. space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
 

#
### flex-start（默认值）：左对齐
也就是默认情况下的对齐方式，从左边一路排过去

#
### flex-end 右对齐，类似float=right的效果
也就是子元素全部往右边靠

#
### center 居中

也就是将元素居中，类似text-align：center，但text-align ： center会把子元素里面的文字居中

#
### space-between 两端对齐 但子元素之间的间隔都相等

也就是左右两边紧贴边框，中间的间隙是等距的，类似columns布局的效果

#
### space-around 在同一行中，每个子元素的距离都相等

左右两边的间距是元素与元素之间间距的一半，类似 ```margin：0 10px；```这样的效果

**小拓展：** 

++轴的方式，如果是横轴方向即flex-direction：row，那么就是默认的情况，flex-start表示的是左边。++

++但是！如果我们把轴方向设为flex-direction：cloumn；同时超出部分我们做了换行，那么就相当于绕着原点逆时针旋转了270°，结果是flex-start表示的是右边！！！++

demo：
```css
.box{
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    justify-content: flex-end;
    width: 500px;
    height: 500px;
    border: 1px solid #000;
}
.item{
    width: 100px;
    height: 100px;
    background: red;
    border: 1px solid #fff;
}
```

```html
<div class="box">
    <div class="item">123</div>
    <span class="item">456</span>
    <div class="item">789</div>
    <div class="item">123</div>
    <span class="item">456</span>
    <div class="item">789</div>
    <div class="item">123</div>
    <span class="item">456</span>
    <div class="item">789</div>
</div>
```

---

### 看到这里，为了防止文章过长导致知识点忘记，我们有必要 看一看开头讲的六个父元素样式

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

---

#
## align-items 纵轴上面的对齐方式 类似垂直对齐方式

我们知道，justify-content可以设置在横轴上面的对齐方式，；可以左对齐、右对齐、居中、平分、两边对齐。接下来我们了解一下如何在纵轴设置对齐方式



### align-items: flex-start | flex-end | center | baseline | stretch;


它可能取5个值。具体的对齐方式与交叉轴的方向有关，下面假设交叉轴从上到下。
1. flex-start：交叉轴的起点对齐。
1. flex-end：交叉轴的终点对齐。
1. center：交叉轴的中点对齐。
1. baseline: 项目的第一行文字的基线对齐。
1. stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

#
### flex-start 默认的对齐方式

上顶到纵轴的最顶端，类似行级元素的```vertical-align：rop```

#
### flex-end 底部对齐

也就是下压到纵轴的最底端

#
### center 在纵轴居中

类似```vertical-align：middle```

#
### baseline 基于基准线对其方式

也就是子元素对齐方式是基于文字基准线的，即使上内边距是很大的距离，都会以文字基准线对齐。（这里补充一点，如果这个子元素没有字体，那么设置了align-items：baseline样式的元素就会以底边作为基准值来对齐。）

这个例子只能用demo来说明了

demo：
```css

.box {
    display: flex;
    align-items: baseline;
    width: 500px;
    height: 500px;
    border: 1px solid #000;
}
.item,
.item1,
.item2,
.item3 {
    width: 100px;
    height: 100px;
    background: red;
    border: 1px solid #fff;
}
.item1 {
    padding-top: 1em;
}
.item2 {
    padding-top: 3em;
}
.item3 {
    padding-top:  4em;
}
```
```html
    <div class="box">
        <div class="item2">123</div>
        <span class="item">456</span>
        <div class="item">789</div>
        <div class="item">123</div>
        <span class="item1">456</span>
        <div class="item">789</div>
        <div class="item3">123</div>
        <span class="item2">456</span>
        <div class="item">789</div>
    </div>
```

#
###  stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

#### stretch 拉伸 伸展的意思

默认值。元素被拉伸以适应容器。

如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。


## align-content 多轴对齐方式

### 在学习align-content之前，我们先要回顾一下前面的两个样式，justify-content和align-items。


style| ---|---|---|---|---
---|---|---|---|---|---
justify-content| flex-start|flex-end|center|space-between|space-around|
align-items：| flex-start|flex-end|center|baseline  |  stretch |
---|---|---|---|---|---



### 而align-content的属性是这个样子


style| ---|---|---|---|---|---
---|---|---|---|---|---|---
justify-content| flex-start|flex-end|center|space-between|space-around|
align-items：| flex-start|flex-end|center|baseline  |  stretch |
align-content|flex-start|flex-end|center|space-between|space-around|stretch
---|---|---|---|---|---|---

通过对比可以明显看出，align-content单词上面的意思就是用justify-content和align-items和后面和前面拼接成align-content。而样式属性也是将baseline去掉，然后两者进行合并。

#
### 那么接下来，我们需要探索一下他俩的结合究竟能产生什么样子的变化

首先，我们需要搞清楚什么是“多轴”。

之前看的一些杂七杂八的文章，对这个轴的说法乱七八糟的，其实没那么复杂。这个“多轴”，指的是换行之后的另一行。比如：一行占满了，这就是一根轴，另起一行，那么另起的那行就是第二根轴！！就这么简单。


这么看来，后面的属性就很好理解了。

1. flex-start	 换行之后 贴近顶部 

2. flex-end	    换行之后 贴近底部 注意不是从下往上排

3. center	换行之后 行行之间靠近 上下居中

4. space-between	换行是换行了 不过上下是紧贴的 中间居中平分

4. space-around	 换行是换行了 不过每一个子元素都有自己的上下外边距 平均拉开竖轴

5. stretch 默认样式，换行之后，没有填充满一行的就把一行填满，类似百度图片的布局方式
 


# 最后，用一个表格总结本片文章的内容

style| ---|---|---|---|---|---
---|---|---|---|---|---|---
flex-direction| row|columns|row-reverse|columns-reverse
flex-wrap| wrap|wrap-reverse|nowrap|
flex-flow| row|columns|row-reverse|columns-reverse| wrap|wrap-reverse|nowrap|
justify-content| flex-start|flex-end|center|space-between|space-around|
align-items：| flex-start|flex-end|center|baseline  |  stretch |
align-content|flex-start|flex-end|center|space-between|space-around|stretch




