# 景深 transform-style:preserve-3d
##### preserve: 英文翻译是“保留”的意思。preserve-3d 意为保留3d效果

transform-style有两个值，一个是 **++flat(平面)++**，一个是 **++preserve-3d(保留3D效果)++**

要注意的是，tansform-style要设置在高于所有嵌套的元素才能生效。这句话翻译过来的意思是，要放在父元素才能生效。但又由于它是保留了3D效果，所以我们若在此基础上增加一个overflow:hidden;这个元素也会失效。

**总结一下：**

若想使用transform-3d，要有以下三个充分必要条件
1. transform-3d写在父元素
2. transform-3d的值写上preserve-3d
3. 不能再使用overflow

 例如：
 ```css
 .item{
     transform-style:preserve-3d;
     width:200px;
     height:200px;
     border:1px solid #000;
 }
 
 .box{
     background:red;
     width:200px;
     height:200px;
     transform:rotateX(45deg); /* 沿x轴旋转45度 */
 }
 
 ```
 ```html
 <div class="item">
    <div class="box"></div>
 </div>
 ```
 
根据上面几行代码我们可以看出，实现的效果是在一个200px*200px的黑色方框里面有个布满的红色方块，但是由于红色方块进行了围绕x轴旋转变形，所以我们只能看到红色正方形变成了红色长方形。这个时候的效果，并不是很容易看出，```transform-style```这货到底干了什么活。

这个时候perspective——景深出现了

**perspective：透视，远景的意思**

当我们在父元素加入了perspective之后，这个元素仿佛活了起来，出现了我们想要的3D效果

 ```css
 .item{
     transform-style:preserve-3d;
     perspective:600px;
     width:200px;
     height:200px;
     border:1px solid #000;
 }
 
 .box{
     background:red;
     width:200px;
     height:200px;
     transform:rotateX(45deg); /* 沿x轴旋转45度 */
 }
 
 ```
 ```html
 <div class="item">
    <div class="box"></div>
 </div>
 ```

**小总结：** 

++如果想使用transform-style，还需要用perspective来激活，perspective是透视的意思，类似于美术中的投影透视效果。++

**小拓展**：
在3D变形中，除了perspective属性可以激活一个3D空间之外，在3D变形的函数中的perspective()也可以激活3D空间。他们不同的地方是:perspective用在舞台元素上（变形元素们的共同父元素）;perspective()就是用在当前变形元素上，并且可以与其他的transform函数一起使用



---
# perspective 写法之谜

上面我们知道，perspective是透视的意思，是写在父元素里面的，但是我们能不能皮一下，写在子元素里面呢？
- 答案是可以的

于是我们就把父元素的```perspective:600px;``` 这样写

 ```CSS
 .item{
     transform-style:preserve-3d;
     
     width:200px;
     height:200px;
     border:1px solid #000;
 }
 
 .box{
     perspective:600px;
     
     background:red;
     width:200px;
     height:200px;
     transform:rotateX(45deg); /* 沿x轴旋转45度 */
 }
 
 ```
 #### 但是
 结果并没有出现任何效果，问题出在哪？
 其实，如果写在子元素的话，并不能直接使用perspective 而是将它放进transform当中去即可，例如
 ```CSS
   .item {
        transform-style: preserve-3d;
        width: 200px;
        perspective-origin: top;
        height: 200px;
        border: 1px solid #000;
        }

    .box {
        background: red;
        width: 200px;
        height: 200px;
        transform: perspective(600px) rotateX(80deg);
        /* 沿x轴旋转45度 */
    }
```
```.box{transform: perspective(600px) }```的意思是，给每个box都加上一个景深。
由此，我们才弄清楚了perspective的写法

 

# 眼源 perspective-origin:top;

perspective-origin ，透视点定位的地方，也就是我们眼睛所处在的位置

当我们旋转色块九十度的时候，由于色块没有厚度，我们是看不到色块的。当我们设置```perspective-origin:top;```的时候，相当于我们眼睛所在的位置在top这个位置，从top即顶部往下看。


# 背面可视 backface-visibility：visible || hidden

backface：背面。visibility：可视。
是否显示元素转动180°之后的背面


 ```css
 .item{
     transform-style:preserve-3d;
     perspective:600px;
     width:200px;
     height:200px;
     border:1px solid #000;
 }
 
 .box{
     backface-visibility:visible; /* 显示背景 */
     background:red;
     width:200px;
     height:200px;
     transform:rotateX(45deg); /* 沿x轴旋转45度 */
 }
 
 ```
 ```html
 <div class="item">
    <div class="box"></div>
 </div>
 ```

### 在此处扩展个小知识点：旋转之后的元素 坐标轴也是跟着旋转的

# 如何优化css3动画

1. 尽可能多的利用硬件能力，如使用3D变形来开启GPU加速
2. 尽可能的让动画元素不在文档流中，以减少重排
3. 尽可能少的使用box-shadows与gradients, 这两个都是页面性能杀手，能避免尽量避免
4. 尝试使用css hack
>     写css样式的时候，恐怕最头疼的就是各个浏览器下的兼容性问题，即css hack
>     不同的浏览器对css的解析结果是不同的
>     因此会导致相同的css输出的页面效果不同，这就需要css hack来解决浏览器局部的兼容性问题。
>     使用css、 hack将会使用你的css代码部分失去作用
>     然后借助条件样式，使用其原css代码在一些浏览器解析
>     而css hack代码在符合条件要求的浏览器中替代原css那部分代码。    
5.优化 DOM reflow
