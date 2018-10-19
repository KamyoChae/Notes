你可以用正则表达式选择器的大写版本 来转化任何匹配。

举个例子：\s 匹配任何空白字符，\S 匹配任何非空白字符。

任务
用 /\S/g 来匹配字符串testString中的所有非空白字符。
```javascript
// 初始化变量
var testString = "How many non-space characters are there in this sentence?";

// 请只修改这条注释以下的代码

var expression = /\S/g;  // 请修改这一行

// 请只修改这条注释以上的代码

// 用 nonSpaceCount 存储 testString 中匹配到 expression 的次数
var nonSpaceCount = testString.match(expression).length;

```
