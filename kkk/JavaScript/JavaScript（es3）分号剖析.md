# JavaScript（es3）分号剖析
### javascript分号问题在开发过程中可能会引起某些不必要的bug 为此次我们进行了部分探索测试，以图摸清这个梗，从此少踩坑...

#### 首先抛出我们的测试结果：
> 
> 1. “(”、“[”、“/”、“+”、“-”，都会在上一行代码不加分号的情况下，与上一行代码相接
> 1. “++”、“--”在上下两行都不加分号的情况下，与下一行代码相接
> 1. 如遇上了return才会在改行末尾加上分号。
    

# 与上一行代码相接


### 1. "(" 情况

```JavaScript
function f(n){
    return n * n;
}
```


// 加分号情况：

```JavaScript
var a1 = f ; // 将函数f指向a1
(5)+5; // 10
console.log(a1 ) // 函数f
```


// 不加分号情况：

```JavaScript
var a2 = f  // 将函数f指向a2
(5)+5 // 10
console.log(a2 ) // 30
```



### 2.  "[" 情况

```JavaScript
var arr = [400,300,200,100,50];
```

// 加分号情况：

```JavaScript
var b1 = arr;
[0];
console.log(b1) // 400,300,200,100,50
```


// 不加分号情况：

```JavaScript
var b2 = arr
[0]
console.log(b2) // 400
```



### 3.  "/"

```JavaScript
var num = 1000;
```


// 加分号情况：

```JavaScript
var c1 = num;
///2; // 报错
```


// 不加分号：

```JavaScript
var c2 = num
/2 
console.log(c2) // 500
```


### 4.  "+、-"

```JavaScript
var x = 5,
    y = 10;
```

// 加分号：

```JavaScript
var d1 = x;
+ // -
y;
console.log(d1) // 5 。 - : 5
```

// 不加分号：

```JavaScript
var d2 = x
+ // -
y
console.log(d2) // 15 。 - : -5
```



# 与下一行代码相接：

```JavaScript
var i = 20,
    j = 30;
```

// 加分号:

```JavaScript
var e1 = i;
// ++/--; 报错
++
j;
console.log(e1,j) // 20 31。 - : 29
```

// 不加分号:
```JavaScript
var e2 = i
++ // --
j
console.log(e2,j) // 20 31。 - : 29
```

# 在行末自动加分号

```JavaScript
+ function test1(){
   console.log("test1") // test1
    return  // 此处默认有个";"
    {
        console.log("这里是return") // 无输出
    }
}()
+ function test2(){
   console.log("test2") // test2
    return console.log("这里是return") // 有输出
}()
```
