
之间调用函数的方式：

```javascript
var person = function(){}

function show(){
  console.log("hello world!")
}
show()

function Person(name){
  this.name = name
}

var p1 = new Person("狗子")
Person.call(window, "window")
```

 ## ES6中的函数
 
 ```javascript
 function Person(name){
    this.name = name || "none"
 }
 
 let p = new Person("狗子")
 let p1 = new Person()
 let p2 = new Person(null)
 let p3 = new Person(0)
 let p4 = new Person("")
 let p5 = new Person(false)
 let p6 = new Person(undefined)
 
 如果我们需要往name传入null 那结果是this.name = none
 
 为了解决这个问题 出现了ES6的默认值写法
 
  function Person(name="none"){
    this.name = name
 }
 在该函数中 不传值则会默认this.name = none
 注意：当值绝对等于undefined的时候才会走默认值
 
 
 这里还有一个要注意的地方 
 function max(num1, num2){
    console.log(num1, arguments[0])
    console.log(num2, arguments[1])
    num1 = 44
    console.log(num1, arguments[0])
    arguments[0] = 66
    console.log(num1, arguments[0])
 }
 max(1,2)
 结果 1 1 / 2 2 / 44 44 / 66 66
 
 如果加上ES5严格模式
  function max(num1, num2){
     "use strict"
    console.log(num1, arguments[0])
    console.log(num2, arguments[1])
    num1 = 44
    console.log(num1, arguments[0])
    arguments[0] = 66
    console.log(num1, arguments[0])
 }
 max(1,2)
 结果 1 1 / 2 2 / 44 1 / 44 66
 
 如果使用ES6的赋值表达
  function max(num1 = 1, num2){ 
    console.log(num1, arguments[0])
    console.log(num2, arguments[1])
    num1 = 44
    console.log(num1, arguments[0])
    arguments[0] = 66
    console.log(num1, arguments[0])
 }
 max(1,2)
 结果 1 1 / 2 2 / 44 1 / 44 66
 竟然与ES5严格模式一模一样的结果！！
 
 严格模式下arguments失效
 
  
 ```
 #
 ## 惰性求值
 ```javascript
 function getValue(){
    console.log("hello")
    return 99
 }
 
 function count (n, m = getValue()){
    console.log(n + m)
 }
 
 count(1) // 结果是 hello 100
 count(1, 1) // 结果是 2
 ```
 
 #
 ## 不定参数
 
 ```JavaScript
 function count (arr, ...arg){
    console.log(arg)
    console.log(arguments)
    for(var i = 0; i < arr.length; i++){
        arr[i] = arg[i] + 1
    }
 }
 count([], 1,2,3,4,5,6,7,8,9,10) 
 结果：[],  [],1,2,3,4,5,6,7,8,9,10
 
 
 let arr = []
 function fn(first, last, ...arg){
    ...
 }
 fn(1,2,3,4,5,6,7,8,9,10)
 
 结果 没有报错
 但如果有多个arg或是arg放在中间 就会报错 
 
 小总结：在每个函数中，最多有且只能有一个不定参数 不可以有默认值 ...必须是最后一个参数
 ```
 
 # 
 ## 神奇的...
 
 ```javascript
 平时我们要把两个数组拼接起来 需要使用cancat
 但是es的...却可以轻松的把两个数组合并起来
 
 例如：
 var arr1 = [1,2,3,3,3,3]
 var arr2 = [2,2,2,2,2,2]
 
 console.log(...arr1, ...arr2)
 结果输出 [1,2,3,3,3,3,2,2,2,2,2,2]
 ```
 
 #
 ## 箭头函数
 ### 箭头函数具有绑定this的能力
 this绑定分为4种
 - 默认绑定（函数单纯执行 this指向window）
 - 隐式绑定（谁调用 this指向谁）
 - 显示绑定（apply call bind（硬绑定））
 - new绑定 （权重最高）
 
 ```javascript
 var name = "window"
 var obj = {
  name:'obj',
  
  print:function(){
    console.log(this.name)
  }
  obj.print()
  var newPrint = obj.print.bind(window) //
  var 
 }
 ```
 注意：
 - 箭头函数没有this 
 - 箭头函数具有绑定this的能力
 - 箭头函数中绑定的this是离自己最近的非箭头函数作用域中的this
 #
 ### 箭头函数尿性：
- 形参只有一个 可以不用括号
- 语句只有一条 可以不用花括号
- return 一个值 直接写出来
```javascript
let a = name => name

等价于

let a = function(name){
  return name
}
```

# 
1. 箭头函数没有this
1. 没有arguments
1. 没有super(class)
1. 没有prototype
1. 不能被new操作符执行
1. 箭头函数不能被作为构造函数 更多功能用于计算、数据流向等 方便js引擎优化代码
1. 箭头函数具有this绑定能力


箭头函数绑定this
```javascript
var name = "狗子"
var ar = (name) => console.log(this.name) 
结果： 狗子
原因： this指向window

  
```
