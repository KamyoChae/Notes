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
 
 ```
