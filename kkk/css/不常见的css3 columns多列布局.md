如果我们想在网页上实现报纸那样子一竖竖的布局，难道要用div设定一个宽度，然后计算这个宽度能容纳多少个字，超出部分截取到另一个div里面吗？如果不知道columns，实现这样的需求我可能会考虑这样子的蠢办法。

庆幸的是，如果我们想做出报纸杂志那样子的多列布局，可以用css3的columns多列布局来轻松实现。

# 

#### 我们来了解一下官方的解（fei）释（hua）：
为了能在Web页面中方便实现类似报纸、杂志那种多列排版的布局，W3C特意给CSS3增加了一个多列布局模块（CSS Multi Column Layout Module）。它主要应用在文本的多列布局方面，这种布局在报纸和杂志上都使用了几十年了，但要在Web页面上实现这样的效果还是有相当大的难度。
    

# columns初步了解
1. columns：复合属性
1. column-width：设置每一列的列宽
1. column-count：设置分割成多少个列
1. column-gap：设置列与列间隙
1. column-rule：列与列之间的样式规则，不会占据列宽
1. column-span：设置列的标题占据1列或是全部，只支持1或all（在子元素使用）
 
#### 注意只有columns后面有“s”

## columns
```css
.box {
    width: 800px;
    border: 1px solid #000;
    column-rule-style: dashed;
    column-width: 200px;
    column-gap: 10px;
}
.child{
    column-span:all;
}
```
## column-width 和 column-count 

- column-width:   指每一列的宽度 根据容器宽度自适应 （最小宽度） 
- column-count:   指规定的列数 唯一精准的是列数
 
columns-width和columns-count只需要写其中一个就够了，前一个是设置每列宽度，后一个是设置分割成多少列。这两个样式都是写在父元素的。

## column-gap：设置列与列之间的间隙

gap：间隙的意思。

列与列之间的间隙，是不占据列宽的。
column-gap：50px;表示列与列之间有50px的间隙

## colummn-rule:列与列之间的样式，也就是那条分割线

这个属性和border可谓是如出一撤，border是border-width、border-style、borer-color，它是column-width、column-style、column-color，简直一模一样。

## column-span

span：跨度的意思

这个样式只能写在子元素里面，意为将子元素设置成款列效果，参数为1或all，当为1时，只占据当前一列顶部作为标题，为all时则会横跨所有列，成为所有列的标题
