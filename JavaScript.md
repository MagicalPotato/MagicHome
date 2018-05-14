### 基础语法
```
 var srt = "zicuchuan"; 
 var str = 'kekeke';        //使用两种引号很多时候可以省去在其中转义另一种引号的麻烦.同一个文档中所有地方引号尽量保持一致.
 
 var num = 23; 
 var num = 21.5; 
 var num = -19; 
 var num = 0.1225698;
 
 var arr = Array(4); 
 var arr = Array();     //数组,注意A要大写.
 arr[0] = 'liling';
 var names = Array('zhang','wang','li','zhao');     //尽管JavaScript语法并不要求每一行后面加分号,但是加了是一种很好的做法. 
 var names = ['zhang','wang','li','zhao'];   //在JavaScript中[]括起来的是数组并不是Java中的list.
 var arr = ['String',5,true,str,['a',6,false,'false'],'数组'];  //数组可以混杂,可以直接放入变量. str就是一个变量,可以放入别的数组.
 var str = arr[4][3];
 
 var obj = Object();
 obj.name = 'liling';
 obj.age = 24;
 var obj = {name:'liling',age:'24'};   //花括号里面的实际上是JavaScript的对象.
 
 方法名:function()
 {
  statement;    //方法或函数;注意,js中的方法定义是用关键字function(),还要将方法名称加上.类似于Java的方法,是由public等一堆关键字修饰的.
 }
 
 var num = Math.round(2.65);   //四舍五入
 var data = new Data();   //获取当前日期对象
 data.getDay();   //获取当前对象的Day,表示获取星期几
 data.getHours();  //时间
 data.getMonth(); //月份
 
```

### 注意的点
1. 在js中,var a=false;var b=''; 如果非严格比较,那么a和b是相等的,也就是if(a==b){这里会判断为true};如果要严格比较的话,需要用===,或!==,这样不仅比较值,而且会比较类型.
2. 一般情况下,全局的变量在声明时不需要用var,而在局部声明的时候一定将var带上.这样能够便于区分.而且如果一旦有个局部变量和全局变量名字一样,那么可以避免局部变量误改的情况.因为如果局部没有带var则会将同名的全局变量的值改变.
3.
```
<script src="script/functionA.js></script>
<script src="script/functionB.js></script>
<script src="script/functionC.js></script>
```
在html中包含脚本的最佳方式就是通过使用外部文件,这样可以使得标记和脚本能够分离,而且浏览器也能对站点中的多个页面重用缓存过的脚本.但是要尽量避免这样来使用,应该把所有的脚本合并到一个脚本当中,这样可以减少加载页面时发送的请求数量. **而减少请求数量通常是优化性能时优先要考虑的**.
4. 把脚本放在html文档的末尾,**<**/body>标记之前,可以让页面加载的更快.
5.很多html文档最开头都有这样一句话 **<**!DOCTYPE html>.其实这句话的意思是这个文档使用了html5的特性.从html到xhtml,版本的变迁出现了很多小的差别,html5统一了这些差别,可以兼容所有的历史版本并且新增了很多特性,加了这句话就是说这个文档用了最新的标准和特性.

  
##### 一个很重要的概念:元素和节点
* 元素和节点并没有实质性的关联关系,他们只是描述一个html文档的两种不同方式.如果你用元素来描述,那么一个元素就是一个标签,比如我说根元素,那我的意思是在说根标签也就是每个html文件里面的 **<**html**>** 标签.而节点是对html文档模型的另一种表达方式.一个html文档的有三种节点:
  - 元素节点:文档中的各个标签,无论是根还是父还是子,就是带尖括号括起来的那个东西(忽略里面的属性和文本).
  - 属性节点:标签里面用来描述这个标签属性的东西.
  - 文本节点:标签里面实际的内容
  
```
<!DOCTYPE html>
<html lang="en"> 
 <head>  
  <meta charset="utf-8"/>  
  <title>Image Callery</title> 
 </head> 
 <body>  
  <h1>snap shot</h1>  
  <p title="a gentlt reminder">dont forget to buy this stuff.</p>  
  <ul id="wang">
   <li>
    <a herf="pic/abc.jpg" title="A picture"> Fireworks</a>
   </li>
 </body>
</html>
```

- 之前一直以为getElement("body")获取到的是body标签下的所有东西,直接拿个list去接,但看了getElementByTagNam("body")之后发现理解错了.无论方法的名字变成了什么,getElement最终得到的都是一个对象数组(至少在JavaScript中是如此,在Java中或许api返回的是list),假如有多个body标签,那么得到的就是多个对象,这些对象被放在数组当中.现在这个代码里只有一个body标签,那么我得到的就只有一个body对象.然后我用body.childNodes()方法就可以获得这个body标签对象下的所有子节点.这里的子节点指的就是body标签下的所有子标签.之前一直不理解,以为得到一个标签就是得到了这个标签下的所以子标签,而且以为childNodes()方法返回的是body下的所有标签.但是实际上:标签和节点原本就是两个不同的东西.节点是一个大的范围,而标签只是节点当中的一种.所以说用childNodes()方法返回的是所有的节点,包括元素节点,属性节点,文本节点,只要是这个body中的节点,都会返回.
- 而且还有一点要注意,假设现在你获取了p这个标签,你用childNodes()方法获取了其下的所有节点,现在这些节点都在一个数组中,数组中的第一个节点是属性节点,如果我用 node.nodeValue获取该节点的属性值,得到的是title="a gentlt reminder",第二个节点是一个文本节点,这个文本节点的nodeValue是"dont forget to buy this stuff."关键就在这里,但是如果我得到了p对象之后调用p.nodeValue实际上得到的并不是"dont forget to buy this stuff."这句话,而是null,为什么呢.原因还在于对节点的理解,p虽然是个元素节点,但是这个元素节点本身并没有nodeValue,自己没有值,但是p含有别的节点,属性节点和文本节点,属性节点.nodeValue得到的就是那一串属性值,而 文本节点.nodeValue得到的才是那段我最终想要的话.所以用childNodes()的到节点数组后,我要拿到那话应该是node[2].nodeValue.或者node.lastChild.nodeValue.    由此还能得到一个结论,所有标签节点的nodeValue都是null.因为标签节点并没有节点值(这个待验证).  一定要注意区分节点值和节点属性值(节点的属性节点的值).
