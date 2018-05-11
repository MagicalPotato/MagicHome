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
 
 function()
 {
  statement;    //方法或函数;
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
  
##### 一个很重要的概念:元素和节点
* 元素和节点并没有实质性的关联关系,他们只是描述一个html文档的两种不同方式.如果你用元素来描述,那么一个元素就是一个标签,比如我说根元素,那我的意思是在说根标签也就是每个html文件里面的 **<**html**>** 标签.而节点是对html文档模型的另一种表达方式.一个html文档的有三种节点:
  - 元素节点:实际上就是文档中的各个标签,无论是根还是父还是子,就是带尖括号括起来的那个东西(忽略里面的属性和文本).
  - 属性节点:标签里面用来描述这个标签属性的东西.
  - 文本节点:标签里面实际的内容
  
  
  
  
  
  
  
  
  
  
  
666  

