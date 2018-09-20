    css3新属性：

    1、border-radius: 圆角
        border-top-left-radius：50px水平轴 3px垂直轴

        border-radius:20px;

        设置单个圆角 左上 右上 右下 左下
        /border-radius:20px 20px 20px 20px;

        单独设置
        border-top-left-radius:20px;
        border-top-right-radius:20px;
        border-bottom-right-radius:20px;
        border-bottom-left-radius:20px;

        
        设置某个圆角的x轴半径 y轴半径
        border-top-left-radius:20px 20px;
        border-top-right-radius:20px 20px;
        border-bottom-right-radius:20px 20px;
        border-bottom-left-radius:20px 20px;




    2、box-shadow: 阴影
        box-shadow: X轴偏移量 Y轴偏移量 [阴影模糊半径] [阴影扩展半径] [阴影颜色] [投影方式];
        box-shadow:4px 2px 6px 7px #333333 inset;

        同一盒子，可以同时加多个阴影，阴影之间用“,”隔开
        box-shadow:4px 2px 6px #f00, -4px -2px 6px #000, 0px 0px 12px 5px #33CC00 inset;

    3、text-shadow文本阴影
        语法
        text-shadow:X-Offset Y-Offset blur color;
        X-Offset：表示阴影的水平偏移距离，其值为正值时阴影向右偏移，反之向左偏移；      
        Y-Offset：是指阴影的垂直偏移距离，如果其值是正值时，阴影向下偏移，反之向上偏移；
        Blur：是指阴影的模糊程度，其值不能是负值，如果值越大，阴影越模糊，反之阴影越清晰，如果不需要阴影模糊可以将Blur值设置为0；
        Color：是指阴影的颜色，其可以使用rgba色。
        比如，我们可以用下面代码实现设置阴影效果。
        text-shadow: 0 1px 1px #fff


    4、rgba颜色 ：
        RGB是一种色彩标准，是由红(R)、绿(G)、蓝(B)的变化以及相互叠加来得到各式各样的颜色。
        RGBA是在RGB的基础上增加了控制alpha透明度的参数。
        语法：
        color：rgba(R,G,B,A)
        以上R、G、B三个参数，正整数值的取值范围为：0 - 255。百分数值的取值范围为：0.0% - 100.0%。
        超出范围的数值将被截至其最接近的取值极限。
        并非所有浏览器都支持使用百分数值。A为透明度参数，取值在0~1之间，不可为负值。
        代码示例：
        background-color:rgba(100,120,60,0.5);


    5、渐变
        CSS3的渐变分为两种
        1）线性渐变（linear - to）
        语法: linear-gradient([direction], color [percent], color [percent], …)
        [] 内为选填
        direction角度的单位为 “deg” 也可以用to bottom, to left, to top left等的方式来表达

        2）径向渐变（radial - at）
        语法:radial-gradient(shape at position, color [percent] , color, …)
        shape:放射的形状，可以为原型circle，可以为椭圆ellipse
        position: 圆心位置，可以两个值，也可以一个
                    如果为一个时，第二个值默认center 即 50%。
                    值类型可以为，百分数，距离像素，也可以是方位值(left,top...); （x 轴主半径 y轴次半径）

    6、word-wrap：break-word 文字换行
    

    7、font-face：载入字体

        @font-face{
            font-family:”myFirstFont”;
            src:url('Sansation_Light.ttf'),
            url(‘Sansation_Light.eot') format(‘eot’)；
        }
        p{
             font-family:”myFristFont”;
        }

        format: 此值指的是自定义的字体的格式，
                主要用来帮助浏览器识别浏览器对@font-face的兼容问题，
                这里涉及到一个字体format的问题，
                因为不同的浏览器对字体格式支持是不一致的，
                浏览器自身也无法通过路径后缀来判断字体

        @font-face {
            font-family: 'diyfont';
            src: url('diyfont.eot'); /* IE9+ */
            src: url('diyfont.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
                 url('diyfont.woff') format('woff'), /* chrome、firefox */
                 url('diyfont.ttf') format('truetype'), /* chrome、firefox、opera、Safari, Android, iOS 4.2+*/
                 url('diyfont.svg#fontname') format('svg'); /* iOS 4.1- */


    8、border-img：边框图片

        border-image: url(xxx.png)  number 
        stretch 很好理解就是拉伸，有多长拉多长。有多远“滚”多远
        repeat (和4角上 同等大小图片进行平铺  当边框中间区域长度不是4角图片大小的整数倍时 会被切割)
        铺满(round)(4角上的图片 进行拉伸平铺  不会被切割)（共三个参数）

        number 为截取指定图片四周的宽度作为border的背景填充部分(截取图可按border-width 大小伸缩), number为一个数字时是复合写法
        最后一个属性为border-image的展示策略

        eg:border-image: url('./border.png') 27 round;


    9.背景图片起始位置background-origin

        语法：
        background-origin ： border-box | padding-box | content-box;
        参数分别表示背景图片是从边框，还是内边距（默认值），或者是内容区域开始显示。


    10.裁剪背景background-clip

        语法：
        background-clip ： border-box | padding-box | content-box | no-clip
        参数分别表示从边框、或内填充，或者内容区域向外裁剪背景。

        no-clip表示不裁切，和参数border-box显示同样的效果。
        background-clip默认值为border-box。

        text : background-clip : text ;
        从前景内容的形状（比如文字）作为裁剪区域向外裁剪，如此即可实现使用背景作为填充色之类的遮罩效果
        注意：webkit独有属性，且必须配合text-fill-color属性
        -webkit-background-clip:text;
        -webkit-text-fill-color:transparent;
        text-fill-color:-webkit-background-clip;
        -webkit-background-clip: text;


    11.背景图片尺寸background-size

        设置背景图片的大小，以长度值或百分比显示，还可以通过cover和contain来对图片进行伸缩。
        语法：
        background-size: auto | <长度值> | <百分比> | cover | contain
        取值说明：
        1、auto：默认值，不改变背景图片的原始高度和宽度；
        2、<长度值>：成对出现如200px 50px，将背景图片宽高依次设置为前面两个值，当设置一个值时，将其作为图片宽度值来等比缩放；
        3、<百分比>：0％~100％之间的任何值，将背景图片宽高依次设置为所在元素宽高乘以前面百分比得出的数值，当设置一个值时同上；
        4、cover：用一张图片铺满整个背景，如果比例不符，则截断图片
        5、contain：尽量让背景内，存在一整张图片











