
# 拓展知识
## 盒模型的不同解析
在css中盒模型被分为两种，第一种是w3c的标准盒模型，另一种是IE6混杂模式的传统模型。 他们都是对元素计算尺寸的模型。但他们的不同是计算的方式不同。

我们先来了解一下什么是标准盒模型 什么是混杂模式盒模型

# 

### 标准盒子和混杂盒子
1. W3C标准盒模型
    
    - 盒子高度（空间） = width + padding + border;
    
        width 为内容高度。即width不包括padding 和 border

2. IE6混杂模式盒模型
    
    - 盒子高度 （内容）= width (padding + border )
    
        即 设置width的数值就是element 的空间高度，width包含padding 和border

可以看出，在盒子里面没有任何东西的情况下，在w3c标准里面，盒子高度是由内边距（padding）、宽度（width）、边框（border）之和相加起来的。而混杂模式情况下，盒子高度则是由宽度（width）包含着内边距（paddig）和边框宽度（border)

### 如何触发混杂模式？
必备条件：

1、删掉html最上面的那行渲染标准，即
```html
<!DOCTYPE html>
```
2、放到IE6及以下的浏览器使用

那么如果我们一定要使用怪异模式下的盒模型，那该怎么办。可以使用box-sizing。

## box-sizing

borer-box：混杂模式解析
content-box：标准模式解析

