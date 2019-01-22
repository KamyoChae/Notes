
什么是ajax？

简单来说，ajax就是用JavaScript以异步的形式操作xml（现在操作的是json）
ajax是“Asynchronous JavaScript And Xml”的缩写



为什么要使用ajax？

利用form表单进行数据交互时，会对整个页面进行刷新。

form表单进行数据提交demo：
<form action="./test.php" method="GET">
    <input type="text" name="username"> // 输入按钮
    <input type="submit" id="submit"> // 提交按钮
</form>

当点击提交按钮时，浏览器地址栏跳转到“test.php”的地址，同时在地址的后面拼接上“username=输入的值”，同时页面会进行全部刷新。

如果地图是用form表单来做，则每次获取数据时，我们明明是需要某个位置的信息（即局部刷新），可整个页面都会进行刷新，这对用户体验来说是十分糟糕的。

form表单做不到局部刷新的功能，我们可以交给ajax来做。



如果想取消form表单默认的提交方式 可以在form元素内不写属性 如
<form>
    <input type="text" name="username"> // 输入按钮
    <input type="submit"> // 提交按钮
</form>

或者取消submit默认事件：

submit.onclick = function (e) {
    e.preventDefault();
}

 
 ajax请求过程

 1、浏览器。首先要有一个浏览器
 2、需要一个ajax对象。只要有了这个对象，就能调用函数
 3、初始化请求方式 ajax.open(method，url，true)。
 (method：请求方式。url：请求地址。true/false：异步请求/同步请求。)
 4、发送请求 ajax.send()。 
 5、监听数据状态 onreadystatechange 
 6、校验返回的数据 status == 200 数据找到了

封装ajax兼容性写法
    思路：
 1、浏览器
 2、创建ajax对象
    new XMLHttpRequest()。
    ie浏览器没有XMLHttpRequest()，创建ajax对象只能用new ActiveXObject('Microsoft.XMLHttp');
 3、初始化。
    xhr.open('GET', 'test.php', true);
 4、发送请求。 
    xhr.send();
 5、监听数据状态。
    xhr.onreadystatechange = function(){
        if(xhr.readystate == 4){
            // 执行体
        }
    }

 6、校验返回的数据。
    xhr.onreadystatechange = function(){
        if(xhr.readystate == 4){
            if(xhr.status == 200){
                console.log('响应成功')
            }
        }
    }

    把思路整合成代码：
 ```
 var xhr = null;
 if(window.XMLHttpRequest){
     xhr = new XMLHttpRequest();
 }else{
     xhr = new ActiveXObject('Microsoft.XMLHttp');
 }

 xhr.open('GET', 'test.php', true);
 xhr.send();
 xhr.onreadystatechange = function(){
     if(xhr.readystate == 4){
         if(xhr.status == 200){
             console.log('响应成功')
         }
     }
 }
 ```
注意：
GET请求可以把数据写在请求的地址后面，例如
 xhr.open('GET', 'test.php?name=kam&age=18', true);

POST请求不能这么做
要使用POST请求，有两个要注意的地方
1、数据要写在send()里面；
2、要设置请求头setRequestHeader。意思是如果你要给别人传一个数据，你得告诉对方你的数据是什么格式的
代码如下：
xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");

post请求方式：
```
 xhr.open('POST', 'test.php', true);
 xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
 xhr.send('name=kam&age=18');
 xhr.onreadystatechange = function(){
     if(xhr.readystate == 4){
         if(xhr.status == 200){
             console.log('响应成功')
         }
     }
 }
```
对两种请求方式进行封装，造一个ajax轮子：
```
function ajaxFun(method, url, data, callback, flag) {

    // method 请求方式 url 请求地址 data 传输的数据 callback 回调函数 flag 是否异步发起
    var xhr = null;
    if(window.XMLHttpRequest){
        xhr = new XMLHttpRequest(); 
    }else{
        xhr = new ActiveXObject("Microsoft.XMLHttp");
    }

     xhr.onreadystatechange = function(){
        if(xhr.readystate == 4){
            if(xhr.status == 200){
                callback(xhr.responseText)
            }else{
                consoel.log("Ajax Error")
            }
        }
    }
    method = method.toUpperCase(); // 解决大小写兼容
    if(method == "GET"){
        xhr.open(method, url + "?" + data, flag); // 数据拼接
        xhr.send(); // 发送数据
    }else if(method == "POST"){
        xhr.open(method, url, flag); 
        xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded"); // 定义请求头
        xhr.send(data); 发送数据
    }
}

```
注释及拓展：

action：请求的地址
method：请求的方式。GET、POST。get获取数据，post传输数据，delete删除数据
enctype：规定在发送表单数据之前如何对其进行编码 一般不用写
    application/x-www-form-urlencoded 表示在发送前编码所有字符（默认方式）
    multipart/form-data(<input type="file">) 表示不对字符编码 在使用包含文件长传控件的表单时，必须使用该值


 readyState
对象状态（integer），状态值
0 = 未初始化，未调用send()方法
1 = 读取中，已调用send()，正在发送请求
2 = 已读取，send方法执行完成，接收到全部响应内容
3 = 交互中，正在解析响应内容
4 = 完成，响应内容解析完成


responseText
获得字符串形式的相应数据

status
服务器返回的状态码
如：
404 = “文件未找到”
200 = “成功” 
500 = “服务器内部错误” 
304=“资源未被修改”

abort()
停止当前请求
