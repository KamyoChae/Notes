
一、let命令

    1、基本用法
     类似var 但所声明的变量只在let命令所在的代码块内有效
     如：{let a = 10} console.log(a) ->报错
    
    2、不存在变量提升
     不能先使用后声明 
     如：typeof a 报错 （es5不报错）
    
    3、暂时性死区
     在代码块中 使用let命令声明变量之前 该变量不可用 就称为暂时性死区
     只要块级作用域内存在let命令 他所声明的变量就binding掉这个区域 即：不再受外部的影响
     如：
      var tem = 123
      if(true){
        tem = 456
        let tem // 作用域内存在let 该作用域被绑定（binding） 不再受他人控制
      }    
     
     4、不允许重复声明
      这一点和java c 等其他类型的语言相似 在相同作用域内 不能重复声明一个变量
      
二、块级作用域
     1、为什么需要块级作用域？
      es5只有全局作用域和函数作用域，没有块级作用域
      
      第一种情况
      如：
        var tmp = new Date();
        function f() {
          console.log(tmp);
          if (false) {
            var tmp = 'hello world';
          }
        }
        f(); // undefined
       原意是：
           if代码块的外部使用外层的tmp变量，内部使用内层的tmp变量。
           但是，函数f执行后，输出结果为undefined，原因在于变量提升，导致内层的tmp变量覆盖了外层的tmp变量。
     
       第二种情况
       如：
         var s = 'hello';
          for (var i = 0; i < s.length; i++) {
            console.log(s[i]);
          }
          console.log(i); // 5
       变量i只是用来控制循环的 但是循环结束后 他并没有消失 泄露成了全局变量
     
     
     2、es6块级作用域
        let实际上为JavaScript新增了块级作用域 实现类似java c 等语言类似的变量空间
     
     
     3、块级作用域与函数声明
        es5(严格模式)规定 函数只能在顶层作用域和函数作用域之中声明，不能再块级作用域声明
        例如：
          1: if(){function f(){}}
          2: try{function f(){}}catch(e){}
          
        以上两种写法在严格模式下（es5）是非法的 
        但为了兼容以前的旧代码 ECMA3模式下都能运行并且不会报错
     
     
 三、const命令
      
      1、基本用法
        const声明一个只读常量 一旦声明 常量的值就不能改变
        只声明 不赋值 回报错  必须在声明的同时赋值
        
      2、const本质 
        将值保存在一个内存地址 该地址是不可改变的
      
      3、let与const的异同点
        
        同：
          作用域：相同
          变量是否提升：不提升
          是否可以重复声明：否
        
        异：
          能否改变赋值：let能 定义的是变量  const不能 定义的是常量
          
      4、声明变量的6种方法
        es5只有2种声明变量的命令
        es6除了添加let const命令外 还有import class 命令 所以es6 有6种声明变量的方法
      
      
      
四、顶层对象属性
   
   顶层对象 在浏览器环境下指的是window对象 在node指的是global对象。es5中 顶层对象的属性与全局变量是等价的
   
      
      
      
      
      
      
      
      
      
      
      
      
      
      
  
