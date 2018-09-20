为什么会有cookie？

Web应用程序使用HTTP协议传输数据。
HTTP协议是无状态的协议。
一旦数据交换完毕，客户端与服务器端的连接就会关闭，再次交换数据需要建立新的连接。这就意味着服务器无法从连接上跟踪会话。



每次请求
1、标记用户身份的http请求首部
    form：email
    user-agen 并不能识别出用户 只能识别浏览器
    referer 只能记录上一次浏览的网站

2、根据客户端ip地址识别
    ip不稳定

3、用户登录
    用户自己输入账号密码

4、胖url
    例如亚马逊 超链接 每次进入页面时，会在超链接后面添加特定到的标识码，一旦关闭窗口，标识码失效

5、cookie
    本地text文件


cookie建立过程：
第一步：浏览器发起请求 请求发送到服务器

第二步：服务器生成set-cookie 放在响应报文头 服务器返回 http
同时服务器标识用户 并记录该用户的浏览操作

第三步：浏览器接收http 获得set-cookie指令 设置cookie 断开响应

当第二次响应同样请求 浏览器端报文会添加上一次存储的cookie 一起发送给服务器
服务器获取报文cookie 进行匹配 在服务器端获取与该用户相关的所有信息 并给浏览器返回相关匹配值 

从服务器生成到浏览器获取，整个过程中 只有浏览器和服务器自动操作 前端不参与。因而广大网站使用广泛




cookie分两种 

会话cookie （或称临时cookie）
永久cookie 

cookie一般只用于标识 不做其他操作

设置cookie时 可以设置域名 当前域名及子域名都能拿到所设置的值
子域名不能拿到主域名cookie 主域可以拿到子域cookie

跨域拿cookie技巧：把同级的cookie设置成子域的cookie，可实现跨域取cookies



在当前页面出入document.cookie 
可获取当前域下所有cookie

设置cookie方法：


document.cookie = "age=10;max-age=1000";
document.cookie = "name=kam";

设置时间 
var oDate = new Date();
oDate.setDate(oDate.getDate()+3) // 设置三天后的时间

document.cookie = "expires="+oDate; 把cookie设置成三天后的时间
当时间超过所设置时间 cookie自动删除


设置路径
document.cookie = "path=/web";

path不设置 默认为当前路径 
若设置 则只能设置当前路径或上一级，不能设置下一级路径

expires默认为session
session代表临时 活动周期仅在当前窗口



封装cookie方法
var manageCookie = {
    setCookie:function(name, value, time){
        document.cookie = name + "=" + value + ";" + "max-age=" + time;
        return this;
    },
    removeCookie:function(name){
        return this.setCookie(name, "", -1)
    },
    getCookie:function(name, callback){
        var allCookieArr = document.cookie.split("; ");
        for(var i = 0; i < allCookieArr.length; i++){
            var itemCookieArr = allCookieArr[i].split("=");
            if(itemCookieArr[0] == name){
                callback(itemCookieArr[1]);
                return this;
            }
        }
        callback(undefined)
        return this;
    }
}

然而，Cookie实际上主要是web服务器开发人员设置的，前端开发人员较少使用cookie。但是我们得学会简单操作cookie


var foo = "123";

function print(){

    this.foo = 234;
    // 无new时 this指向window 有new时 this指向自身

    console.log(foo);
}
print();
