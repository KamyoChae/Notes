我们也可以使用正则表达式选择器 \s 来选择一个字符串中的空白。

空白字符有 " " (空格符)、\r (回车符)、\n (换行符)、\t (制表符) 和 \f (换页符)。

空白正则表达式类似于：

/\s+/g

任务
用 \s 选取句子中的所有空白字符。
```javascript
// 初始化变量
var testString = "How many spaces are there in this sentence?";

// 请只修改这条注释以下的代码

var expression = /\s+/g;  // 请修改这一行

// 请只修改这条注释以上的代码

// 用 spaceCount 存储 testString 中匹配到 expression 的次数
var spaceCount = testString.match(expression).length;

```
