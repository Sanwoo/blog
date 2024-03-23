---
title: html
---
# html参考文档: 
[HTML 属性参考 - HTML（超文本标记语言） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Attributes)

# html元素参考文档: 
[HTML 元素参考 - HTML（超文本标记语言） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)

# html属性参考文档: 
[HTML 属性参考 - HTML（超文本标记语言） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Attributes)

# h1 h2 h3 h4 h5 h6 p br
>重要性 字体加粗效果递减
p 段落
br强制换行（单标签）

# strong vs b :加粗

> strong 是一个逻辑状态，而 bold 是一个物理状态。逻辑状态分离内容和表现形式，使用逻辑状态允许你用各种不同的方式来表达。比如你想把文字渲染成红色，使用其它大小的字体、带有下划线或其他样式，而不是加粗的样式。必须要理解使用 strong 呈现出的表现形式不同于单纯的加粗。 因为 bold 是一个物理状态，他没有区分表现形式和内容。如果让 bold 做了加粗文本外的其它任何事情，都将会令人困惑而且也不符合逻辑。
>
> 同样应该注意<b></b> 还有其他用途，比如想单纯地吸引注意而不增加其重要性。

# em vs i :倾斜

> 在默认情况下，它们的视觉效果是一样的。但语义是不同的。 `<em>` 标签表示其内容的着重强调，而 `<i>` 标签表示从正常散文中区分出的文本，例如外来词，虚构人物的思想，或者当文本指的是一个词语的定义，而不是其语义含义。（作品的标题，例如书籍或电影的名字，应该使用 `<cite>`。）
>
> 这意味着，正确使用哪一个取决于具体的场景。两者都不是纯粹为了装饰的目的，那是 CSS 样式所做的。
>
> 一个 `<em>` 的例子可能是："Just *do* it already!"，或："We *had* to do something about it"。人或软件在阅读文本时，会对斜体字的发音使用重读强调。
>
> 一个 `<i>` 的例子可能是："The *Queen Mary* sailed last night"。在这里，没有对 "Queen Mary" 这个词添加强调或重要性。它只是表明，谈论的对象不是一个名叫玛丽的女王，而是一艘名字叫玛丽的船。另一个 `<i>` 的例子可能是："The word *the* is an article"。

# del vs s :删除线

> 推荐使用del

# ins vs u :下划线

> **HTML `<u>` 元素（表意不清标注元素）**表示一个需要标注为非文本化（non-textual）的内联文本域。默认情况下渲染为一个实线下划线，可以用 CSS 替换。
>
> 此元素以前在旧版本的 HTML 中称为 “下划线” 元素，但有时仍会以这种方式被滥用。要为文本加下划线，您应该应用包含 CSS [`text-decoration`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-decoration)属性设置为 `underline` 的样式。
>
> 推荐使用ins     

# div vs span :盒子

> div暂且理解为大盒子 一行一个
>
> span暂且理解为小盒子 一行多个

# img :

> 添加图片 src：图片地址/路径  alt：图片显示不出来时的文字描述  title：光标移上去时显现的文字描述 
>
> width和height：图片长宽的数值 只修改其中一个时另一个会根据图像原始比例等比缩放

# 相对路径 :

> 上一级目录 ../   
>
> 同一级目录 ./或者直接写名称
>
> 下一级目录 在同级目录下寻找 / 

# a :

> `<a href=""></a>` 中间可以插入任何网页元素 包括文字 图片 视频 音频 表格都可以添加超链接
>
> href :添加url网址/添加#为空地址/添加文件则点击下载该文件
>
> target :跳转网址的方式 默认为 _self 原窗口跳转 可改为 _blank 新窗口打开 
>
> 注意当还没有确定好链接地址时应该在href填入空连接# 否则在用伪类选择器a:link时将无法为其更改颜色

# 锚点链接：

> 点击锚点链接可以快速地跳到当前页面中的某一个位置
>
> 在链接文本的href属性中添加==#名字== 的形式，如`<a href="#personallife">个人生活</a>`
>
> 找到目标位置标签，里面添加一个ID属性=刚才的名字，如 `<h3 id ="personallife">个人生活简历</h3>`  

# 注释字符 :

> `<!--  -->`  快捷键 :ctrl+/ 

# 特殊字符 :

![](/images/$D2VC{0B24XUJ@`Z_DP76VP.png)

# 表格 :

> table>thead tbody>tr(table row)>th(table head) td(table data)  

``` HTML
<table>
     <thead>
         <tr>
             <th>这是表头文本/数据 会加粗居中</th>
         </tr>
     </thead>
     <tbody>
         <tr>
             <td>这是表格中的文字/数据</td>
         </tr>
     </tbody>
 </table>
``` 

# 表格中的单元格合并 :

> rowspan 跨行合并 colspan 跨列合并
>
> 若跨行合并则在要合并的单元格(即td中加入属性 rowspan=x x为具体要合并的单元格的数目)
>
> 并将其他被合并的单元格的td删除 以免单元格溢出
>
> 跨列合并同理

# ul 和 ol和li :

> ul 无序列表 前面有小黑点
>
> ol 有序列表 前面自动有1. 2. 3.
>
> 两者内部只能有li标签 li标签内可以有任何标签
>
> ==li只能存在ol、ul、menu和dir内部==

# dl :

> dl自定义列表  dt(定义项目/名字)  dd(描述每个项目/名字) 只能用dt和dd两种标签 dt和dd内可以有任何标签
>
> ``` HTML
> <dl> 
> 
> <dt>关注我们</dt>
> 
> <dd>新浪微博</dd>
> 
> <dd>官方微信</dd>
> 
> <dd>联系我们</dd>
> 
> </dl>    
> ```

# 表单域 :所有的表单元素都要在表单域内

> `<form action="your url" method="post/get" name="yourname"></form>` 
>
> 其中url是后台处理数据服务器的地址 name则是表单域名称用来和同一页面中的不同表单域区分彼此

# 表单元素input的各种属性 :

> type:text password radio checkbox submit reset button file date datetime-local
>
> name和value是每个表单元素都有属性值，主要给后台人员使用
>
> 要求单选按钮和复选框都要有相同的name值
>
> checked属性主要针对单选按钮和复选框，主要作用是一打开页面就可以默认选中某个表单元素
>
> maxlength是用户可以在表单元素中输入的最大字符数，一般较少使用
>
> label标签的for的值与其他标签的id的值相同时 点击label标签内的文本时 可以直接选中与它id相同的标签内容
>
> ``` HTML
> <form action="url" method="post">
> 
> <!-- label标签的for的值与其他标签的id的值相同时 点击label标签内的文本时 可以直接选中与它id相同的标签内容-->
> 
> <!-- text 文本框 用户可以在里面输入任意文字 value的值可以在文本框中显示出来 maxlength表示最大字符数-->
> 
> <label for="username">用户名:</label><input type="text" name="username" id="username" value="请输入用户名" maxlength="6">
> 
> <br>
> 
> <!-- passsword 密码 用户可以在里面输入密码 密码不可见 -->
> 
> 密码: <input type="password" name="pwd"><br>
> 
> <!-- radio 圆心单选按钮 选择后不可以取消选择 拥有同样name的选项只能选择一个 将所有选项的name都设为相同的值则可以实现多选一的效果-->
> 
> <!-- checked主要让页面默认选中该表单元素 单选按钮只能有一个有checked 复选框则可以随意加或者不加checked -->
> 
> 性别: 男<input type="radio" name="sex" id="" checked="checked"> 女<input type="radio" name="sex" id=""> 未知<input type="radio" name="sex" id="">
> 
> <br>
> 
> <!-- checkbox 对勾复选框 可以实现多选 也可以实现取消选择 无论name为何种状态-->
> 
> 爱好: 吃饭<input type="checkbox" name="hobby" id="" checked="checked"> 睡觉<input type="checkbox" name="hobby" id="" checked="checked"> 打豆豆<input type="checkbox" name="hobby" id="" checked="checked">
> 
> <br>
> 
> <!-- 点击了按钮 可以把表单域form里面的表单元素里面的值提交给后台服务器 更改value可以写上你想按钮上显示的字如“提交” “注册”等-->
> 
> <input type="submit" value="提交">
> 
> <br>
> 
> <!-- 重置按钮可以还原表单元素出事的默认状态 此处value同上-->
> 
> <input type="reset" value="重写">
> 
> <br>
> 
> <!-- 普通按钮button 后期搭配js一起使用 -->
> 
> <input type="button" value="获取短信验证码">
> 
> <br>
> 
> <!-- 文件域使用场景 上传文件使用的 使用accept可以规定控件能选择的文件类型 accept可以为具体的文件格式 也可以为宽泛的音频/视频/图片等格式 分别对应audio/* video/* image/* -->
> 
> <input type="file" name="" accept="audio/*" id="">
> ```
>
> (其中type=file时 改变value不会将按钮上的值相应地改变)

# 表单元素select :

地点:
``` HTML
<select name="" id="">
 <option value="">湖北</option>
 <option value="">湖南</option>
 <option value="">山东</option>
 <option value="">北京</option>
 <option value="">广东</option>
</select>
```

# 表单元素textarea :

``` HTML
今日反馈:
<textarea>
这是我写的文本！
</textarea>
```

# emmet语法快捷生成html标签

![img](images/KT8HJ8FFGO%255RR3QM}BJUX.png)

# html5

>![](images/image-20220309101520786.png)
section就是大号div
emmet语法:
header+(main>(section>nav+article+aside))+footer

## 视频标签video
>mp4格式是最推荐的，被大多数浏览器支持
常见属性:
![](images/image-20220309111500032.png)
其中preload和controls不常用
https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video


## 音频标签audio
>mp3格式是最推荐的，被大多数浏览器支持
常见属性:
![](images/image-20220309112839908.png)
https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/audio

## input新增type属性值:

> ![](images/image-20220309121005618.png)
在末尾添加`<input type="submit" value="">`，在其他的input输入完毕后点击submit可以自动检测输入的值是否符合它的type类型，例如type: "email";如若没有输入email格式的值再点击submit则会弹出消息框显示输入有误

## input新增表单属性:

> ![](images/image-20220309122103642.png) 
required使用情况较少 placeholder的值是输入框中的默认提示值，当你在输入框中输入文本，则该默认提示值消失，当你删除掉你输入的文本后默认提示值又出现
autofocus使用较少，在百度等搜索引擎页面使用较多，当你进入页面你的光标会自动选定该input表单 
autocomplete大部分情况下应选择off(为了保证数据安全)
multiple `<input type="file" name="" id="">` 默认只能上传一个文件，当你加入multiple="multiple"后可以使用ctrl键上传多个文件



