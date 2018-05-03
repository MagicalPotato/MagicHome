* Python中建议在使用的地方引包是有用意的,假如同样的两个模块A和B都有一个相同的函数setColour,那么当你from A import setColour完之后再from B import setColour那么B的
setColour就会覆盖A的,实际上你用的只是B的.  有一种方式,from A import setColour as AColour,可以这样用.

* 在cmd状态下,输入jps然后回车可以查看当前的Java虚拟机进程和对应的进程ID,然后可以用jstack再打印相应的进程ID就可以看到对应进程的堆栈信息. taskkill /f /im 123可以强
制终止某个进程.  Java的线程实际上并不是线程的实体,真正执行任务的实体是操作系统非配给虚拟机的操作系统线程,每个Java虚拟机线程都会对应一个操作系统的线程,  打印出来
的pid是用16进制表示的,它是真正的线程实体id,就对应你正在查的那个Java虚拟线程的ID.

* Object obj = new Object();obj = null;对于程序员来说，这样手动垃圾回收是一件繁琐二且违背了GC的事情。手动置空不应该需要程序员来做的，因为对于一个简单的对象而言，当调用它的方法执行完毕，引用会自动从栈
中弹出，这样下一次GC的时候就会自动回收这块内存了。但是有例外。比如说缓存，由于缓存对象是程序需要的，那么只要程序正在运行，缓存中的引用是不会被GC的，随着缓存中的
引用越来越多，GC无法回收的对象也越来越多，无法被自动回收。当这些对象需要被回收时，回收这些对象的任务只有交给程序员了，然而这却违背了GC的本质----自动回收。所以Java中引入了弱引用WeakReference。意思是无论内存有没有溢出,下次GC的时候都会回收弱引用对象.还有个软引用。SoftReference（软引用）和WeakReference的区别在于，假
如下次GC的时候，发现内存没有溢出，则不会回收SoftReference关联的对象，但是对于WeakReference，在下一次GC的时候，一定会回收，无论内存是否满。像缓存，其实最适合的个
人认为应该是SoftReference，因为缓存里面的数据是运行时会用到的，能不回收就尽量不要去回收它们，而等到内存实在不够的时候再去回收

* for (int i = 0, length = list.size(); i < length; i++){...}  //注意这段代码,平常写的时候都是直接写size,不会定义变量,但是实际上这种方式是会消耗多余的资源的,因为每次循环都会计算一次size的值,对于次数少的循环来说,可以
不用管,但是对于数量级大的循环,那这个开销肯定是比较大的,所以要注意这个地方.

* 尽量使用HashMap、ArrayList、StringBuilder，除非线程安全需要，否则不推荐使用Hashtable、Vector、StringBuffer，后三者由于使用同步机制而导致了性能开销.

* 循环内不要不断创建对象引用,应该在训话外创建对象的引用,然后在循环内去和不同的对象关联.这样对象 虽然有狠多,但是引用只有一份,省了很多引用的内存.
```
    Object obj = null;
    for (int i = 0; i <= count; i++)
    {    
      obj = new Object();
    }
```

* 除非能确定一整个方法都是需要进行同步的，否则尽量使用同步代码块，避免对那些不需要进行同步的代码也进行了同步，影响了代码执行效率。
* 实现RandomAccess接口的类实例(注意是类的实例,也就是说那个list中放的是实现了随机访问的类的实例)，假如是随机访问的，使用普通for循环效率将高于使用foreach循环；反过来，如果是顺序访问的，则使用Iterator会效率更高。可以使用类似如下的代码作判断：
```
    if (list instanceof RandomAccess)
    {    
        for (int i = 0; i < list.size(); i++){}
    }
    else
    {
        Iterator<?> iterator = list.iterable();
        while (iterator.hasNext()){iterator.next()
    }
```

* 把一个基本数据类型转为字符串，基本数据类型.toString()是最快的方式、String.valueOf(数据)次之、数据+""最慢:String.valueOf()方法底层调用了Integer.toString()方法，但是会在调用前做空判断;Integer.toString()方法就不说了;直接调用了i + ""底层使用了StringBuilder实现，先用append方法拼接，再用toString()方法获取字符串.  注意:循环体内不要使用"+"进行字符串拼接，而直接使用StringBuilder不断append,因为使用"+"在底层执行的时候,还是先创建新的StringBuilder对象然后再append.

* 遍历map效率最高的方式:
```
    public static void main(String[] args)
    {
        HashMap<String, String> hm = new HashMap<String, String>();
        hm.put("111", "222");
        Set<String> keySet = hm.keySet();  //如果只遍历key,用这个最高效.  
        Set<Map.Entry<String, String>> entrySet = hm.entrySet();
        Iterator<Map.Entry<String, String>> iter = entrySet.iterator(); 
        while (iter.hasNext())
        { 
            Map.Entry<String, String> entry = iter.next();
            System.out.println(entry.getKey() + "\t" + entry.getValue());
        }
     }
```

* 计算机内部在存储数字的时候实实际上存储的都是数字对应的二进制的补码,之所以这样用是因为补码可以在通过加法来实现减法运算.

* list和StringBuild底层都是数组,但是他们分配空间的方式和大小是不同的,当内存不够的时候,SB是将当前的数组直接扩大一倍,然后复制老数组并添加新字符串,但是list实际上是把当前数组的大小先拿到,然后把当前数组大小的二进制表示向右移了一位得到一个值然后把这个值和老数组的大小相加得到最后的大小.左移几是乘以2的几次方,右移几是除以2的几次方,相当于是扩容了大约三倍. 无论是SB的数组还是list的数组,底层存的都是对象,并没有区分放进去的东西的类型.我们使用时要指定泛型,只是Java提供的语法,这样会避免很多问题.




























































