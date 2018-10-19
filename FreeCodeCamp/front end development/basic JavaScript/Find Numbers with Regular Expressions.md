我们可以在正则表达式中使用特殊选择器来选取特殊类型的值。

特殊选择器中的一种就是数字选择器\d，意思是被用来获取一个字符串的数字。

在JavaScript中, 数字选择器类似于: /\d/g。

在选择器后面添加一个加号标记(+)，例如：/\d+/g，它允许这个正则表达式匹配一个或更多数字。

尾部的g是'global'的简写，意思是允许这个正则表达式 找到所有的匹配而不是仅仅找到第一个匹配。

任务
用 \d 选择器来选取字符串中的所有数字。

```javascript
// 初始化变量
var testString = "There are 3 cats but 4 dogs.";

// 请只修改这条注释以下的代码

var expression = /\d+/g;  // 请修改这一行

// 请只修改这条注释以上的代码

// 用 digitCount 存储 testString 中匹配到 expression 的次数
var digitCount = testString.match(expression).length;

```
