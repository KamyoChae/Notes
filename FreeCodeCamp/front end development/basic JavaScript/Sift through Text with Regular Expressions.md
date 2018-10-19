Regular expressions 正则表达式被用来根据某种匹配模式来寻找strings中的某些单词。

举例：如果我们想要找到字符串The dog chased the cat中单词 the，我们可以使用下面的正则表达式: /the/gi

我们可以把这个正则表达式分成几段：

/ 是这个正则表达式的头部

the 是我们想要匹配的模式

/ 是这个正则表达式的尾部

g 代表着 global(全局)，意味着返回所有的匹配而不仅仅是第一个。

i 代表着忽略大小写，意思是当我们寻找匹配的字符串的时候忽略掉字母的大小写。

任务
用全局、忽略大小写的模式选取字符串 testString中所有的单词 and。

你可以尝试把 . 替换成 and。

```javascript
// 初始化变量
var testString = "Ada Lovelace and Charles Babbage designed the first computer and the software that would have run on it.";

// 举例
var expressionToGetSoftware = /software/gi;
var softwareCount = testString.match(expressionToGetSoftware).length;
  

// 请只修改这条注释以下的代码

var expression = /and/gi;  // 请修改这一行

// 请只修改这条注释以上的代码

// 用 andCount 存储 testString 中匹配到 expression 的次数
var andCount = testString.match(expression).length;


```
