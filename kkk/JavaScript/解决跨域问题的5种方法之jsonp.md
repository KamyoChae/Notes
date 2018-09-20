解决跨域问题的5种方法

1、flash （不做讨论）

2、服务器代理中转
    原理：用ajax向自己的服务器中A文件发起请求，A文件向B文件发起请求（A和B不同源），B文件返回给A，A返回给ajax。达到跨域获取数据的目的。

3、jsonp

4、document.domain(针对基础域名相同的情况)
在基础域名相同的情况下，分别在各自的脚本下设置document.domain，可解决跨域问题
例如：
bj.58.com  tj.58.com 域名不同
在各自脚本下设置 document.domain = '58.com' 即可


5、CORS跨域

原理：域名A(http://a.example)的某 Web 应用程序中通过<img>标签引入了域名B(http://b.foo)站点的某图片资源(http://b.foo/image.jpg)。这就是一个跨域请求，请求http报头包含Origin: http://a.example，如果返回的http报头包含响应头 Access-Control-Allow-Origin: http://a.example （或者Access-Control-Allow-Origin: http://a.example），表示域名B接受域名B下的请求，那么这个图片就运行被加载。否则表示拒绝接受请求


JSONP原理

1.拥有"src"属性的标签都拥有跨域的能力，比如<script>、<img>、<iframe>

2.把数据放到服务器上，并且数据为json形式（因为js可以轻松处理json数据）

3.当异步加载完成，做一个(load)处理。

4.实现定义好处理跨域获取数据的函数，如 function doJSON（data）{}。

5.用src获取数据的时候添加一个参数cb=‘doJSON’ (服务端会根据参数cb的值返回 对应的内容) 
  此内容为以cb对应的值doJSON为函数真实要传递的数据为函数的参数的一串字符 如 doJSON（’数据’）
