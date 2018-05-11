* 基础语法
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
 
```
