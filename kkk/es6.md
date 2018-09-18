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
