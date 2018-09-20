css3选择器

    E[att^=“val”] {…}  其属性值以val开头的任何字符串 选择匹配元素E, 且E元素定义了属性att

    E[att$=“val”]{…} 其属性值以val结尾的任何字符串 选择匹配元素E, 且E元素定义了属性att

    E[att*=“val”]{…} 其属性值任意位置出现了“val” 选择匹配元素E, 且E元素定义了属性att。即属性值包含了“val”，位置不限。



    初级伪类选择器：伪类用于向某些选择器添加特殊的效果。
    伪类的效果可以通过添加一个实际的类来达到。
    伪元素的效果则需要通过添加一个实际的元素才能达到。
    这也是为什么他们一个称为伪类，一个称为伪元素的原因。
    https://segmentfault.com/a/1190000000484493

    1.root 根标签选择器

        “:root”选择器等同于<html>元素，简单点说：

        :root{background:orange}
        html{background:orange}
        得到的效果等同
        建议使用:root（xml等）

    2.:not 否定选择器
        用法和jQuery 中的not类似，可以排除某些特定条件的元素
        div:not([class=“demo”]){
                background-color:red;
        }

        意思为除了class为demo的div以外，所有的div的背景颜色都变红



    3.empty 空标签选择器
        用来选择没有内容的元素、不在文档树中的元素，这里的没有内容指的是一点内容都没有，哪怕是一个空格。

    4.target 目标元素选择器
        用来匹配被location.hash 选中的元素(即锚点元素)
        选择器可用于选取当前活动的目标元素

    5.:first-child 第一个子元素
        :last-child 最后一个子元素
        :nth-child(){} 第xxx个子元素，n代表变量自然数
        :nth-last-child(){}  从后往前数
        以上四个选择器均有弊端，即如果当前位置元素不是前面所修饰的元素，那么无效
        注：其父元素的第 N 个子元素，不论元素的类型。
    
    6.:first-of-type 第一个子元素
        :last-of-type 最后一个子元素
        :nth-of-type(){} 第xxx个子元素，n代表变量自然数
        :nth-last-of-type(){}  从后往前数

        此种选择器，限制了类型，即在所修饰元素的类型下选择特定位置的元素。

    7 :only-child  唯一子元素选择器
        选择是独生子的子元素，即该子元素不能有兄弟元素，它的父元素只有他一个直接子元素。
        注意：选择的元素是独生子子元素，而非有唯一子元素的父元素。
        
        :only-of-type
        如果要选择第某类特定的子元素(p) 在兄弟节点中是此类元素唯一个的话 就需要用到这个属性了

    8 :enabled  可用的元素
        :disabled 不可用的元素
        在web的表单中，有些表单元素有可用（“enabled”）和不可用（“disabled”）状态，
        比如输入框，密码框，复选框等。
        在默认情况下，这些表单元素都处在可用状态。
        那么我们可以通过伪类选择器 enabled 进行选择，disabled则相反。

    9:checked选择框的被选中状态
        注：checkbox, radio 的一些默认状态不可用属性进行改变，如边框颜色。

    10:read-only  选中只读的元素
        eg:<input type=“text” readonly=“readonly”/>

        :read-write 选中非只读的元素
        eg:<input type=“text”/>

    CSS3对伪元素进行了一定的调整，在以前的基础上增加了一个:也就是现在变成了::first-letter,::first-line,::before,::after
        另外还增加了一个::selection
        1.::selection

        “::selection” 选择器是用来匹配突出显示的文本（用鼠标选择文本的时候）。
        浏览器默认情况下，用鼠标选择网页文本是以“蓝色的北京，白色的字体”显示的。
        属性：user-select: none;

        注：火狐下必须加-moz-
        -moz-::selection

    条件选择器
        E > F  an F element child of an E element
        直接子元素
        E + F an F element immediately preceded by an E element 
        后面的紧挨着的兄弟节点
        E ~ F an F element preceded by an E element
        后面的兄弟节点
