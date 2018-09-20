

最早的Cookies自然是大家都知道，问题主要就是太小，大概也就4KB的样子，而且IE6只支持每个域名20个cookies，太少了。优势就是大家都支持，而且支持得还蛮好。

很早以前那些禁用cookies的用户也都慢慢的不存在了，就好像以前禁用javascript的用户不存在了一样。

userData是IE的东西，垃圾。

再之后Google推出了Gears，虽然没有限制，但不爽的地方就是要装额外的插件（没具体研究过）

到了HTML5把这些都统一了，官方建议是每个网站5MB，非常大了，就存些字符串，足够了。比较诡异的是居然所有支持的浏览器目前都采用的5MB，尽管有一些浏览器可以让用户设置，但对于网页制作者来说，目前的形势就5MB来考虑是比较妥当的。

# localStorage语法
 
1. window.localStorage.getItem( "key" ); 获取
2. window.localStorage.setItem( "key", value );
3. window.localStorage.removeItem( "key" );
4. window.localStorage.clear();
5. window.localStorage.length;
6. window.localStorage.key( i );
 

## localStorage.getItem("key")

获取本地存储中key的值

## localStorage.setItem("key", 555)
给key设置值555

## localStorage.removeItem( "key" )
在本地存储中移除key

## localStorage.clear()
清除所有本地存储

## localStorage.length 
查看本地存储中有多少个数据

## localStorage.key( i )
获取本地存储中的第i个数据



# Cookie、localStorage、sessionStorage三剑客对比

特性 | Cookie| localStorage | sessionStorage
---|--- | --- | ---
数据的生命期|由服务器产生，可设置失效时，如果浏览器生成Cookie，默认是关闭浏览器后失效 | 除非被清除，否则永久保存 | 只在当前会话有效，关闭页面浏览器会清除
存放数据大小|4K左右|一般为5MB| 一般为5MB
与服务器通信|每次都携带在http头中，如果使用cookie保存过多数据会带来性能问题 | 不参与服务器通信 | 不参与服务器通信
易用性|原生cookie接口不友好，需要自己封装 | 原生接口可已接受，可对Object、Array支持 | 原生接口可已接受，可对Object、Array支持




### 小总结：

这个api十分实用，我们可以用来做很多东西，比如存储一些笔记，存储一些游戏过程中的数据等等。
在我的另外两个demo中也用到了localStorage以及它的思想。

github仓库：

[这是我利用localStorage做的一款仿牛客app刷题H5小应用，点击查看](https://github.com/KamyoChae/webTest)

[这是我做的一款微信小程序应用，里面有个笔记的功能就是借鉴了localStorage的思想，点击查看](https://github.com/KamyoChae/Mini-Program_NiYuan)
