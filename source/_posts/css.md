---
title: css
date: 2024-03-15 21:03:14
tags:
---

# css使用技巧和部分问题解决

> margin/padding使用%为单位时，会默认以父元素的宽度作为参考，即使你设置的是padding-top和padding-bottom，也会以父元素的宽作为参考，==这也是为什么缩小浏览器宽度里面的子元素会发生变动==
>
> 应该一开始设置好最大的长宽(用整数px设置)，避免出现小数
>
> 
>
> 控制单行文本且多出的文字为省略号
>
> >  text-overflow: ellipsis; // 多出的文字用省略号显示
> >
> >  overflow: hidden; //溢出盒子的部分隐藏
> >
> >  white-space: nowrap; //文本不转行
>
> 多行文本显示省略号:(有较大兼容性问题，但用也无妨，最好让后台人员来实现)
>
> > overflow: hidden;
> >
> > text-overflow: ellipsis;
> >
> > /* 弹性伸缩盒子模型显示 */
> >
> > display: -webkit-box;
> >
> > /* 限制在一个块元素内显示的文本行数 */
> >
> > -webkit-line-clamp: 2;
> >
> > /* 设置或者检索伸缩盒子对象的子元素的排列方式 */
> >
> > -webkit-box-orient: vertical;
> >
> > (一般选择2 两行，若想在第n行显示省略号，只需要将-webkit-line-clamp: 2;中的2改为相应的n即可)
> >
> > (注意若限制的文本行数为n但实际文本超出了n行则还是会显示n行后的的文字，所以应提前设计好文本框的高，譬如限制的文本行数为n，则应该将文本框盒子的高调到刚好只够n行文字显示的宽高，==或者是用行内元素让文字把高撑开，又因为上面的设置只有两行，那么就完美的变成两行了==)
>
> ==文本的最好使用行内元素包裹，并通过上面的css设定显示几行，这样能刚好撑高元素的高，不必手动去调==
>
> 
>
> 父元素设置了圆角，但是子元素没有设置圆角，子元素的直角遮挡了父元素的圆角
>
> > 解决办法：在父元素上添加 overflow：auto
>
> 
>
> ==相同rgb但a不同算相同颜色，transition无效，会将文本消失==
>
> 
>
> div指定宽高后放background-image，调整样式
>
> > ```
> > display: inline-block;
> >     background-position: center center;
> >     background-size: 100% 100%;
> > ```
>
> vh、vw和%的区别
>
> > vh根据浏览器的视口的高度变化，vw根据浏览器的视口的宽度变化，将视口的长宽分别分为100份。1vh就是视口的1%，vw同理
> >
> > 添加overflow: auto;根据浏览器的视口的变化显示滚动条，添加::-webkit-scrollbar {display: none;}可以隐藏滚动条
> >
> > %则需要根据父盒子的长宽来决定
> >
> > https://blog.csdn.net/weixin_43958820/article/details/118544081
>
> 
>
> margin取负值时就是反向移动，例如margin-left取正值时是向右移动x，取负值就是向左移动x
>
> 
>
> 选择器优先级从高到低
>
> > ==!important==
> >
> > ==内联样式==
> >
> > ==id选择器== 100
> >
> > ==类class选择器==、伪类选择器、属性选择器 10
> >
> > ==标签选择器==、伪元素选择器 1
> >
> > 通配符、子类选择器、兄弟选择器 、后代选择器 （* > + 空格）0
> >
> > 继承
>
> 
>
> 子绝父相后将子z-index: 1可以将其被其他元素压住
>
> 
>
> 定义svg颜色用fill
>
> 
>
> 父元素display: flex后最后一个子元素margin-left: auto即可实现最后一个元素紧靠最右边
>
> > [flex 右对齐_Flex布局使用 Auto Margin 对齐_麦丽素达人朱厚熜的博客-CSDN博客](https://blog.csdn.net/weixin_42471590/article/details/113072184)
> >
> > 需要注意的是，假设同时将最后一个子元素margin-left: auto和将div(==最后一个子元素是div==)设为display: flex则margin失效
> >
> > ```css
> > <div>
> > <div>1 </div>
> > <div>2 </div>
> > <div class='third'>3 </div>
> > </div>
> > 
> > .third {
> >   margin-left: auto;
> > }
> > 
> > div {
> >   display: flex;
> >   align-items: center;
> > }
> > // 这样margin失效
> > ```
> >
> > ```css
> > <div>
> > <div>1 </div>
> > <div>2 </div>
> > <div class='third'>3 </div>
> > </div>
> > 
> > .third {
> >   margin-left: auto;
> >   border: 1px solid #fff;
> >   border-radius: 3vw;
> >   display: flex;
> >   align-items: center;
> > }
> > // 这样margin有效
> > ```

# CSS参考文档: [CSS 参考 - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference)

# 一个属性有多个属性值，多个属性值之间应该用空格空开

# 选择器(标签)和大括号之间应保留空格

# 属性值前面，冒号后面应保留一个空格

# 元素选择器(标签选择器)

> ```
> span {
>   background-color: DodgerBlue;
>   color: #ffffff;
> }
> ```
>
> 作用于下面的html
>
> ```
> <span>这里是由 span 包裹的一些文字.</span>
> ```

# 类选择器class(class可以设定多个值 每个值之间要空格隔开 每个值都可以用来选择该标签 可以达到多类名选择的效果)

> ```
> .classy {
>   background-color: DodgerBlue;
> }
> ```
>
> 作用于下面的html
>
> ```
> <span class="classy">Here's a span with some text.</span>
> ```

# id选择器 id  一般和js搭配使用 一次只能使用一个标签

> ```
> #identified {
>   background-color: DodgerBlue;
> }
> ```
>
> 作用于下面的html
>
> ```
> <span id="identified">Here's a span with some text.</span>
> ```

# 通配符选择器 * 选择所有 性能最低 不推荐使用 只在特定情况下使用(优先级最低)

> ```
> *[lang^=en]{color:green;}
> *.warning {color:red;}
> *#maincontent {border: 1px solid blue;}
> ```
>
> 作用于下面的html
>
> ```
> <p class="warning">
>   <span lang="en-us">A green span</span> in a red paragraph.
> </p>
> <p id="maincontent" lang="en-gb">
>   <span class="warning">A red span</span> in a green paragraph.
> </p>
> ```

# 属性选择器(元素选择器的一种变换使用方法)

> ```
> /* 存在title属性的<a> 元素 */
> a[title] {
>   color: purple;
> }
> 
> /* 以 "#" 开头的页面本地链接 */
> a[href^="#"] {
>   background-color: gold;
> }
> 
> /* 存在href属性并且属性值匹配"https://example.org"的<a> 元素 */
> a[href="https://example.org"] {
>   color: green;
> }
> 
> /* 存在href属性并且属性值包含"example"的<a> 元素 */
> a[href*="example"] {
>   font-size: 2em;
> }
> 
> /* 存在href属性并且属性值结尾是".org"的<a> 元素 */
> a[href$=".org"] {
>   font-style: italic;
> }
> 
> /* 存在class属性并且属性值包含以空格分隔的"logo"的<a>元素 */
> a[class~="logo"] {
>   padding: 2px;
> }
> ```
>
> [属性选择器 - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Attribute_selectors)

# 组合选择器: 后代选择器: (元素选择器的叠加，权重叠加)

> ![img](images/4@O6EKO4Y%7BVV1YX%7BVVW%5D56K.png)
>
> 事实上可以有元素1 元素2 元素3...直到元素n然后加上{样式声明} 且元素2-n都是元素1的子元素 但只有元素n也就是样式声明前一个元素受到样式声明的影响，且对所有级别的元素n都产生影响(例如元素n既存在于第二级又存在于第三级，该样式声明既对第二级和第三级的元素n同时产生影响)
>
> 详细例子见1:30左右: [黑马程序员pink老师前端入门教程，零基础必看的h5(html5)+css3+移动端前端视频教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14J4114768?p=99)
>
> 元素1也可以是其他元素(假设存在元素0)的子元素，但不可以是元素2-n的子元素
>
> [后代选择器 - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Descendant_combinator)

# 组合选择器: 子选择器 :(元素选择器的叠加，权重叠加)

> ![img](images/1%7B_KK%7D9JT2GCL%5DN26O45$EJ.png)
>
> 子选择器必须按照严格的亲子顺序来排列，不可以跨级，即父元素下一个必须是子元素不能是孙，也不能是重孙和更下级的元素，且该样式声明只对元素1作为父元素、元素2作为元素1的子元素条件时对该元素2产生影响，对于其他父元素下的子元素元素2不产生影响
>
> 详细例子见2:00左右: [黑马程序员pink老师前端入门教程，零基础必看的h5(html5)+css3+移动端前端视频教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14J4114768?p=99)
>
> 但如若有多个父元素子元素的关系符合上述要求。则所有这些都受到影响，例如
>
> li>a { color: red; }
>
>  对
>
> `<li><a href="#">子</a></li>`
>
> `<li><a href="#">子</a></li>`
>
> 都产生影响
>
> 同样的元素1可以是其他元素(假设存在元素0)的子元素，但不能是元素2-n的子元素

# 选择器列表/并集选择器: (元素选择器的并列，权重各算各的)

>   ```
>   element1, element2, element3 { style properties }
>   ```
>
>   元素1和元素2和元素3中间用，隔开
>
>   任何形式的选择器都可以成为选择器列表/并集选择器的一部分
>
>   选择器列表无效化: 用选择器列表进行样式声明与将每个元素分别进行一次样式声明并不是等价的，这是因为，在选择器列表中如果有一个选择器不被支持(例如没有支持的字体系列)，==那么整条规则都将会失效。==可以使用:is()==选择器来解决，但这样的代价是其中所有的选择器都会拥有相同的优先级
>
>   [选择器列表 - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Selector_list)

# 伪类选择器: (元素选择器和伪类选择器的叠加，权重也叠加)

> [伪类 - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes)
>
> 实际上伪类选择器的:前面可以是任意元素，甚至可以为空，当然要视不同的伪类选择器来选择对应的:前面的元素
>
> a:link 选择所有未被访问的链接
>
> a:visited 选择所有已被访问过的链接
>
> a:hover 选择鼠标指针位于其上的链接
>
> a:active 选择活动链接(鼠标按下未弹起的链接)
>
> 按照lvha的顺序写(爱恨原则) 实际生产中一般只需要a:hover
>
> input:focus {color: red;background: yellow;} 选择被光标选中的输入框
>
> ==没有href属性则link和visited失效，有href属性但是内容为空则visited有效，link失效，如果有href属性且内容不为空则link有效==

# font-family :定义字体系列

> ```
>     <style>
>         textarea {
>             font-family: 'Times New Roman', Times, serif;
>         }
>     </style>
> ```
>
> 微软雅黑=Microsoft Yahei
>
> 字体选择有Times New Roman时选择Times New Roman 没有则顺延至Times 再顺延至serif 优先级从左到右

# font-size :自定义字体大小 单位为像素px

> ```
> /* <absolute-size>，绝对大小值 */
> font-size: xx-small;
> font-size: x-small;
> font-size: small;
> font-size: medium;
> font-size: large;
> font-size: x-large;
> font-size: xx-large;
> 
> /* <relative-size>，相对大小值 */
> font-size: larger;
> font-size: smaller;
> 
> /* <length>，长度值 */
> font-size: 12px;
> font-size: 0.8em;
> 
> /* <percentage>，百分比值 */
> font-size: 80%;
> 
> font-size: inherit;
> ```
>
> 谷歌浏览器的默认字体大小为16px
>
> ### [Em](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-size#ems)
>
> 另一种方法是用`em`值设定字体大小。`em` 值的大小是动态的。当定义或继承font-size属性时，1em等于该元素的字体大小。如果你在网页中任何地方都没有设置文字大小的话，那它将等于浏览器默认文字大小，通常是16px。所以通常1em = 16px。2em = 32px。 如果你设置了body元素的字体大小为20px，那1em = 20px、2em = 40px。那个2就是当前em大小的倍数。
>
> 可用这个公式计算像素大小的等价em大小：
>
> ```
> em = 希望得到的像素大小 / 父元素字体像素大小
> ```
>
> 例如，假设body的文字大小设为1em，且浏览器默认1em = 16px。如果你想得到12px，那你就可设定0.75em (because 12/16 = 0.75)。同样的，如果你想设定文字大小为10px，那就定义0.625em (10/16 = 0.625)；若想要22px就定义1.375em (22/16=1.375).
>
> 一个流行的技巧是设置body元素的字体大小为62.5% (即默认大小16px的62.5%)，等于10px。现在你可以通过计算基准大小10px的倍数，在任何元素上方便的使用em单位。这样有6px = 0.6em, 8px = 0.8em, 12px = 1.2em, 14px = 1.4em, 16px = 1.6em。例如：
>
> ```
> body {
>   font-size: 62.5%; /* font-size 1em = 10px */
> }
> p {
>   font-size: 1.6em; /* 1.6em = 16px */
> }
> ```
>
> 因为em可以自动适应用户的字体，em是一个非常有用的CSS单位。
>
> 最好使用用户默认字体大小的相对大小，避免使用除了em或ex以外的绝对单位，如果一定要使用绝对单位的话，px是最好的单位，因为其含义不会随着操作系统所认为的屏幕分辨率的大小改变而改变。

# font-weight :自定义字体粗细程度

> ```
> p {
> 
>    font-weight: normal
> 
> }
> ```
>
> normal
>
> 正常粗细。与`400等值。`
>
> bold
>
>  加粗。 与`700等值。`
>
> lighter
>
> 比从父元素继承来的值更细(处在字体可行的粗细值范围内)。
>
> bolder
>
> 比从父元素继承来的值更粗 (处在字体可行的粗细值范围内)。
>
> <number>
>
> 一个介于 1 和 1000 (包含) 之间的 number 类型值。更大的数值代表字体重量粗于更小的数值 (或一样粗)。一些常用的数值对应于通用的字体重量名称，如章节[常见粗细值名称和数值对应](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-weight#常见粗细值名称和数值对应)所描述。

# font-style :自定义字体style

> ```
> font-style: normal;
> font-style: italic;
> font-style: oblique;
> font-style: oblique 10deg;
> 
> /* Global values */
> font-style: inherit;
> font-style: initial;
> font-style: unset;
> ```
>
> normal
>
> 选择常规字体。
>
> italic
>
> 选择斜体，如果当前字体没有可用的斜体版本，会选用倾斜体（`oblique` ）替代。
>
> oblique
>
> 选择倾斜体，如果当前字体没有可用的倾斜体版本，会选用斜体（`italic` ）替代。
>
> [font-style - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-style)

# font复合属性写法 :

> font: italic small-caps bold 16px/2 cursive;
>
> /* font-style font-variant font-weight font-size/line-height font-family */
>
> line-weight:计算文本行间距[line-height - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/line-height)
>
> font-variant:[font-variant - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-variant)
>
> ==复合属性除了font-size和font-family以外都可以省略，否则不产生效果==
>
> ==复合属性必须按照上述顺序进行顺序书写，否则不产生效果==
>
> 应尽量使用复合写法

# color :自定义文本颜色

> ```
> /* 关键词 */
> color: currentcolor;
> 
> /* <named-color>值 */
> color: red;
> color: orange;
> color: tan;
> color: rebeccapurple;
> 
> /* <hex-color>值 */
> color: #090;
> color: #009900;
> color: #090a;
> color: #009900aa;
> 
> /* <rgb()>值 */
> color: rgb(34, 12, 64, 0.6);
> color: rgba(34, 12, 64, 0.6);
> color: rgb(34 12 64 / 0.6);
> color: rgba(34 12 64 / 0.3);
> color: rgb(34.0 12 64 / 60%);
> color: rgba(34.6 12 64 / 30%);
> 
> /* <hsl()>值 */
> color: hsl(30, 100%, 50%, 0.6);
> color: hsla(30, 100%, 50%, 0.6);
> color: hsl(30 100% 50% / 0.6);
> color: hsla(30 100% 50% / 0.6);
> color: hsl(30.0 100% 50% / 60%);
> color: hsla(30.2 100% 50% / 60%);
> 
> /* 全局值 */
> color: inherit;
> color: initial;
> color: unset;
> ```
>
> 十六进制的写法最为常用(因为可以用PS吸取颜色的16进制值)

# text-align: 行内内容(如文字)对齐

> text-align: left;左对齐
>
> text-align: center;居中对齐
>
> text-align: right;右对齐
>
> [text-align - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-align)

# text-decoration: 文本装饰

> ```
> <'text-decoration-line'> || <'text-decoration-style'> || <'text-decoration-color'> || <'text-decoration-thickness'>
> ```
>
> 具体例子见下链接
>
> [text-decoration - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration)
>
> 以上属性无顺序规则
>
> 当给a标签添加text-decoration: none时可以去掉a标签本身自带的下划线
>
> text-decoration-line: [text-decoration-line - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-line)
>
> text-decoration-color: [text-decoration-color - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-color)
>
> text-decoration-style: [text-decoration-style - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-style)
>
> text-decoration-thickness: [文本装饰线厚度(粗细) - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration-thickness)

# text-indent: 文本首行缩进

> text-indent: 2em首行缩进2个默认字符大小 em详情见上文font-size中
>
> text-indent: 32px首行缩进32px距离 因为浏览器默认字符大小为16px 所以缩进2个字符大小
>
> text-indent: 15%首行缩进包含块宽度15%的距离 不常用
>
> [text-indent - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-indent)

# line-height: 行高

> 行高包括文本高度、上间距以及下间距
>
> ![](images/image-20220225123917335.png)
>
> 上一行与这一行之间的空白高度=上一行的下间距+这一行的上间距
>
> ```
> /* Keyword value */
> line-height: normal;
> 
> /* Unitless values: use this number multiplied
> by the element's font size */
> line-height: 3.5;
> 
> /* <length> values */
> line-height: 3em;
> 
> /* <percentage> values */
> line-height: 34%;
> 
> /* Global values */
> line-height: inherit;
> line-height: initial;
> line-height: unset;
> ```
>
> normal
>
> 取决于用户端。桌面浏览器（包括Firefox）使用默认值，约为`1.2`，这取决于元素的 `font-family`。
>
> <数字>
>
> 该属性的应用值是这个无单位数字[`<数字>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/number)乘以该元素的字体大小。计算值与指定值相同。大多数情况下，这是设置`line-height`的**推荐方法**，不会在继承时产生不确定的结果。
>
> <长度>
>
> 指定[`<长度>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length)用于计算 line box 的高度。参考[`<长度>`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length)了解可使用的单位。以 **em** 为单位的值可能会产生不确定的结果（见下面的例子）。
>
> <百分比>
>
> 与元素自身的字体大小有关。计算值是给定的百分比值乘以元素计算出的字体大小。**百分比**值可能会带来不确定的结果（见下面第二个例子）。
>
> 具体说明: 
>
> 无单位数字是该元素字体的本身大小乘以该数字得到的行间距，该元素数字的本身大小不受父元素的字体大小的影响
>
> em为单位的长度则是受父元素的字体大小的影响后再乘上数字得到的行间距
>
> 可使用以下链接中的最后一个例子通过更改.green和.red的line-height以及h1和.box的font-size来进行验证
>
> [line-height - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/line-height)

# 外部样式表:  

> `<link rel="stylesheet" href="css文件路径">`

# emmet快速生成css

> ![img](images/%25J5%60H0N3@5IXN55OL5DI%5DEO.png)

# shift+alt+f或者右键点击空白区域选择即可格式化代码 让代码看起来更加规范

# 块元素: 

> ![img](images/ZD15J5L4%7DG9O%25TO25L%608%7B%5DR.png)

# 行内元素:

> ![image-20220224231527816](images/image-20220224231527816.png)

# 行内块元素:

> ![image-20220224231915140](images/image-20220224231915140.png)

# 显示模式的互相转换: 

> diplay:block;    转换为块元素，还有显示元素的作用
>
> display:inline;    转换为行内元素
>
> display:inline-block;     转化为行内块元素

# 文字垂直居中的小技巧:

> 将行高line-height设为和块元素的height相同的数值即可，行高的上间距和下间距相等，会自动将文本压缩到正中间，以实现垂直居中的视觉效果(进阶见下vertical-align: middle;)

# 背景颜色background-color::

> background-color: transparent;将背景颜色设置为透明
>
> background-color: rgba(red, green, blue, alpha);alpha是一个0-1的值，用以控制颜色的透明度，
>
> 0是完全透明(transparent),1是完全不透明

# 背景图片background-image:

> ![image-20220225124930332](images/image-20220225124930332.png)
>
> 背景颜色和背景图片同时存在会导致背景图片压住背景颜色

# 背景平铺background-repeat: 

> 背景图片默认设置为平铺(即复制后铺满整个平面)
>
> ![image-20220225125714397](images/image-20220225125714397.png)
>
> 当选择no-repeat时，只显示一个图片且图片默认靠近盒子的左上方

# 背景图片位置background-position:

> ![image-20220225131152079](images/image-20220225131152079.png)
>
> 当使用方位名词时，不区分前后顺序，且如果只写一个方位名词，另外一个默认为center。(此方法为粗略方法)
>
> (实际上这里也可以理解为center right是x的center和y的right，right center是x的right和y的center，这两者表达的意思都是一致的，这样表现出来的形式让人觉得可以不分前后顺序)
>
> 
>
> 当图片过大时应使用背景图片位置并将它设置成为center top已展示图片中最为核心的内容
>
> 
>
> 当使用精确的以px为单位的精确数值时，x y的顺序不可颠倒，且x(y)代表图片的左(右)边界与容器的左(右)边界的距离
>
> 
>
> 当使用百分比代替以px为单位的精确数值时，百分比值的偏移指定图片的相对位置和容器的相对位置重合。值0%代表图片的左边界（或上边界）和容器的左边界（上边界）重合。值100%代表图片的右边界（或下边界）和容器的右边界（或下边界）重合。值50%则代表图片的中点和容器的中点重合。另外需要注意，如果背景图片的大小和容器一样，那设置百分比的值将永远无效，因为“容器和图片的差”为0（图片永远填满容器，并且图片的相对位置和容器的相对位置永远重合）。在这种情况下，需要为偏移使用绝对值（例如px）。
>
> 
>
> 另外也可使用方位名词和精确数值的混合使用方法，例如x用精确数值而y用方位名词，同样此方法也不能颠倒x y的顺序

# 背景固定background-attchment:

> background-attachment: scroll 相对于元素的边框固定 背景图像相对于元素本身(即盒子/容器)固定 即使元素(即盒子/容器)内的文字发生滚动了 背景图像也不会发生滚动是默认效果
>
> 
>
> background-attachment: local  背景图像相对于对象内容固定，如果对象内容(如文字)滚动则相应发生滚动
>
> 
>
> background-attachment: fixed 背景图像固定(相对于视口，无论你的鼠标怎么上下滑动，该图片都原封不动地在页面上显示)(多见于恶心弹窗广告)
>
> 
>
> 多背景图支持: 此属性支持多张背景图片，可以用逗号分开为每一张图片制定不同的attachment属性。但要对应相同的顺序。如下例子fixed对应startsolid.gif、scroll对应startransparent.gif
>
> ```
> p {
>   background-image: url("https://mdn.mozillademos.org/files/12057/starsolid.gif"), url("https://mdn.mozillademos.org/files/12059/startransparent.gif");
>   background-attachment: fixed, scroll;
>   background-repeat: no-repeat, repeat-y;
> }
> ```
>
> 
>
> [background-attachment - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-attachment)
>
> (scroll、local和fixed容易发生混淆，应多加理解)

# 背景图片大小background-size: 

> background-size: x y;            x代表宽度 y代表高度
>
> background-size: cover;         背景图片等比例放大直到cover住整个父盒子
>
> background-size: contain;       背景图片等比例放大直到长(宽)和父盒子的长(宽)达到边界

# 背景属性复合写法；

> 类似于font的复合写法，可以简化代码，且background的符合写法没有属性的顺序要求
>
> 应尽量使用复合写法

# CSS的层叠性:

> CSS文件从上到下运行，n个相同选择器的相同属性有不同的属性值，那么在最后面的这个属性值会将前面的所有其他相同选择器的相同属性的不同属性值覆盖掉，所以只有最后面的这个属性值起作用。
>
> (此规则只在选择器的权重相同时起作用)

# CSS的继承性:

> 子元素继承父元素的属性，主要表现在关于文字的一些属性上，例如text-、font-、line-以及color属性(具体是否继承请查阅mdn文档)
>
> 行高的继承性见上文line-height

# CSS的优先级:

> ![image-20220226144038174](images/image-20220226144038174.png)
>
> 除了行内样式和!important之外，越高级的选择器优先级越高，范围广的样式应用低级选择器，范围小的样式应用高级选择器
>
> 永远不要使用!important 使用它是一个非常坏的习惯
>
> ==继承的权重最低为0==，只要对继承的属性值进行了修改，继承的属性值就不会起作用
>
> 在某些浏览器设置了默认值的属性上，给该属性的父元素设置属性值不会影响到子元素。例如a标签，浏览器默认值为蓝色加下划线，将a标签的父元素设置为其他颜色并不会影响到a标签的颜色。
>
> ==权重的叠加不会进位==
>
> ==一般只在有后代选择器的情况下需要计算权重==

# 盒子模型(border/padding/margin/外边距合并-嵌套块元素塌陷/清除内外边距): 

> ![image-20220226193254025](images/image-20220226193254025.png)
>
> ==盒子的两个border的平行边框的内边之间的距离才是盒子的宽或高，所以padding会把盒子撑开==
>
> ==但是子盒子在父盒子里面所能达到的最大长宽是父盒子两个padding的平行内边框之间的距离==
>
> 
>
> 
>
> (border、padding、margin系列的属性的给值顺序都符合下列规则)
>
> 给定一个值,该值作用于所有边框
>
> 给定两个，从左到右分别作用于横边框和纵边框
>
> 给定三个，上横边、纵边、下横边
>
> 给定四个，上、右、下、左
>
> 
>
> border:
>
> border-width:定义边框粗细 
>
> > thin medium thick并没有给定的数值，因此在不同浏览器中三者的视觉效果不尽相同
> >
> > ==盒子边框的宽度不在盒子的范围内，盒子的两个平行内边框之间的距离则为盒子的宽或高，盒子内边框到相邻盒子外边框的距离则为边框的宽度，也就是border-width的数值==
> >
> > [border-width - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-width)
>
> border-style:定义边框形式
>
> > 默认为none
> >
> > solid 实线
> >
> > dashed 虚线
> >
> > dotted 圆点线
> >
> > [border-style - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-style)
>
> border-color: 定义边框颜色
>
> > [border-color - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-color)
>
> border的复合写法:
>
> > 可以为任意顺序，例如
> >
> > border: medium dashed green;
> >
> > border: green medium dashed;
> >
> > border:  dashed green medium;
> >
> > ...等6种写法效果都是一样的
> >
> > 用border的复合写法时只接受三个参数，也就是用此写法时不可以在一句代码里分别对四个边框做出不同的样式设定，但是可以在多句代码里利用复合写法分别对四个边框进行样式设定，例如
> >
> > border-top: color style width
> >
> > border-right: color style width
> >
> > border-bottom: color style width
> >
> > border-left: color style width
> >
> > 同样的在采用上面四种复合写法时，也可以任意顺序来赋值
>
> padding:
>
> > 内边距，content内容和盒子边框内边之间的距离
> >
> > 可以使用padding-top、padding-right、padding-bottom和padding-left分别赋值
> >
> > 也可以使用复合写法，顺序规则见上
> >
> > ==内边距的宽度在盒子范围内，会撑开盒子的范围，假设给定了盒子的width和height都为x，再给定padding都为y，那么实际上盒子的width和height都将变为x+2y==
> >
> > padding不会撑开盒子的情况:
> >
> > ==盒子的width/height没有给定值(不论盒子的width/height是默认的还是继承的)，则padding不会撑开盒子==
> >
> > ==但一旦盒子的width/height给定了值(不论是具体的带单位值还是百分比)，则padding会撑开盒子==
> >
> > [padding - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding)
>
> margin:
>
> > 外边距，边框外边和其他盒子边框外边的距离
> >
> > 可以使用margin-top、margin-right、margin-bottom和margin-left分别赋值
> >
> > 也可以使用复合写法，顺序规则见上
> >
> > ==为使盒子水平居中，需要让盒子指定了width，并让其左右外边距都设置为auto，有三种写法:==
> >
> > margin-left: auto;margin-right: auto;
> >
> > margin: auto;
> >
> > margin: 0 auto;
> >
> > 其中margin: auto;和margin: 0 auto;表意完全一致，都为上下外边距为0，左右外边距为auto
> >
> > ==以上方法适用于盒子模型(块级元素)，对于行内元素和行内块元素水平居中给其父元素添加text-align: center;即可==
> >
> > [margin - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin)
>
> 外边距合并-嵌套块元素塌陷:
>
> > 当上下两个块元素(兄弟关系)相遇时，如果上面的块元素有下外边距margin-bottom，下面的元素有上外边距margin-top，则它们之间的垂直间距不是margin-bottom和margin-top的和，而是取两者之间较大值为垂直间距(==当margin都是整数时，当有一方margin为负数，那么间距就是正数加负数==)
> >
> > ==解决方案: 尽量只给一个盒子设置margin==
>
> > 当两个嵌套元素(父子关系)的块元素，父元素有上外边距同时子元素也有上外边距，此时父元素会取较大的外边距值进行布局，称之为坍缩现象
> >
> > 解决方案:
> >
> > 为父元素定义上边框border
> >
> > 为父元素定义上内边距padding
> >
> > ==为父元素添加overflow: hidden(此方案是最佳方案)==
> >
> > ==脱标(绝对定位、固定定位、浮动元素)的元素不会发生塌陷==
>
> 清除内外边距:
>
> > ==许多网页元素都自带默认的内外边距数值，不同浏览器的默认值也不尽相同，在布局前应清除网页元素的默认内外边距:==
> >
> > ```
> >     * {
> >             padding: 0;
> >             margin: 0;
> >         }
> > ```
> >
> > 行内元素为了照顾兼容性，尽量只设置左右内外边距，不要设置上下内外边距(反正也不起作用)，如果确实需要设置上下内外边距则应转换为块级元素和行内块级元素

# 表格边框:

> border-collapse: 用来决定表格table的单元格的边框是否合并，是属于th和td的属性
>
> collapse 相邻的单元格共用一条边框 (一个单元格边框的厚度)
>
> separate 每个单元格拥有独立的边框 (两个单元格边框之间存在缝隙)
>
> 默认时相邻单元格的边框紧贴在一起(即两个单元格边框的厚度)
>
> [border-collapse - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-collapse)

# 圆角边框border-radius: (后可接具体带单位数值和百分比)(border-radius: 50%;可以得到半径为原正方形盒子的边长的一半的圆)

> border-radius: 将盒子的四个角分别用具体值为半径的圆的弧形替代
>
> border-radius: 10px;将盒子的四个角分别用10px半径的圆的弧线代替
>
> border-radius: 10px 20px;将盒子的左上和右下的角用10px半径的圆的弧线替代，将盒子的右上和左下的角用20px半径的弧线代替
>
> border-radius:10px 15px 20px 25px;从左到右分别为左上、右上、右下、左下
>
> 以上是复合写法
>
> 也可以使用 border-top-left-radius、border-top-right-radius、border-bottom-right-radius，和 border-bottom-left-radius来分别赋值
>
> [border-radius - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-radius)

# 盒子阴影box-shadow:

> box-shadow: offset-x offset-y blur-radius spread-radius color inset
>
> offset-x: 阴影偏移量， 阴影相对于原图在x方向上的移动距离，可以为负值
>
> offset-y: 阴影偏移量，阴影相对于原图在y方向上的移动距离，可以为负值
>
> blur-radius: 模糊距离，值越大，阴影越模糊/虚，值越小，阴影越清晰/实
>
> spread-radius: 阴影尺寸，值越大，阴影面积越大，值越小，阴影面积越小
>
> color:阴影颜色，黑色阴影通常用rgba( 0, 0, 0, .3 )
>
> inset:内阴影，默认是外阴影，但不能写outset，写了outset就没有影子，只能不写(外阴影)和写inset(内阴影)
>
> ==至少要给出两个length值，至多给出四个length值，==
>
> ==当给出两个、三个或四个 <length>值时。
> 如果只给出两个值, 那么这两个值将会被当作 <offset-x><offset-y> 来解释。
> 如果给出了第三个值, 那么第三个值将会被当作<blur-radius>解释。
> 如果给出了第四个值, 那么第四个值将会被当作<spread-radius>来解释。
> 可选，inset关键字。
> 可选，<color>值。属性值无顺序，但多个length值作为一个整体内部有顺序要求，且length值不能和可选属性交叉使用，否则box-shadow不起作用==
>
> 盒子阴影不会撑开盒子模型，与盒子模型的面积无关
>
> [box-shadow - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-shadow)

# 文字阴影text-shadow:

> text-shadow: offset-x offset-y blur-radius spread-radius
>
> 大致使用规则同上
>
> [text-shadow - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-shadow)

# 浮动float: (脱标)

> 设置了float特性的盒子会漂浮起来，和其他标准流的盒子不在一个图层上了(会在图层1，其他的在图层2，图层1会压住图层二)
>
> 如果多个盒子都设置了浮动，则它们会按照属性值一行内显示并且顶端对齐排列
>
> 浮动的元素是紧贴在一起的，不会有缝隙，如果父级宽度容不下这些盒子，则会另起一行对齐
>
> ==不论行内元素还是行内块元素在添加浮动后都具有行内块元素的类似特性(不需要再转换了)==
>
> 例如:行内元素有了浮动就可以直接给定高度和宽度了
>
> ​         块级元素有了浮动后宽度的大小根据内容的多少来自动调整
>
> ​         块级元素可以在同一行多个放置了
>
> ​         ......
>
> 一般的，浮动元素经常搭配标准流的父盒子一起使用，在父盒子内加入浮动元素，可以通过更改父盒子的宽高来约束浮动元素的浮动边界，从而达到更好的布局效果(例如左右两边有留白的布局模式)
>
> ==浮动元素不会出现外边距合并-嵌套元素塌陷问题(因为脱标)==
>
> ==浮动元素不会压住文字，而绝对定位和相对定位会压住文字，那是因为浮动设计的初衷就是用来制作文字环绕图片的环绕效果的==

# 清除浮动:

> 清除浮动的必要性: 在很多时候，父盒子并不能直接给定高度(因为不能确定里面的内容，诸如pdd百亿补贴商品页面、文本等等的所需要的面积)，所以不能对父盒子的高度赋值，但父盒子高度不赋值，且内部都是浮动元素会导致父盒子实际的高度为0，这样在其下方的盒子会自动抢占它的位置，导致出现不符合期待的布局情况，这就是为什么要进行清除浮动
>
> 
>
> clear:both;在需要清除浮动的父级盒子的里面的最后面加上一个空白==块级==标签，并给它加上clear属性，赋值为both(总共可以选left、right和both三种值，分别代表清除左边/右边/两边的浮动，一般选用both)，缺点是结构性差，在每个需要清楚浮动的父盒子里面都要添加一次
>
> 
>
> overflow:hidden;在需要清除浮动的父级盒子里面加入这个属性，即可消除浮动，缺点是无法显示溢出的部分
>
> 
>
> after伪元素法:(最常用法，代表网站网易、百度、淘宝等)
>
> ```
> .clearfix:after {
> content: "";
> display: block;
> height: 0;
> clear: both;
> visibility: hidden;	
> }
> .clearfix {   /*  IE6、7专有  */
> *zoom: 1;
> }
> ```
>
> 在CSS中加入上述代码，并在需要清除浮动的父元素的class中加入.clearfix即可(clearfix可以为任意名称，用clearfix只是约定俗成的写法)，此方法是clear: both;的升级版，缺点是要照顾低版本的浏览器
>
> 
>
> 双伪元素法:(常用法，代表网站小米、腾讯)
>
> ```
> .clearfix:before,.clearfix:after {
> content: "";
> display: table;	
> }
> .clearfix:after {
> clear: both;
> }
> .clearfix {   /*  IE6、7专有  */
> *zoom: 1;
> }
> ```
>
> 在CSS中加入上述代码，并在需要清除浮动的父元素的class中加入.clearfix即可(clearfix可以为任意名称，用clearfix只是约定俗成的写法)，此方法是after伪类法的升级版，优点是代码更加简洁，缺点是要照顾低版本的浏览器

# 浮动和标准流的一些注意点:

> 第一步确定版心
>
> > 确定版心.w可为后面的布局提供便利，注意.w版心不得定义高度，如定义高度则失去复用性，如需定义高度应另取类名进行高度定义
> >
> > 举例
> >
> > ```
> > .w {
> > width: npx;
> > margin: auto;
> > }
> > ```
> >
> > 此处应注意如若一些盒子需要定义margin-top/margin-bottom则需要在该盒子其他类名重新定义margin: n auto;因为CSS的层叠性
>
> 导航栏实际开发中不会直接用a链接而是用li包含a链接
>
> > li+a语义更清楚，一看就是有条理的列表型内容
> >
> > 如果直接用a，搜索引擎容易辨别为有堆砌关键词的嫌疑(故意堆砌关键字有被搜索引擎降权的风险)，从而影响网站排名
>
> ![image-20220301132935398](images/image-20220301132935398.png)

# 定位(position/z-index):

> [position - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position)
>
> 定位可以让盒子在某个盒子内部自由自在地移动位置或者固定在屏幕中间的某个位置，==并且可以压住其他盒子==
>
> 定位=定位模式+边偏移
>
> ==绝对定位和固定定位的盒子不会有外边距合并-嵌套元素塌陷的问题==
>
> ==绝对定位和固定定位的盒子具有行内块元素的性质:==
>
> ==行内元素可以设置宽高==
>
> ==块级元素不给宽高默认大小是内容的大小==
>
> ==但需要注意的是绝对定位和相对定位元素不会默认一行多个排列而是默认后者覆盖前者(在有z-index的情况下则按照z-index的值来进行覆盖)==
>
> ==浮动元素不会压住文字，而绝对定位和相对定位会压住文字，那是因为浮动设计的初衷就是用来制作文字环绕图片的环绕效果的==
>
> 定位模式
>
> > position: static relative absolute fixed(静态定位 相对定位 绝对定位 固定定位)
>
> 边偏移
>
> > top: npx
> >
> > bottom: npx
> >
> > left: npx
> >
> > right: npx
> >
> > 定义元素相对于父元素的上/下/左/右边线的偏移距离npx
> >
> > 如果一个盒子既有left又有right则默认执行left，同理top和bottom默认执行top
>
> position: static静态定位(==position的默认值==)
>
> > 静态定位按照标准流摆放位置，它没有边偏移
> >
> > 静态定位在布局时很少用到
>
> position: relative相对定位
>
> > 相对定位是相对于该元素本来的位置(参照点是原来位置)，且该元素的本来位置依然被它所占据，也就是说即便它通过相对定位离开了它原本的位置，但在它下面的盒子仍然不能自动抢占它的位置(下面的盒子依然以标准流对待它，该元素==不脱标==，继续保留原本位置)
> >
> > 有边位移，不写默认左上
>
> position: absolute绝对定位
>
> > 绝对定位是相对于它的最近一级有定位(相对、绝对、固定定位)的祖先元素来说的，如果==没有祖先元素==或者==祖先元素没有定位==，则以浏览器为准定位
> >
> > 绝对定位不再占有原来的位置(==脱标==)
> >
> > 有边位移，不写默认左上
> >
> > 
> >
> > 绝对定位盒子居中
> >
> > > ![image-20220302183740385](images/image-20220302183740385.png)
> > >
> > > ==实际上脱标元素都不可以通过margin: 0 auto;来实现水平居中==
> > >
> > > 上述为绝对定位的width: 200px的盒子实现水平居中的方法
> > >
> > > 其它width值的盒子更改margin-left的值为width的一半的负数即可
> > >
> > > 绝对定位的盒子实现垂直居中的思想和上述方法一致
> > >
> > > ==子绝父相后，子向左向上移动本身宽度和高度的一半（也可以用 transform:translate(-50%,-50%)）==
>
> position: fixed固定定位
>
> > 固定定位是相对于视口的定位，永远处于视口的某一固定位置且压住其它盒子，不论鼠标如何滚动它的位置都不发生改变(类似网页的流氓广告)
> >
> > 固定定位不再占有原来的位置(==脱标==)
> >
> > 有边位移，不写默认左上
> >
> > ![image-20220302181106793](images/image-20220302181106793.png)
>
> position: sticky粘性定位
>
> > 粘性定位可以被认为是相对定位和固定定位的混合。元素在跨越特定阈值前为相对定位，之后为固定定位。
> >
> > #one { position: sticky; top: 10px; }
> >
> > 在 viewport 视口滚动到元素 top 距离小于 10px 之前，元素为相对定位。之后，元素将固定在与顶部距离 10px 的位置，直到 viewport 视口回滚到阈值以下。
> >
> > 须指定 [`top`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/top), [`right`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/right), [`bottom`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/bottom) 或 [`left`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/left) 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。
> >
> > ==不脱标==
>
> 子绝父相:
>
> > ![image-20220302165201733](images/image-20220302165201733.png)
>
> 定位叠放次序:
>
> > ![image-20220302182810917](images/image-20220302182810917.png)
> >
> > 只有有定位的盒子才有这个属性，标准流没有

# 网页布局总结:

> ![image-20220302220958464](images/image-20220302220958464.png)

# 元素的显示与隐藏(display/visibility/overflow):

> 类似网站的广告，点击×关闭，但重新加载页面又会出现
>
> display:
>
> > display: none; 隐藏对象，==隐藏后不再占有原来的位置==
> >
> > display: block; 除了转换成块级元素的作用外，同时还有==显示元素==的意思
> >
> > 可以搭配js做很多网页特效
>
> visibility:
>
> > visibility: visible; 元素可见
> >
> > visibility: hidden; 元素隐藏，==隐藏后继续占有原来的位置==
> >
> > 隐藏后要原来位置就用visibility: hidden;不要原来位置就用display: none;(display用处更多)
>
> overflow:
>
> > overflow: hidden;不显示超出对象范围的内容，超出的部分隐藏掉
> >
> > overflow: visible;不剪切也不添加滚动条，直接在盒子外面显示多出的部分内容
> >
> > overflow: scroll;不管内容大小是否超出范围，都添加滚动条
> >
> > overflow: auto;超出显示滚动条，不超出则不显示
> >
> > 水平/竖直滚动条范围距离和盒子宽/高一样长
> >
> > 一般情况下我们都不想让溢出的部分显示出来，因为这样会影响布局
> >
> > 但是如果是有定位的盒子，请谨慎使用overflow: hidden;因为它会隐藏多余的部分(给特效的使用带来了局限性)

# 精灵图:

> 为了减少服务器被请求图片的次数，减轻它的压力，使用精灵图技术(即将多张小图放在一个大图里面，这张大图就叫精灵图)
>
> ![image-20220303125342337](images/image-20220303125342337.png)
>
> 在盒子里面用background引入精灵图并通过确定xy值和盒子宽高来提取精灵图中所需要的那部分小图
>
> 需注意的是这里的background-position和position的xy取值方式不同(position的取值方式见上)，此处xy大多数时为负值
>
> ![img](images/WYKRE9V%7BES8$C9TTQAX9EQC.png)
>
> 上加下减    左加右减
> xy为0时是默认左边的情况 xy赋负值后是右边的情况 ==盒子的左上角点代表盒子的坐标 通过盒子的宽高和坐标来确定所截取的图==

# 字体图标:

> ![image-20220303170115374](images/image-20220303170115374.png)
>
> ![image-20220303172025516](images/image-20220303172025516.png)
>
> ![image-20220303173011069](images/image-20220303173011069.png)
>
> ![image-20220303174143541](images/image-20220303174143541.png)
>
> ![image-20220303174607022](images/image-20220303174607022.png)
>
> ![image-20220303174805984](images/image-20220303174805984.png)
>
> ![image-20220303182231787](images/image-20220303182231787.png)
>
> ![image-20220303182108405](images/image-20220303182108405.png)
>
> 字体图标本质上是字体属性，首先需要给盒子添加font-family: 'icomoon';属性，再将font文件夹中的demo网页打开，再选取自己想要的字体图标的方框复制过来即可实现该字体图标效果，可以通过改变font-size/font-color等属性值来改变样式
>
> 
>
> 字体图标的追加:
>
> 点击网站的IcoMoonApp->Import Icons然后选择selection.json再选中想新添加的图标，重新下载压缩包，并替换原来的文件即可

# css三角形:

> 将盒子的宽高都设为0，并给盒子的四个边框都加上不同的颜色
>
> ```
>     width: 0;
>     height: 0;
>     border: 20px solid;
>     border-top-color: pink;
>     border-right-color: red;
>     border-bottom-color: blue;
>     border-left-color: green;
> ```
>
> 即可得到如下
>
> ![image-20220306131945831](images/image-20220306131945831.png)
>
> 因此如若想得到以其中一个边为底的三角形，只需要将其他三个边框的颜色设置为transparent即可

# 鼠标样式cursor:

> cursor: default;默认 左上白色箭头
>
> cursor: pointer;小手
>
> cursor: mover;移动
>
> cursor: text;文本(文本的默认鼠标样式)
>
> cursor: not-allowed;禁止
>
> 更多样式见下
>
> [cursor - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/cursor)

# 取消表单轮廓-防止拖拽文本域

> outline: none;/outline: 0;可以取消表单的外轮廓(input textarea select)
>
> resize: none;取消文本域的右下角拖拽按钮，可以固定textarea的大小
>
> (textarea和/textarea尽量写在同一行，在不同一行的话文本框内会预先输入几个可以删除的空格)
>
> (空格的数量和textarea与/textarea之间的空格数量相同)

# vertical-align:

> vertical-align是属于img的属性
>
> 相对于父元素
>
> vertical-align:baseline;默认，元素放在父元素的基准线上
>
> vertical-align:middle;元素放置在父元素的中线上
>
> vertical-align:text-top;元素与父元素的字体顶端对齐
>
> vertical-align:text-bottom;元素与父元素的字体底端对齐
>
> 相对于整行
>
> vertical-align:top;元素的顶部与整行的顶部对齐
>
> vertical-align:bottom;元素的底部与整行的底部对齐
>
> (没有弄清楚，关于父元素与整行的区别存疑)
>
> [vertical-align - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)

# 图片底侧空白缝隙解决方案:

> 图片底侧出现空白缝隙的原因: 图片与文字一行时默认为vertical-align: baseline;所以为了与文字都在基线上，图片下方会留出空白缝隙来为文字中的英文如pjyq等留出下尾位置
>
> 解决方案一(推荐):
>
> > 将图片的vertical-align赋值为非baseline即可，只要不是基线对齐则图片不会为英文字母留出下尾位置，则不会出现图片底侧缝隙
>
> 解决方案二:
>
> > 将图片转换为块级元素display: block;块级元素独占一行，不与文字一行则图片不会为英文字母留出下尾位置，则不会出现底侧缝隙

# 溢出文字省略号显示

> 单行文字溢出省略号显示:
>
> > 先white-space: nowrap;(默认为normal，自动换行)此属性值意味着强制文字一行内显示不换行
> >
> > 再overflow: hidden;将溢出的部分隐藏起来
> >
> > 最后text-overflow: ellipsis;将溢出部分用省略号来显示(ellipsis是省略号的意思)
>
> 多行文本显示省略号:(有较大兼容性问题，但用也无妨，最好让后台人员来实现)
>
> > overflow: hidden;
> >
> > text-overflow: ellipsis;
> >
> > /* 弹性伸缩盒子模型显示 */
> >
> > display: -webkit-box;
> >
> > /* 限制在一个块元素内显示的文本行数 */
> >
> > -webkit-line-clamp: 2;
> >
> > /* 设置或者检索伸缩盒子对象的子元素的排列方式 */
> >
> > -webkit-box-orient: vertical;
> >
> > (一般选择2 两行，若想在第n行显示省略号，只需要将-webkit-line-clamp: 2;中的2改为相应的n即可)
> >
> > (注意若限制的文本行数为n但实际文本超出了n行则还是会显示n行后的的文字，所以应提前设计好文本框的高，譬如限制的文本行数为n，则应该将文本框盒子的高调到刚好只够n行文字显示的宽高，==或者是用行内元素让文字把高撑开，又因为上面的设置只有两行，那么就完美的变成两行了==)

# 布局技巧之margin负值巧妙运用:

> 实现商品页每个商品都自带细线框，例如下图淘宝页面
>
> ![image-20220307155204195](images/image-20220307155204195.png)
>
> 为每个盒子添加npx的边框border和浮动float，但第n个盒子和n+1个盒子中间会出现两个边框叠加成2npx厚度的情况,这样只需要将每个盒子加上一个margin-left: -npx;即可，每个盒子向左移动npx则可抵消掉叠加的边框
>
> (此处有疑问，第一个盒子也加上了margin-left: -npx;应该是一整行都向左移动了npx)
>
> (解答: 在每个盒子的属性中都有float: left;所以每个盒子都先自动向左靠紧，然后再执行margin-left: -npx;所以相当于一整行都向左移动了npx，然后除了第一个盒子以外所有的盒子都向左再移动了npx)
>
> (上下相邻的盒子消除叠加的边框原理和上述相似)
>
> 
>
> 实现当鼠标移动到商品框上边框变色的样式
>
> ![image-20220307162637114](images/image-20220307162637114.png)
>
> 为每个盒子添加:hover {border-color: orange;}，但需要注意的是由于使用了上面的方法来消除叠加边框，所以中间盒子的右边框实际上是被它右边盒子的左边框压住的，所以每个中间盒子的有边框都无法显色
>
> 解决方法: 在hover伪类中加入position: relative; 因为相对定位不脱标，还占有原来位置，所以鼠标移上去的时候它变成了相对定位不脱标占有原来位置，右边框也自然占有原来位置显现出来并且变色。但如果该盒子本身是一个子盒子，已经有了绝对定位，那么不能再给它相对定位，此时可以选择给它定位叠放次序z-index，默认为z-index: auto;所以只需在hover伪类中添加z-index: 1;即可

# 布局技巧之文字围绕浮动元素

> 在父盒子中有文字和一个子盒子，给子盒子添加图片并且给图片添加浮动，这样文字就会围绕子盒子/图片，不会被压住

# 布局技巧之行内块元素的巧妙运用

> 利用行内块元素实现底部翻页区域
>
> [黑马程序员pink老师前端入门教程，零基础必看的h5(html5)+css3+移动端前端视频教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14J4114768?p=270)

# 布局技巧之css三角巧妙运用

> ![image-20220308130801544](images/image-20220308130801544.png)
>
> 利用三角实现上图中斜杠
>
> ```
>   div {
>          width: 0;
>          height: 0;
>          border-color: transparent red transparent transparent;
>          border-width: 24px 8px 0 0;
>          border-style: solid;
>      }
> ```
>
> ![image-20220308132117358](images/image-20220308132117358.png)
>
> 将颜色改为白色，用定位调位置，放置在盒子中即可
>
> 原理:
>
> > ```
> >  div {
> >          width: 0;
> >          height: 0;
> >          border-color: pink red blue green;
> >          border-width: 24px;
> >          border-style: solid;
> >      }
> > ```
> >
> > ![image-20220308132536337](images/image-20220308132536337.png)
> >
> > 将底部边框宽度改为0则变成
> >
> > ```
> >   div {
> >          width: 0;
> >          height: 0;
> >          border-color: pink red blue green;
> >          border-width: 24px 24px 0;
> >          border-style: solid;
> >      }
> > ```
> >
> > ![image-20220308132649738](images/image-20220308132649738.png)
> >
> > 再将左侧边框宽度也改为0
> >
> > ```
> >  div {
> >          width: 0;
> >          height: 0;
> >          border-color: pink red blue green;
> >          border-width: 24px 24px 0 0;
> >          border-style: solid;
> >      }
> > ```
> >
> > ![image-20220308132743522](images/image-20220308132743522.png)
> >
> > 再将上边框的颜色改为透明，并且调节上边框的厚度和右边框的厚度即可得到不等边三角形
> >
> > ```
> > div {
> >          width: 0;
> >          height: 0;
> >          border-color: transparent red transparent transparent;
> >          border-width: 24px 8px 0 0;
> >          border-style: solid;
> >      }
> > ```
> >
> > ![image-20220308132940663](images/image-20220308132940663.png)
> >
> > 最后将右边框改成白色即可放置在右边盒子的左边实现斜杠效果
> >
> > (实现其他不同方向不同颜色不同角度不同边长的三角形自行修改相应参数即可)
>
> [黑马程序员pink老师前端入门教程，零基础必看的h5(html5)+css3+移动端前端视频教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14J4114768?p=271)
>
> [黑马程序员pink老师前端入门教程，零基础必看的h5(html5)+css3+移动端前端视频教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14J4114768?p=272)

# CSS初始化:

> CSS初始化是因为不同浏览器对元素的初始值不尽相同，需要初始化来统一初始标准
>
> 每个网页都需要先进行初始化来保证网页的兼容性
>
> 京东初始化代码如下:
>
> > ```
> >         /* 所有标签内外边距清零 */
> >  * {
> >             margin: 0;
> >             padding: 0
> >         }
> >         /* em和i斜体变正 */
> >         em,
> >         i {
> >             font-style: normal
> >         }
> >         /* li去掉前面的小圆点和123 */
> >         li {
> >             list-style: none
> >         }
> >         /* border: 0;是为了照顾低版本浏览器，如果图片外面包含了链接会有边框的问题 */
> >         /* 解决图片底部空白缝隙 */
> >         img {
> >             border: 0;
> >             vertical-align: middle
> >         }
> >         /* 实现按钮鼠标变成小手 */
> >         button {
> >             cursor: pointer
> >         }
> > 
> >         a {
> >             color: #666;
> >             text-decoration: none
> >         }
> > 
> >         a:hover {
> >             color: #c81623
> >         }
> > 
> >         button,
> >         input {
> >             font-family: Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif
> >         }
> > 
> >         body {
> >             /* css3 抗锯齿形 让文字显示的更加清晰 */
> >             -webkit-font-smoothing: antialiased;
> >             background-color: #fff;
> >             font: 12px/1.5 Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif;
> >             color: #666
> >         }
> > 
> >         .hide,
> >         .none {
> >             display: none
> >         }
> >         /* 为元素法清除浮动 */  
> >         .clearfix:after {
> >             visibility: hidden;
> >             clear: both;
> >             display: block;
> >             content: ".";
> >             height: 0
> >         }
> > 
> >         .clearfix {
> >             *zoom: 1
> >         }
> > ```
> >
> > 其中"\5B8B\4F53"代表宋体，但计算机无法识别中文，所以使用unicode编码进行编写
> >
> > ![image-20220308134530143](images/image-20220308134530143.png)
>
> [黑马程序员pink老师前端入门教程，零基础必看的h5(html5)+css3+移动端前端视频教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14J4114768?p=273)

# css3(新增属性选择器/新增结构伪类选择器/伪元素):

> 新增属性选择器:
>
> > ![image-20220309150539353](images/image-20220309150539353.png)
> >
> > [属性选择器 - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Attribute_selectors)
>
> 新增结构伪类选择器:
>
> > ![image-20220309152749933](images/image-20220309152749933.png)
> >
> > 实际应用中应与元素选择器/组合选择器: 后代选择器/组合选择器: 子选择器/并集选择器配合使用
> >
> > 且和它们的效果完美搭配
> >
> > 其中nth-child(n)最重要，n可以是数字、关键字(even偶数 odd奇数)、公式(例如字母n也是一个公式，意思从0开始每次加1，忽略0，效果是选择所有child)(所以公式2n就是even 公式2n+1就是odd -n+5就是前五个)
> >
> > 
> >
> > ==(E: first-child并不是从E元素所在范围内找第一个E元素，而是从E元素所在的范围内找第一个元素(无论它是E元素还是其他元素)，当找到的第一个元素是E元素是则样式声明有效，当不是E元素时样式声明无效，last-child和nth-child(n)同理)==
> >
> > 
> >
> > ==(E: first-of-type则是指从E元素所在范围内找到第一个E元素并对其进行样式声明，last-of-type和nth-of-type(n)同理)==
> >
> > ```
> > <body>
> >    <div>
> >        <li>1</li>
> >        <ul>
> >            <span>1</span>
> >            <li>1</li>
> >            <li>2</li>
> >            <li>3</li>
> >            <li>4</li>
> >            <li>5</li>
> >        </ul>
> >        <ul>
> >            <li>1</li>
> >            <li>2</li>
> >            <li>3</li>
> >            <li>4</li>
> >            <li>
> >                <ol>
> >                    <li>1</li>
> >                    <li>2</li>
> >                    <li>3</li>
> >                </ol>
> >            </li>
> >        </ul>
> >    </div>
> > </body>
> > ```
> >
> > 
> >
> > ```
> >  li:first-child {
> >             background: red;
> >         }
> > ```
> >
> > ![image-20220309163406433](images/image-20220309163406433.png)
> >
> > ```
> >  li:first-of-type {
> >             background: red;
> >         }
> > ```
> >
> > ![image-20220309165305575](images/image-20220309165305575.png)
> >
> > 这里需要注意的是，当E与:first-child之间没有空格时，一切按照上述所说进行渲染，当E与:first-child之间有空格时会出现混乱渲染，逻辑未知，last-child同理，但nth-child(n)似乎不受影响(建议都不留空格)
> >
> > ==当E与:first-of-type之间没有空格时，一切按照上述所说进行渲染，当E与:first-of-type之间有空格时，渲染逻辑变成寻找到自身是父元素的E元素(即自己有子元素的E元素，没有子元素的E元素不算)，然后分别对E的每种子元素的第一个进行样式声明，last-of-type和nth-of-type(n)同理==
> >
> > [伪类 - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes)
>
> 伪元素:
>
> > ![image-20220309173626102](images/image-20220309173626102.png)
> >
> > 伪元素使用双冒号，单冒号的情况是为了照顾低版本浏览器
> >
> > content一般属性值为字体图标，此处字体图标一般不用方框而是类似\E91E这样的转义符号加值，值在demo网页中你所要使用的符号下面可以找到
> >
> > content值可为空
> >
> > 伪元素和伪类选择器搭配使用时，伪元素应该在伪类选择器的后面例如div:hover::before

# css3盒子模型:

> ![image-20220309200310471](images/image-20220309200310471.png)
>
> box-sizing是属性，content-box是默认属性值
>
> 可以在css初始化中加入box-sizing: border-box;

# CSS3滤镜filter:

> ![image-20220309202010084](images/image-20220309202010084.png)
>
> blur()模糊 参数为长度 默认为0 不接受百分比
>
> brightness()亮度  0%全黑 默认100%不变 大于100%提供更明亮的结果
>
> contrast()对比度 0%全黑 默认100%不变 大于100%意味着运用更低的对比(尚未理解对比度)(更高的曝光度？)
>
> drop-shadow()阴影 除了没有inset和spread-radius关键字，其它与box-shadow属性相似
>
> grayscale()灰度 默认0%不变 100%完全转为灰度图像 值范围为0%-100%
>
> hue-rotate()色相旋转 单位为角度angle 默认0deg色相不变 180deg最阴间 360deg相当于转一圈和0deg一样 无最大值
>
> invert()函数反转输入图像？ 默认值0% 最大值100%变阴间(和hue-rotate是不同样式的阴间) 50%图变黑
>
> opacity()不透明度 默认值1图像无变化 最小值0%图像完全透明(消失)
>
> saturate()饱和度 默认值1图像无变化 最小值0%完全不饱和 大于100%则有更高饱和度(颜色更亮？)
>
> sepia()转为深褐色 默认值0%无变化 最大值100%全是深褐色
>
> 可以用复合写法，例如filter: blur(3px) brightness(120%)...
>
> [filter - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter)

# CSS3 calc函数:

> ![image-20220310102016630](images/image-20220310102016630.png)

# CSS3过渡transition

> transition是transition-property、transition-duration、transition-function和transition-delay的简写
>
> transition-property: 所有物、性质，此处指要进行过渡的属性，不可省略
>
> transition-duration: 完成过渡的时间，单位s，不可省略
>
> transition-function: 过渡的方式，默认为ease(逐渐变慢)，可选ease-in(逐渐变快)、ease-out(逐渐变慢)(比ease更慢)、ease-in-out(先快后慢)、linear(匀速)、step-start、step-end和step(n,end)，可省略
>
> transition-delay: 在过渡开始前需要等待的时间，单位s，可省略
>
> transition一般和:hover搭配使用，例如
>
> ```
>         div {
>             width: 200px;
>             height: 200px;
>             background: red;
>             transition: background .5s;
>         }
> 
>         div:hover {
>             background: green;
>         }
> ```
>
> 如若对多个属性进行过渡则将transition并写即可
>
> ```
>   div {
>             width: 200px;
>             height: 200px;
>             background: red;
>             transition: background .5s , width .5s , height .5s;
>         }
> 
>         div:hover {
>             width: 300px;
>             height: 300px;
>             background: green;
>         }
> ```
>
> 如若对所有属性进行过渡
>
> ```
>   div {
>             width: 200px;
>             height: 200px;
>             background: red;
>             transition: all .5s;
>         }
> 
>         div:hover {
>             width: 300px;
>             height: 300px;
>             background: green;
>         }
> ```
>
> [transition - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition)

# transform

> transform从左到右执行，需要注意的是rotate会改变图片的xy坐标轴，所以rotate后translateX不再是水平移动而是旋转后的图片的x轴移动，所以transform: rotate(45deg) translateX(150px);和transform: translateX(150px) rotate(45deg) 两者的最终效果不同
>
> > ```css
> > transform: translateX(150px) rotate(45deg) 
> > ```
> >
> > ![image-20220902162532451](images/image-20220902162532451.png)
> >
> > ```css
> > transform: rotate(45deg) translateX(150px);
> > ```
> >
> > ![image-20220902162553213](images/image-20220902162553213.png)

# CSS3 2D转换之transform: translate

> transform: translate(x,y)
>
> transform: translate(x) = transform: translate(x,0) = transform: translateX(x) 
>
> transform: translate(0,y) = transform: translateY(y)
>
> 移动到距左边x距离，距上边y距离的位置，只对块级元素和行内块元素有效
>
> translate的盒子继续占有原来的位置(==不脱标==)
>
> xy可以是长度和百分比，x(y)为百分比时值为带有该属性的盒子的相应的宽(高)乘x(y)
>
> [translate() - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/translate())
>
> [translateX() - CSS: Cascading Style Sheets | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/translateX())
>
> [translateY() - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/translateY())
>
> `translateZ(tz)` is equivalent to `translate3d(0, 0, tz)`.
>
> [translateZ() - CSS: Cascading Style Sheets | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/translateZ())
>
> [translate3d() - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/translate3d())

# CSS3 2D转换之translate实现盒子水平垂直居中:

> ![image-20220311173134285](images/image-20220311173134285.png)

# CSS3 2D/3D旋转之rotate

> (用rotate也可以实现三角[黑马程序员pink老师前端入门教程，零基础必看的h5(html5)+css3+移动端前端视频教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14J4114768?p=359&spm_id_from=pageDriver))
>
> transform: rotate(n) 图片顺时针旋转n度，n可为负值，为负值时则逆时针旋转n的绝对值度，单位为deg
>
> transform: rotateX(n) 图片绕x轴旋转n度(方向为从左往右看的顺时针方向)，n可为负值，为负值时方向为从左往右看的逆时针方向
>
> transform: rotateY(n) 图片绕y轴旋转n度(方向为从上往下看的顺时针方向)，n可为负值，为负值时方向为从上往下看的逆时针方向
>
> [rotate() - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotate())
>
> [rotateX() - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotateX())
>
> [rotateY() - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotateY())
>
> rotate3d()[rotate3d() - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotate3d())
>
> rotateZ(a)相当于rotate(a) or rotate3d(0, 0, 1, a)
>
> [rotateZ() - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/rotateZ())

# CSS3 transform-origin:

> 确定transform的中心点
>
> 一个值：
> 必须是<length>，<percentage>，或 left, center, right, top, bottom关键字中的一个。
> 两个值：
> 其中一个必须是<length>，<percentage>，或left, center, right关键字中的一个。
> 另一个必须是<length>，<percentage>，或top, center, bottom关键字中的一个。
> 三个值：
> 前两个值和只有两个值时的用法相同。
> 第三个值必须是<length>。它始终代表Z轴偏移量。
>
> [transform-origin - CSS（层叠样式表） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-origin)

# CSS3 2D转换之scale:

> 实现盒子的缩放功能
>
> transform: scale(x,y)
>
> ```
> p {
>   width: 50px;
>   height: 50px;
>   background-color: teal;
> }
> 
> .transformed {
>   /* 等同于变换: scaleX(2) scaleY(2);*/
>   transform: scale(2);
>   background-color: blue;
> }
> 
> ```
>
> transform: scale实现盒子嗯低缩放功能是根据transform-origin的坐标进行比例缩放
>
> transform: scale(x,y)在实现盒子的缩放与单纯的改变盒子的width和height的方法相比更具优势:
>
> > transform: scale(x,y) x代表宽度被缩放的倍数，y代表高度被缩放的倍数，xy都没有单位，当只有一个值时，则对宽高都生效(x > 1就是放大 x < 1就是缩小 y同理)
> >
> > transform: scale是按照transform-origin的坐标来进行缩放的，而改变width和height的方法则是原盒子的上边框的位置不变，改变其他的三个边框，以此来进行缩放
> >
> > transform: scale缩放并不会影响其他元素布局，而改变width和height则会挤压其他元素的空间
> >
> > 

# CSS3 transform的复合写法

> 其格式为 transform: translate() rotate() scale()......
>
> 其顺序会影响最终结果，是因为先旋转rotate会改变坐标轴的方向
>
> 当我们有位移和其他的属性值时，要将位移放在最前面

# CSS3 动画序列@keyframes

> ```
> @keyframes name {
>   from {
>     transform: translateX(0%); 
>   }
> 
>   to {
>     transform: translateX(100%);
>   }
> }
> ```
>
> from to相当于0% 100%，若想设置更多的节点，可以在里面添加其他的percentage，此处的percentage相当于整个animation-duration的部分时间，例如
>
> ```
> @keyframes name {
>   0% { top: 0; left: 0; }
>   30% { top: 50px; }
>   68%, 72% { left: 50px; }
>   100% { top: 100px; left: 100%; }
> }
> ```
>
> ==注意还要在添加动画效果的盒子中加入animation-name: name;和animation-duration: ns;==
>
> ![image-20220317110444650](images/image-20220317110444650.png)
>
> 其中animation-play-state是需要将鼠标放在动画盒子上才能起作用的
>
> animation复合写法，(animation-play-state不能写在复合写法里面，animation-name和animation-duration使必须的)
>
> 复合写法的顺序没有要求，但是相同单位的两个属性之间的顺序有要求，例如animation-duration和animation-delay的单位都是秒s
>
> 各个属性见mdn文档 css参考

# CSS3 3D转换translate和perspective

> `translateZ(z)` is equivalent to `translate3d(0, 0, z)`.
>
> [translateZ() - CSS: Cascading Style Sheets | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function/translateZ())
>
> perspective: length;<length> 指定观察者距离 z=0 平面的距离，为元素及其内容应用透视变换。当值为0或负值时，无透视变换。(在父元素中加入perspective，子元素的变换过程会更有立体感)
>
> perspective一般用于父盒子，translateZ一般用于父盒子中具体的子盒子

# CSS3 transform-style:

> transform-style: flat;默认值 设置元素的子元素位于该元素的平面中
>
> transform-style: preserve-3d;设置元素的子元素位于3D空间中(给父盒子添加)
>
> 平面:
>
> > ![image-20220317165308919](images/image-20220317165308919.png)
>
> 3D:
>
> > ![image-20220317165552031](images/image-20220317165552031.png)

