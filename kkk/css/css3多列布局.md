columns  多列布局

    为了能在Web页面中方便实现类似报纸、杂志那种多列排版的布局，
    W3C特意给CSS3增加了一个多列布局模块（CSS Multi Column Layout Module）。
    它主要应用在文本的多列布局方面，这种布局在报纸和杂志上都使用了几十年了，
    但要在Web页面上实现这样的效果还是有相当大的难度，庆幸的是，CSS3的多列布局可以轻松实现。

    语法：
    columns: [column-width] [column-count];
    column-width:   指每一列的宽度 根据容器宽度自适应 （最小宽度） 
    column-count:   指规定的列数 唯一精准的是列数
    不要column-width column-count一起使用 会乱

    column-gap:设置列与列之间的宽度，直接用数值表示即可(eg:10px) 

    column-gap主要用来设置列与列之间的间距  如果没有显示设置column-gap值时，其值大小会根据浏览器默认的font-size来定

    column-rule是不占用任何空间位置的，在列与列之间改变其宽度不会改变任何列的位置。
        column-rule-width: 宽度
            类似于border-width属性，主要用来定义列边框的宽度，其默认值为“medium”。
            column-	rule-width属性接受任意浮点数，但不接收负值。
            但也像border-width属性一样，可以使用关键词：medium、thick和thin。

        column-rule-style: 样式
            类似于border-style属性，主要用来定义列边框样式，其默认值为“none”。
            column-rule-style属性值与border-style属值相同，
            包括none、hidden、dotted、dashed、solid、double、groove、ridge、inset、outset	

        column-rule-color: 颜色
            类似于border-color属性

    column-span: 1/all 
    设置多列布局元素内的子元素，可以跨列，类似标题效果。即一个新闻标题要横跨所有内容列。
    注：此属性要在子元素上设置。



    hack1：

	假设现在列宽为195px,同时列之间的距离为0；而且他们之间没有任何样式
	此时两列的宽度为了填补列与列之间的空间，他们两列的宽度自动变成了变大 200px。
	大家还可以测试一下，当你把column-gap样式禁掉时，此时列只会显示一列，(因为加上gab 外部容器容纳不下了 所以变成了一列)前面也说过，
			 
        如果没有显示设置column-gap值时，其值大小会根据你所设置的font-size来定，
        因此容器无法容现两列的位置，从而第一列宽度扩展到容器大小一样			 
		 .colGapHack {
			width: 400px;
			border: 1px solid #008000;
			-moz-column-width: 195px;
			-webkit-column-width: 195px;
			column-width: 195px;
			-moz-column-gap: 0;
			-webkit-column-gap: 0;
			column-gap: 0;
			-moz-column-rule: 0 none; 
			-webkit-column-rule: 0 none;
			column-rule: 0 none;			 
		 }

    hack2：

	虽然设置的列宽大于元素容器的宽度(两列的宽度 加上默认的gap 大于 width)，但并不会让元素内容按列的宽度进行布局而撑破元素容器。他只会把列宽降到与元素容器宽度相等。
			
解决办法:
	column-width = (width-(n-1)*font-size)/n  
	/*其中n大于或等于2;并且其他值为默认值
			如当n为2 时 所设置的column-width 大于上面公式算出的值 那么 就会变成1列
			（Opera下最好再减1个px，当然如果你是中文的话也最好这样做，减1-2px，至于为什么，只有opera能解释清楚。） 
		 .colBigWidth {
			width: 400px;
			border: 1px solid #008000;
			-moz-column-width: 200px;
			-webkit-column-width: 200px;
			column-width: 200px;			 
		 }
