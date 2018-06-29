### html
```
<html> 元素：是指从开始标签到结束标签的所有代码。定义整个 HTML 文档
<html>
<body>
<p>This is my first paragraph.</p>
</body>
</html>
这个元素拥有一个开始标签 <html>，以及一个结束标签 </html>。元素内容是另一个 HTML 元素（body 元素）。


HTML属性:属性提供了有关**HTML元素**的更多的信息 ,且总是以键值对的形式出现，比如：name="value",且总是在HTML元素的开始标签中规定。 以下是常见的属性
id 规定元素的唯一 id。
style 规定元素的行内 CSS 样式。
lang 规定元素内容的语言。
class 规定元素的一个或多个类名（引用样式表中的类）。
title 规定有关元素的额外信息。
accesskey 规定激活元素的快捷键。 
contenteditable 规定元素内容是否可编辑。 
contextmenu 规定元素的上下文菜单。上下文菜单在用户点击元素时显示。 
data-* 用于存储页面或应用程序的私有定制数据。 
dir 规定元素中内容的文本方向。 
draggable 规定元素是否可拖动。 
dropzone 规定在拖动被拖动数据时是否进行复制、移动或链接。 
hidden 规定元素仍未或不再相关。  
spellcheck 规定是否对元素进行拼写和语法检查。 
tabindex 规定元素的 tab 键次序。 
translate 规定是否应该翻译元素内容。 

<h1> - <h6>  标题标签
<hr /> 水平分割线 实横线
<!-- This is a comment --> 注释
<p>这是段落</p>
<br /> 用于在段落的文本中换行  直接敲空格没用,浏览器会默认忽略所有的空格
style属性用于给标签添加样式 这种方式是未来的方向,应该及早放弃以前老旧的使用方法. 
<h1 align="center">This is heading 1</h1>  旧方法
<h1 style="text-align:center">This is a heading</h1>  新方法
<p>The heading above is aligned to the center of this page.</p>

<b> 定义粗体文本。 
<big> 定义大号字。 
<em> 定义着重文字。 
<i> 定义斜体字。 
<small> 定义小号字。 
<strong> 定义加重语气。 
<sub> 定义下标字。 
<sup> 定义上标字。 
<ins> 定义插入字。 
<del> 定义删除字。  //以上是文本格式化标签
<code> 定义计算机代码。 可以在里面写代码,这种代码的显示样式和普通文本不同,看着就像是计算机输出的,但这个标签不保留格式,如果想保留,
                       在code里添加一个pre标签.把代码写在pre中.
<kbd> 定义键盘码。 
<samp> 定义计算机代码样本。** 实际上计算机和引用类的标签都是更改了一下文本的样式,这种更改通过style属性也可以实现,但是直接定义好标签更方便一点**
<tt> 定义打字机代码。 
<var> 定义变量。 
<pre> 定义预格式文本。   //以上是"与计算机输出相关"的标签  
<abbr> 定义缩写。 <p><abbr title="World Health Organization">WHO</abbr> 成立于 1948 年。</p> 实际上这个缩写可以不使用标签,
                但对缩写进行标记能够为浏览器、翻译系统以及搜索引擎提供有用的信息
<acronym> 定义首字母缩写。 
<address> 定义地址。元素定义文档或文章的联系信息（作者/拥有者）。相当于是斜体.
<bdo> 定义文字方向。 
<blockquote> 定义长的引用。   浏览器会对长引用添加默认的缩进
<q> 定义短的引用语。    引用语实际上就是加了个双引号,但是这与你在文本中直接加双引号不同,引用的双引号更正规.
<cite> 定义引用、引证。 
<dfn> 定义一个定义项目。  //以上是引用、引用和术语定义
 

<style> 定义样式定义。 当单个文件需要特别样式时，就可以使用内部样式表。你可以在 head 部分通过 <style> 标签定义内部样式表。 
                    当特殊的样式需要应用到个别元素时，就可以使用内联样式。 使用内联样式的方法是在相关的标签中使用样式属性。
<head>
<style type="text/css">
body {background-color: red}
p {margin-left: 20px}
</style>
</head>    //这是内部样式
<p style="color: red; margin-left: 20px">
This is a paragraph
</p>                 //这是内联样式
<link> 定义资源引用。 当样式需要被应用到很多页面的时候，外部样式表将是理想的选择。
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>  //这是全局引用的样式
<div> 定义文档中的节或区域（块级）。 
<span> 定义文档中的行内的小块或区域。 

  
<a href="http://www.w3school.com.cn/" target="_blank">Visit W3School!</a> 
链接标签  使用属性可以定义一些链接的特性,比如这个target="_blank属性就表示链接在新窗口打开

<img src="boat.gif" alt="Big Boat"> 可以使用alt来表示当图片不存在时的信息.如果无法显示图像，将显示 "alt" 属性中的文本
<map> 定义图像地图。 
<area> 定义图像地图中的可点击区域。

在浏览器无法载入图像时，替换文本属性告诉读者她们失去的信息。此时，浏览器将显示这个替代性的文本而不是图像。
为页面上的图像都加上替换文本属性是个好习惯，这样有助于更好的显示信息，并且对于那些使用纯文本浏览器的人来说是非常有用的。
<html>
<body background="/i/eg_background.jpg">  //定义一个背景图像
<h3>图像背景</h3>
<p>gif 和 jpg 文件均可用作 HTML 背景。</p>
<p>如果图像小于页面，图像会进行重复。</p>
</body>
</html>

<p>图像 <img src="/i/eg_cute.gif" align="bottom"> 在文本中</p>  可以在文本中直接插入一个图像  
align属性可以设置图像在文本中居中middle,偏上top还是偏下bottom

<p><img src ="/i/eg_cute.gif" align ="left"> 带有图像的一个段落。图像的 align 属性设置为 "left"。图像将浮动到文本的左侧。</p>
<img src="/i/eg_mouse.jpg" width="200" height="200"> 通过改变 img 标签的 "height" 和 "width" 属性的值，您可以放大或缩小图像。
<p>您也可以把图像作为链接来使用：<a href="/example/html/lastpage.html"><img border="0" src="/i/eg_buttonnext.gif" /></a></p>  
段落中有个链接,而这个链接是个图片

<table> 定义表格  表格由 <table> 标签来定义。每个表格均有若干行（由 <tr> 标签定义），每行被分割为若干单元格（由 <td> 标签定义）。
字母 td 指表格数据（table data），即数据单元格的内容。数据单元格可以包含文本、图片、列表、段落、表单、水平线、表格等等.
可以使用边框属性来显示一个带有边框的表格:<table border="1">
<caption> 定义表格标题。 
<th> 定义表格的表头。  大多数浏览器会把表头显示为粗体居中的文本：
<tr> 定义表格的行。 
<td> 定义表格单元。  <td>&nbsp;</td> 如果表格单元内没有数据,可以用一个占位符站位.这样当表格有网格的时候可以避免这个空单元格的网格不显示
<thead> 定义表格的页眉。 
<tbody> 定义表格的主体。 
<tfoot> 定义表格的页脚。 
<col> 定义用于表格列的属性。 
<colgroup> 定义表格列的组。 
<ol> 定义有序列表。  同样，有序列表也是一列项目，列表项目使用数字进行标记
<ul> 定义无序列表。 无序列表是一个项目的列表，此列项目使用粗体圆点（典型的小黑圆圈）进行标记。
<li> 定义列表项。 
<dl> 定义定义列表。 
<dt> 定义定义项目。 
<dd> 定义定义的描述。 
------------------------------------------------------------------------------
<div> 元素是块级元素，它是可用于组合其他 HTML 元素的容器。没有特定的含义。浏览器会在其前后显示折行。如果与 CSS 一同使用，
<div> 元素可用于对大的内容块设置样式属性。
<div> 元素的另一个常见的用途是文档布局。它取代了使用表格定义布局的老式方法。使用 <table> 元素进行文档布局不是表格的正确用法。
<table> 元素的作用是显示表格化的数据。
<!DOCTYPE html>
<html>
<head>
<style>
.cities {
    background-color:black;
    color:white;
    margin:20px;
    padding:20px;
} 
</style>
</head>
<body>
<div class="cities">   //这个div中的标签就会被显示成统一的样式  如果我在这个div中加入另外的城市,则这些城市会显示在一起,不间断按顺序往下排
<h2>London</h2>
<p>
London is the capital city of England. 
It is the most populous city in the United Kingdom, 
with a metropolitan area of over 13 million inhabitants.
</p>
</div> 
<div class="cities">   //但是我如果添加多个div之后,那么这些城市虽然样式相同但是他们是间断的,不会连续,就是一个城市占页面的一块且样式相同
<h2>London</h2>
<p>
London is the capital city of England. 
It is the most populous city in the United Kingdom, 
with a metropolitan area of over 13 million inhabitants.
</p>
</div> 
</body>
</html>
header 定义文档或节的页眉      //HTML5 提供的新语义元素定义了网页的不同部分：
nav 定义导航链接的容器 
section 定义文档中的节 
article 定义独立的自包含文章 
aside 定义内容之外的内容（比如侧栏） 
footer 定义文档或节的页脚 
details 定义额外的细节 
summary 定义 details 元素的标题 
<!DOCTYPE html>   //这个是使用上面的定义来达到对页面进行布局的例子
<html>
<head>
<style>
#header {
    background-color:black;
    color:white;
    text-align:center;
    padding:5px;
}
#nav {
    line-height:30px;
    background-color:#eeeeee;
    height:300px;
    width:100px;
    float:left;
    padding:5px;       
}
#section {
    width:350px;
    float:left;
    padding:10px;    
}
#footer {
    background-color:black;
    color:white;
    clear:both;
    text-align:center;
   padding:5px;    
}
</style>
</head>
<body>
<div id="header">
<h1>City Gallery</h1>
</div>
<div id="nav">
London<br>
Paris<br>
Tokyo<br>
</div>
<div id="section">
<h2>London</h2>
<p>
London is the capital city of England. It is the most populous city in the United Kingdom,
with a metropolitan area of over 13 million inhabitants.
</p>
<p>
Standing on the River Thames, London has been a major settlement for two millennia,
its history going back to its founding by the Romans, who named it Londinium.
</p>
</div>
<div id="footer">
Copyright ? W3Schools.com
</div>
</body>
</html>

<!DOCTYPE html> //这个是使用表格对页面进行布局
<html>
<head>
<style>
table.lamp {
    width:100%;
    border:1px solid #d4d4d4;
}
table.lamp th, td { 
    padding:10px;
}
table.lamp th {
    width:40px;
}
</style>
</head>
<body>
 
<table class="lamp">
<tr>
  <th>
    <img src="/images/lamp.jpg" alt="Note" style="height:32px;width:32px">
  </th>
  <td>
    The table element was not designed to be a layout tool.
  </td>
</tr>
</table>
</body>
</html>
-------------------------------
<span> 元素是内联元素，可用作文本的容器。也没有特定的含义。当与 CSS 一同使用时，<span> 元素可用于为部分文本设置样式属性。

<frameset> 框架(窗口)结构标签（<frameset>）定义如何将窗口分割为框架. 每个 frameset 定义了一系列行或列. 
rows/columns 的值规定了每行或每列占据屏幕的面积
<frame> 标签定义了放置在每个框架中的 HTML 文档。  
在下面的这个例子中，我们设置了一个两列的框架集。第一列被设置为占据浏览器窗口的 25%。第二列被设置为占据浏览器窗口的 75%。
HTML 文档 "frame_a.htm" 被置于第一个列中，而 HTML 文档 "frame_b.htm" 被置于第二个列中：
<frameset cols="25%,75%">
   <frame src="frame_a.htm">
   <frame src="frame_b.htm">
</frameset>
假如一个框架有可见边框，用户可以拖动边框来改变它的大小。为了避免这种情况发生，可以在 <frame> 标签中加入：noresize="noresize"。
 
<ifram> 内联框架(子窗口)标签 <iframe src="demo_iframe.htm" width="200" height="200" frameborder="0"></iframe>  
可以将一个网页嵌入当前页面中 frameborder属性可选.设置为0可以移除边框
<!DOCTYPE html>
<html>
<body>
<iframe src="/example/html/demo_iframe.html" name="iframe_a"></iframe>
<p><a href="http://www.w3school.com.cn" target="iframe_a">W3School.com.cn</a></p>
<p><b>注释：</b>由于链接的目标匹配 iframe 的名称，所以链接会在 iframe 中打开。</p>
</body>
</html>

<script> 标签用于定义客户端脚本，比如 JavaScript。
script 元素既可包含脚本语句，也可通过 src 属性指向外部脚本文件。
<noscript> 标签提供无法使用脚本时的替代内容，比方在浏览器禁用脚本时，或浏览器不支持客户端脚本时。
只有在浏览器不支持脚本或者禁用脚本时，才会显示 noscript 元素中的内容：
<script type="text/javascript">
document.write("Hello World!")
</script>
<noscript>Your browser does not support JavaScript!</noscript>

<head> 元素是所有头部元素的容器。<head> 内的元素可包含脚本，指示浏览器在何处可以找到样式表，提供元信息，等等。
以下标签都可以添加到 head 部分：<title>、<base>、<link>、<meta>、<script> 以及 <style>。
<title> 标签定义文档的标题。在所有 HTML/XHTML 文档中都是必需的。
能够定义浏览器工具栏中的标题.提供页面被添加到收藏夹时显示的标题.显示在搜索引擎结果中的页面标题
<base> 标签为页面上的所有链接规定默认地址或默认目标（target）：
<head>
<base href="http://www.w3school.com.cn/images/" />
<base target="_blank" />
</head>
<link> 标签定义文档与外部资源之间的关系。最常用于连接样式表：
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css" />
</head>
<style> 标签用于为 HTML 文档定义样式信息。
<head>
<style type="text/css">
body {background-color:yellow}
p {color:blue}
</style>
</head>

<meta> 标签提供关于 HTML 文档的元数据。元数据不会显示在页面上，但是对于机器是可读的。
典型的情况是，meta 元素被用于规定页面的描述、关键词、文档的作者、最后修改时间以及其他元数据。
<meta> 标签始终位于 head 元素中。元数据可用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他 web 服务。
针对搜索引擎的关键词一些搜索引擎会利用 meta 元素的 name 和 content 属性来索引您的页面。
下面的 meta 元素定义页面的描述：
<meta name="description" content="Free Web tutorials on HTML, CSS, XML" />
下面的 meta 元素定义页面的关键词：name 和 content 属性的作用是描述页面的内容。
<meta name="keywords" content="HTML, CSS, XML" />
常用字符的html表示! 实体名称对大小写敏感！
  空格 &nbsp; &#160; 
< 小于号 &lt; &#60; 
> 大于号 &gt; &#62; 
& 和号 &amp; &#38; 
" 引号 &quot; &#34; 
' 撇号  &apos; (IE不支持) &#39;
× 乘号 &times; &#215; 
÷ 除号 &divide; &#247; 
**URL 编码**   之前以为带%那一串东西是被加密了,现在看来网址会被转换成ascii编码而不是进行了加密 .看来可以通过对照编码表对浏览器的url内同进行解析
URL 只能使用 ASCII 字符集来通过因特网进行发送。
由于 URL 常常会包含 ASCII 集合之外的字符，URL 必须转换为有效的 ASCII 格式。
URL 编码使用 "%" 其后跟随两位的十六进制数来替换非 ASCII 字符。
URL 不能包含空格。URL 编码通常使用 + 来替换空格。

 
颜色由一个十六进制符号来定义，这个符号由红色、绿色和蓝色的值组成（RGB）。每种颜色的最小值是0（十六进制：#00）。最大值是255（十六进制：#FF）。
  #000000 rgb(0,0,0)  黑
  #FF0000 rgb(255,0,0) 红
  #00FF00 rgb(0,255,0) 绿
  #0000FF rgb(0,0,255) 深蓝
  #FFFF00 rgb(255,255,0) 黄
  #00FFFF rgb(0,255,255) 浅蓝
  #FF00FF rgb(255,0,255) 橙
  #C0C0C0 rgb(192,192,192) 灰
  #FFFFFF rgb(255,255,255) 白
仅仅有 16 种颜色名被 W3C 的 HTML4.0 标准所支持。
它们是：aqua, black, blue, fuchsia, gray, green, lime, maroon, navy, olive, purple, red, silver, teal, white, yellow。
如果需要使用其它的颜色，需要使用十六进制的颜色值

<!DOCTYPE> 不是 HTML 标签。它为浏览器提供一项信息（声明），即 HTML 是用什么版本编写的。 以下是常用的声明.未来估计只会存在第一种
HTML5
<!DOCTYPE html>
HTML 4.01
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
XHTML 1.0
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

ＸＨＴＭＬ:更高级更简洁的html,和xml共通.通过结合 XML 和 HTML 的长处，开发出了 XHTML。
XHTML 是作为 XML 被重新设计的 HTML。XHTML 文档必须进行 XHTML 文档类型声明（XHTML DOCTYPE declaration）。
XHTML 元素是以 XML 格式编写的 HTML 元素。元素必须正确嵌套.元素必须始终关闭.元素必须小写..**文档必须有一个根元素** 
<html>、<head>、<title> 以及 <body> 元素也必须存在，并且必须使用 <html> 中的 xmlns 属性为文档规定 xml 命名空间。
html最终会被xhtml取代但是xml不会
下面的例子展示了带有最少的必需标签的 XHTML 文档：
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>Title of document</title>
</head>
<body>
......
</body>
</html>
如何从 HTML 转换到 XHTML 
1.向每张页面的第一行添加 XHTML <!DOCTYPE>
2.向每张页面的 html 元素添加 xmlns 属性 ,且属性不能被简写
3.把所有元素名改为小写
4.关闭所有空元素
5.把所有属性名改为小写
6.为所有属性值加引号
------------------------------------
<form> HTML表单元素. 用于收集用户输入。表单包含表单元素。表单元素指的是不同类型的 input 元素、复选框、单选按钮、提交按钮等等.
<input> 元素是最重要的表单元素。
<input type="text"> 定义用于文本输入的单行输入字段 .请注意表单本身是不可见的。同时请注意文本字段的默认宽度是 20 个字符。
<input type="radio"> 定义单选按钮。 单选按钮允许用户在有限数量的选项中选择其中之一：
<input type="submit"> 定义用于向表单处理程序（form-handler）提交表单的按钮。表单处理程序通常是包含用来处理输入数据的脚本的服务器页面。
表单处理程序在表单的 action 属性中指定
<input type="password"> 定义密码字段 如果将类型设置成密码,那么页面显示的时候就会隐藏密码而用圆点取代
<input type="number"> 校验是不是数字
<input type="email"> 会校验是不是邮箱
<!DOCTYPE html>
<html>
<body>
<form action="/demo/demo_form.asp" method="GET">
First name:<br>
<input type="text" name="firstname" value="Mickey" readonly>  
//如果要正确地被提交，每个输入字段必须设置一个 name 属性。如果去了,那么这个属性时不会被提交的
<br>                                                         //readonly 属性表示这个默认属性不能被修改,是只读的
<input type="text" name="firstname" value="Mickey" size="40" maxlength="10" disabled>  
//如果是disabled属性,则这个整个框子连同默认值都会变成灰色,不能点,相当于被禁用                                                         
Last name:<br>                                   //size是可输入最大字符数  maxlength是最大字符串长度
<input type="text" name="lastname" value="Mouse">haha</input> //value属性是输入框里面的默认值  haha会显示在输入框外面的右面
<br><br>
<input type="submit" value="Submit">
</form> 
<p>如果您点击提交，表单数据会被发送到名为 demo_form.asp 的页面。 
如果省略 action 属性，则 action 会被设置为当前页面。method 属性规定在提交表单时所用的 HTTP 方法（GET 或 POST）默认是GET</p>
<p>当您使用 GET 时，表单数据在页面地址栏中是可见的：action_page.php?firstname=Mickey&lastname=Mouse ,GET最适合少量数据的提交。
浏览器会设定容量限制。但是如果表单正在更新数据，或者包含敏感信息（例如密码)推荐使用POST ,安全性更好，且地址栏中被提交的数据是不可见的</p>
</body>
</html>
HTML5 增加了多个新的输入类型：color date datetime datetime-local month range search tel time url week  
但老式 web 浏览器不支持的输入类型，会被视为输入类型 text。
这些乱七八糟的格式实际上并不常用,因为你输入的都是字符串,都可以用text代替,如果不是有特殊的要求,用都可以用text,所以不必过分关注只要知道就行了
还是在form中:<select> 元素定义下拉列表,<option> 元素定义待选择的选项。
<form action="/demo/demo_form.asp">
<select name="cars">
<option value="volvo">Volvo</option>
<option value="saab">Saab</option>
<option value="fiat" selected>Fiat</option>
<option value="audi">Audi</option>
</select>

<textarea name="message" rows="10" cols="30">  //<textarea> 元素定义多行输入字段（就是一个能写文本的框子,可以滚动）大小自己设置
The cat was playing in the garden.
</textarea>
<button type="button" onclick="alert('Hello World!')">点击我！</button>  可以在form中也可以单独用
<form action="/demo/demo_form.asp">
<input list="browsers" name="browser">  //<datalist> 元素为 <input> 元素规定预定义选项列表。用户会在他们输入数据时看到预定义选项的下拉列表。
<datalist id="browsers">               //也就是说显示一个白色输入框,当用户点击一下白框会自动出现几个待选项,就跟浏览器的搜索框一样
  <option value="Internet Explorer">  //<input> 元素的 list 属性必须引用 <datalist> 元素的 id 属性。
  <option value="Firefox">
  <option value="Chrome">
  <option value="Opera">
  <option value="Safari">
</datalist>
<input type="submit">
</form>
<form action="action_page.php" method="GET" target="_blank" accept-charset="UTF-8" 
ectype="application/x-www-form-urlencoded" autocomplete="off" novalidate>
......
</form>   //这是一个form标签的所有属性
accept-charset 规定在被提交表单中使用的字符集（默认：页面字符集）。 
action 规定向何处提交表单的地址（URL）（提交页面）。 
autocomplete 规定浏览器应该自动完成表单（默认：开启）。 
enctype 规定被提交数据的编码（默认：url-encoded）。 
method 规定在提交表单时所用的 HTTP 方法（默认：GET）。 
name 规定识别表单的名称（对于 DOM 使用：document.forms.name）。 //就是上面说的那个不可省略,如果省略就无法提交的那个name
novalidate 规定浏览器不验证表单。 
target 规定 action 属性中地址的目标（默认：_self）。 
autocomplete 属性规定表单或输入字段是否应该自动完成。当自动完成开启，浏览器会基于用户之前的输入值自动填写值。
可以把表单的 autocomplete 设置为 on，同时把特定的输入字段设置为 off，反之亦然。
autocomplete 属性适用于 <form> 以及如下 <input> 类型：text、search、url、tel、email、password、datepickers、range 以及 color。
自动完成开启的 HTML 表单（某个输入字段为 off）： 自动填充用户名和密码应该用的就是这种方式
<form action="action_page.php" autocomplete="on">   
   First name:<input type="text" name="fname"><br>
   Last name: <input type="text" name="lname"><br>
   E-mail: <input type="email" name="email" autocomplete="off"><br>
   <input type="submit">
</form>
 
<form action="action_page.php" novalidate="novalidate"> //novalidate属性用于使input中的属性失效. 也就是不对表单数据进行验证 . 
<input type="email" name="user_email" />  //虽然input是的类型是email,但是并不会对其验证,写啥都行. 
有时候可能日后需要验证,但是当前并不需要,那么用这个属性很方便
<input type="email" name="user_email" pattern="[A-z]{3}"/> //pattern属性用于验证正则表达式.-
form中重要和常用的属性都在这里的,还有一些不常见的实际使用时再查吧
------------------------------------
以下 HTML 4.01 元素已从 HTML5 中删除：<acronym><applet><basefont><big><center><dir><font><frame><frameset><noframes><strike><tt>
HTML5 <section> 元素 定义文档中的节。是有主题的内容组，通常具有标题。可以将网站首页划分为简介、内容、联系信息等节。
HTML5 <article> 元素 规定独立的自包含内容。文档有其自身的意义，并且可以独立于网站其他内容进行阅读。 
HTML5 <header> 元素 为文档或节规定页眉。应该被用作介绍性内容的容器。一个文档中可以有多个 <header> 元素。 
HTML5 <footer> 元素为文档或节规定页脚。页脚通常包含文档的作者、版权信息、使用条款链接、联系信息等等。您可以在一个文档中使用多个 <footer> 元素。
HTML5 <nav> 元素定义导航链接集合。旨在定义大型的导航链接块。不过，并非文档中所有链接都应该位于 <nav> 元素中！
<nav>
<a href="/html/">HTML</a> |
<a href="/css/">CSS</a> |
<a href="/js/">JavaScript</a> |
<a href="/jquery/">jQuery</a>
</nav>
HTML5 <figure> 和 <figcaption> 元素 在书籍和报纸中，与图片搭配的标题很常见。
标题（caption）的作用是为图片添加可见的解释。图片和标题能够被组合在 <figure> 元素中：
<figure>
   <img src="pic_mountain.jpg" alt="The Pulpit Rock" width="304" height="228"> //<img> 元素定义图像，<figcaption> 元素定义标题。
   <figcaption>Fig1. - The Pulpit Pock, Norway.</figcaption>
</figure>
html5将以往的html中的一些标签语义化了,把每个标签有了明确的意义,这样就省去了传统的用一个标签然后搞一大堆属性的麻烦.同事html使用更简洁更灵活.
请始终对图像使用 alt 属性。当图像无法显示时该属性很重要。请始终定义图像尺寸。这样做会减少闪烁，因为浏览器会在图像加载之前为图像预留空间。
<img src="html5.gif" alt="HTML5" style="width:128px;height:128px">
请勿毫无理由地增加空行。请增加空行来分隔大型或逻辑代码块。请增加两个空格的缩进。请勿使用 TAB。请勿使用没有必要的空行和缩进。
没有必要在短的和相关项目之间使用空行，也没有必要缩进每个元素：
表格示例：
<table>
  <tr>                 
    <th>Name</th>
    <th>Description</th>               //tr :table row缩写  th:table header缩写  td:table date缩写
  <tr>
  <tr>
    <td>A</td>
    <td>Description of A</td>
  <tr>
  <tr>
    <td>B</td>
    <td>Description of B</td>
  <tr>
</table>
列表示例：
<ol>
  <li>LondonA</li>       //ol:order list   li:list 代表一列
  <li>Paris</li>
  <li>Tokyo</li>
</ol>
<title> 元素在 HTML5 中是必需的。请尽可能制作有意义的标题 <title>HTML5 Syntax and Coding Style</title>
为了确保恰当的解释，以及正确的搜索引擎索引，在文档中对语言和字符编码的定义越早越好：
<!DOCTYPE html>
<html lang="en-US">
<head>
  <meta charset="UTF-8">
  <title>HTML5 Syntax and Coding Style</title>
</head>
请使用简单的语法来链接样式表（type 属性不是必需的）：
<link rel="stylesheet" href="styles.css">
短规则可以压缩为一行，就像这样：
p.into {font-family:"Verdana"; font-size:16em;}
长规则应该分为多行：
body {
  background-color: lightgrey;
  font-family: "Arial Black", Helvetica, sans-serif;
  font-size: 16em;
  color: black;
}
开括号与选择器位于同一行 ,在开括号之前用一个空格, 使用两个字符的缩进 ,在每个属性与其值之间使用冒号加一个空格, 在每个逗号或分号之后使用空格 
在每个属性值对（包括最后一个）之后使用分号 ,只在值包含空格时使用引号来包围值, 把闭括号放在新的一行之前且不用空格, 避免每行超过 80 个字符
注释：在逗号或分号之后添加空格，是所有书写类型的通用规则。
HTML 文件名应该使用扩展名 .html（而不是 .htm）。CSS 文件应该使用扩展名 .css。JavaScript 文件应该使用扩展名 .js
HTML5的<canvas>元素使用JavaScript 在网页上绘制图像。画布是一个矩形区域，您可以控制其每一像素。
canvas 拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法。
canvas 元素本身是没有绘图能力的。所有的绘制工作必须在 JavaScript 内部完成：
<canvas id="myCanvas" width="200" height="100"></canvas>
<script type="text/javascript">
var c=document.getElementById("myCanvas");
var cxt=c.getContext("2d");
cxt.fillStyle="#FF0000";
cxt.fillRect(0,0,150,75);
</script>
JavaScript 使用 id 来寻找 canvas 元素：var c=document.getElementById("myCanvas");
然后，创建 context 对象：var cxt=c.getContext("2d"); getContext("2d") 对象是内建的 HTML5 对象，
拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法。
Canvas 和 SVG 都允许您在浏览器中创建图形，但是它们在根本上是不同的。
SVG 是一种使用 XML 描述 2D 图形的语言。SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。
在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。
Canvas Canvas 通过 JavaScript 来绘制 2D 图形。Canvas 是逐像素进行渲染的。在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。
如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。
SVG 不依赖分辨率,支持事件处理器,最适合带有大型渲染区域的应用程序（比如谷歌地图）,复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）,
不适合游戏应用
Canvas 依赖分辨率,不支持事件处理器,弱的文本渲染能力,能够以 .png 或 .jpg 格式保存结果图像,最适合图像密集型的游戏，其中的许多对象会被频繁重绘
<object> 的作用是支持 HTML 助手（插件）。插件有很多用途：播放音乐、显示地图、验证银行账号，控制输入等等。
可使用 <object> 或 <embed> 标签来将插件添加到 HTML 页面。
<embed height="100" width="100" src="song.mp3" />  //两个功能都一样
<embed height="100" width="100" src="song.mp3" />  //会在页面绘制一个长100宽100的播放器点击播放按钮之后可以播放
<audio> 标签也可以播放音频,但是并不是借助插件. <object> 或 <embed>是通过插件来播放的,如果你的机器上并没有相关的插件则不会起作用.
<audio>元素在老式浏览器中不起作用。
最好的 HTML音频 解决方法: HTML5 <audio> 元素会尝试以 mp3 或 ogg 来播放音频。如果失败，代码将回退尝试 <embed> 元素。
<audio controls="controls" height="100" width="100">
  <source src="song.mp3" type="audio/mp3" />
  <source src="song.ogg" type="audio/ogg" />
<embed height="100" width="100" src="song.mp3" />
</audio>
最好的视屏解决方法.HTML 5 <video> 元素会尝试播放以 mp4、ogg 或 webm 格式中的一种来播放视频。如果均失败，则回退到 <embed> 元素。
<video width="320" height="240" controls="controls">
  <source src="movie.mp4" type="video/mp4" />
  <source src="movie.ogg" type="video/ogg" />
  <source src="movie.webm" type="video/webm" />     // 和音频一样,用<object> 或 <embed>也可以播放视频,但是都是插件.
  而video是html本身的不借助插件的视频播放标签
  <object data="movie.mp4" width="320" height="240">
    <embed src="movie.swf" width="320" height="240" />
  </object>
</video>
----------------------
HTML 本地存储提供了两个在客户端存储数据的对象：
window.localStorage - 存储没有截止日期的数据
window.sessionStorage - 针对一个 session 来存储数据（当关闭浏览器标签页时数据会丢失）
// Check browser support
if (typeof(Storage) !== "undefined") {
    // Store
    localStorage.setItem("lastname", "Gates");
    // Retrieve
    document.getElementById("result").innerHTML = localStorage.getItem("lastname");
} else {
    document.getElementById("result").innerHTML = "抱歉！您的浏览器不支持 Web Storage ...";
}
</script>
<!DOCTYPE html>    //使用本地存储来计数  这个数据是本地永久保存的
<html>
<head>
<script>
function clickCounter() {
    if(typeof(Storage) !== "undefined") {
        if (localStorage.clickcount) {
            localStorage.clickcount = Number(localStorage.clickcount)+1;
        } else {
            localStorage.clickcount = 1;
        }
        document.getElementById("result").innerHTML = "您已经点击这个按钮 " + localStorage.clickcount + " 次。";
    } else {
        document.getElementById("result").innerHTML = "抱歉！您的浏览器不支持 Web Storage ...";
    }
}
</script>
</head>
<body>
<p><button onclick="clickCounter()" type="button">请点击这里！</button></p>
<div id="result"></div>
<p>请点击按钮使计数器递增。</p>
<p>请关闭浏览器或标签页，然后再试一次，计数器将继续计数（不会重置）。</p>
</body>
</html>

<!DOCTYPE html>  //使用sessionStorage来存储.sessionStorage 对象等同 localStorage 对象，如果用户关闭具体的浏览器标签页，数据也会被删除。
<html>
<head>
<script>
function clickCounter() {
    if(typeof(Storage) !== "undefined") {
        if (sessionStorage.clickcount) {
            sessionStorage.clickcount = Number(sessionStorage.clickcount)+1;
        } else {
            sessionStorage.clickcount = 1;
        }
        document.getElementById("result").innerHTML = "在本 session 中，您已经点击这个按钮 " + sessionStorage.clickcount + " 次。";
    } else {
        document.getElementById("result").innerHTML = "抱歉！您的浏览器不支持 Web Storage ...";
    }
}
</script>
</head>
<body>
<p><button onclick="clickCounter()" type="button">请点击这里</button></p>
<div id="result"></div>
<p>请点击按钮使计数器递增。</p>
<p>请关闭浏览器或标签页，然后再试一次，计数器会重置。</p>
</body>
</html>
-------------------
如需启用应用程序缓存，请在文档的 <html> 标签中包含 manifest 属性：
<!DOCTYPE HTML>
<html manifest="demo.appcache">
...
</html>

每个指定了 manifest 的页面在用户对其访问时都会被缓存。如果未指定 manifest 属性，则页面不会被缓存（除非在 manifest 文件中直接指定了该页面）。
manifest文件的建议文件扩展名是：".appcache"。
注意：manifest 文件需要设置正确的 MIME-type，即 "text/cache-manifest"。必须在 web 服务器上进行配置。
manifest 文件是简单的文本文件，它告知浏览器被缓存的内容（以及不缓存的内容）。有三个部分：
CACHE MANIFEST - 在此标题下列出的文件将在首次下载后进行缓存
NETWORK - 在此标题下列出的文件需要与服务器的连接，且不会被缓存
FALLBACK - 在此标题下列出的文件规定当页面无法访问时的回退页面（比如 404 页面）
第一行，CACHE MANIFEST，是必需的：
CACHE MANIFEST
/theme.css
/logo.gif
/main.js 
上面的 manifest 文件列出了三个资源：一个 CSS 文件，一个 GIF 图像，以及一个 JavaScript 文件。
当 manifest 文件被加载后，浏览器会从网站的根目录下载这三个文件。然后，无论用户何时与因特网断开连接，这些资源依然可用。
```
