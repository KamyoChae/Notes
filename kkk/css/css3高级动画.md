
# 形状变换transform
  transform可以实现元素的形状、角度、位置等的变化。

值：
1. rotate(); 以x/y/z为轴进行旋转，默认为z
1. rotatex(), rotatey(), rotatez(), rotate3d(x, y, z, angle) x, y, z --->
1. scale(); 以x/y为轴进行缩放
1. scale(x, y) 接受两个值，如果第二参数未提供，则第二个参数使用第一个参数的值
1. scalex(),scaley() 值是数字表示倍数，不加任何单位
1. scalez()
1. scale3d()  scale3d(sx,sy,sz)
1. skew(); 对元素进行倾斜扭曲
1. skew(x, y);接受两个值，第一个参数对应X轴，第二个参数对应Y轴。如果第二个参数未提供，则默认值为0
1. skewx(), skewy()
1. translate(); 可以移动距离,相对于自身位置。
1. translate(x, [y])
1. translatex(),translatey(),translatez(),translate3d(x, y, z)



# 变换原点transform-origin 
任何一个元素都有一个中心点，
默认情况下，其中心点是居于元素x轴和y轴的50%处

# 过渡动画transition  
1. transition  属性是css3的一个复合属性，主要包括以下几个子属性
1. transition-property:指定过渡或动态模拟的css属性
1. transition-duration:指定过渡所需要的时间
1. transition-timing-function:指定过渡函数
1. transition-delay:指定开始出现的延迟时间

# 动画铺垫animation  

 

## ```@keyframes``` 动画关键帧


animation 动画会按照keyframes 关键帧里面指定的帧状态而过渡执行。


```css
/* 0% - 100% 代表动画的时间过渡 */
@keyframes demoMove{
    0%{ background-color:red;}
    10%{ background-color:green;}
    20%{ background-color:white;}
    50%{ width:200px;}
    100%{ height:200px;}
}
```


帧频里面如果只有 0% 和 100%两个关键帧，那么可以用 from to 代替

## **animation** 属性为css3的复合属性，主要包括以下子属性
1. animation-name:             此属性为执行动画的 keyframe 名
1. animation-duration:         此属性为动画执行的时间
1. animation-timing-function:  指定过渡函数速率
1. animation-delay:            执行延迟时间
1. animation-direction:        normal/reverse/alternate/alternate-reverse; 
1. animation-iteration-count:  infinite/number; 
1. animation-fill-mode:        forwards/backwards/both/none;

 
###   animation-direction:主要用来设置动画播放方向
1. normal	默认值。动画按正常播放。	
1. reverse	动画反向播放。
1. alternate	动画在奇数次（1、3、5...）正向播放，在偶数次（2、4、6...）反向播放。	
1. alternate-reverse	动画在奇数次（1、3、5...）反向播放，在偶数次（2、4、6...）正向播放。	

 
### animation-iteration-count: 主要用来定义动画的播放次数。
1. n 播放次数
1. infinite 无限次


### animation-play-state: 主要用来控制元素动画的播放状态。
1. running 播放
1. paused  暂停

### animation-fill-mode: 定义在动画开始之前和结束之后发生的操作。
主要具有四个属性值：
1. none: 默认值，表示动画将按预期进行和结束，在动画完成其最后一帧时，动画会反转到初始帧处	
1. forwards:  表示动画在结束后继续应用最后的关键帧的位置
1. backwards:  会在向元素应用动画样式时迅速应用动画的初始帧（结合延迟1s来看）
1. both: 元素动画同时具有forwards和backwards效果



---

# 高级动画

### transform-style: flat|preserve-3d;

1. flat: 默认，子元素将不保留其 3D 位置
1. preserve-3d : 子元素将保留其 3D 位置。
1. 注意：transform-style 属性需要设置在父元素中, 高于任何嵌套的变形元素
1. 设置了transform-style:preserve-3d的元素，就不能防止子元素溢出设置overflow：hidden；否则会导致preserve-3d失效


### perspective - 景深
- 可以简单的把perspective理解成人距离显示器的距离
- 此值越大的效果越差 越小效果越好 
- 假设你距离100米和1米的距离去看一个边长一米的正方体
- 重点记住perspective的值要大于3d物体的值

### perspective: 600px ;默认值none
- 物体距离人眼的距离是600px;


### perspective-origin:默认值是50% 50%
### perspective与translateZ() 前者以屏幕为基准 往眼睛靠近 后者往离远

- 在3D变形中，除了perspective属性可以激活一个3D空间之外，
- 在3D变形的函数中的perspective()也可以激活3D空间。
- 他们不同的地方是:
- perspective用在舞台元素上（变形元素们的共同父元素）;
- perspective()就是用在当前变形元素上，
- 并且可以与其他的transform函数一起使用。


```css
.wrapper{
    perspective:600px;
}

div{
    transform:perspective(600px);
}
```

### 注意：当有多个变形元素时，第一种只有一个透视点，第二种每一个变形元素都有一个透视点

### backface-visibility: visible | hidden
在元素运动过程中，能否展示元素的背面

### Css 3 动画性能优化
1. 尽可能多的利用硬件能力，如使用3D变形来开启GPU加速
2. 尽可能少的使用box-shadows与gradients, 这两个都是页面性能杀手，能避免尽量避免
3. 尽可能的让动画元素不在文档流中，以减少重排 position:fixed|absolute
4. 优化 DOM reflow

启用3d效果的动画流畅度比较高，但同时也会占据更多的内存与消耗，权衡兼顾
