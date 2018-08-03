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

* list和StringBuild底层都是数组,但是他们分配空间的方式和大小是不同的,当内存不够的时候,SB是将当前的数组直接扩大一倍,然后复制老数组并添加新字符串,但是list是把当前数组的大小先拿到,然后把当前数组大小的二进制表示向右移了一位得到一个值然后把这个值和老数组的大小相加得到新数组的大小.左移几是乘以2的几次方,右移几是除以2的几次方,相当于是额外增加了老数组的一半大小. 无论是SB的数组还是list的数组,底层存的都是对象,并没有区分放进去的东西的类型.我们使用时要指定泛型,只是Java提供的语法,这样会避免很多问题.

* 将一个字符串压缩
```
 public static void main(String[] args)
 {
        String str = "fffffffhg";
        StringBuffer result = new StringBuffer();  
        char temp = str.charAt(0);  
        int sum = 1;  
        for (int i = 1; i < str.length(); i++)  {  
            char c = str.charAt(i);  
            if (temp == c)  
            {  
                sum++;  
                continue;  
            } 
            else
            {
             result.append(sum).append(temp);  
             temp = c;  
             sum = 1;  
            }
        }  
        result.append(sum).append(temp);  
        System.out.println(result.toString());  
 }
```

* PO和POJO的区别: ＰＯ和ＰＯＪＯ都是持久化的对象．也就是我们平常在代码里见到的那种里面只有属性和ｇｅｔ，ｓｅｔ方法的类产生的对象．我们通常用这种对象来存储数据,然后将这个对象的字段对应到数据库的表字段上，这样就能直接将这个对象里的数据入库．但是两者也有区别．ＰＯＪＯ相对于ＰＯ来说更简洁，POJO是代码来管理的,ＰＯＪＯ对象是你在代码里创建和使用，用的是ｎｅｗ关键字，最后由ＧＣ来回收．而PO是数据库来管理,PO中会增加一些用来管理数据库entity状态的属性和方法。创建和管理都不是程序，而是数据库，在ｉｎｓｅｒｔ的时候创建一个ＰＯ对象，在ｄｅｌｅｔｅ的时候这个对象被删除，基本和数据库的生命周期相关．另外ＰＯ对象往往只能存在一个数据库Connection之中，Connnection关闭以后，持久对象就不存在了，而POJO只要不被GC回收，总是存在的。

* 使用集合转数组的方法，必须使用集合的```toArray(T[] array)```，传入的是类型完全一样的数组，大小就是list.size()。
说明：使用toArray带参方法，入参分配的数组空间不够大时，toArray方法内部将重新分配内存空间，并返回新数组地址；如果数组元素个数大于实际所需，下标为```[ list.size() ]```的数组元素将被置为null，其它数组元素保持原值，因此最好将方法入参数组大小定义与集合元素个数一致.
```
    正例:
    List<String> list = new ArrayList<String>(2);
    list.add("guan");
    list.add("bao");
    String[] array = new String[list.size()];
    array = list.toArray(array);
    反例：直接使用toArray无参方法存在问题，此方法返回值只能是Object[]类，若强转其它类型数组将出现ClassCastException错误。
```

* 使用工具类Arrays.asList()把数组转换成集合时，不能使用其修改集合相关的方法，它的add/remove/clear方法会抛出UnsupportedOperationException异常。
 说明：asList的返回对象是一个Arrays内部类，并没有实现集合的修改方法。Arrays.asList体现的是适配器模式，只是转换接口，后台的数据仍是数组。
```
    String[] str = new String[] { "you", "wu" }; 
    List list = Arrays.asList(str); 
    第一种情况：list.add("yangguanbao"); 运行时异常。 
    第二种情况：str[0] = "gujin"; 那么list.get(0)也会随之修改。
```

* 不要在foreach循环里进行元素的remove/add操作。remove元素请使用Iterator方式，如果并发操作，需要对Iterator对象加锁。
```
    List<String> list = new ArrayList<>();
    list.add("1");
    list.add("2");
    Iterator<String> iterator = list.iterator();
    while (iterator.hasNext()) {
        String item = iterator.next();
        if (删除元素的条件) {
            iterator.remove();
        }
    }
```

* 集合初始化时，指定集合初始值大小。 说明：HashMap使用HashMap(int initialCapacity) 初始化。hashMap在赋值时会先判断给定容量是否大于最大容量2的30次方，1<<30 =（1073741824,1左移30位就是2的30次方），如果大于此数初始化容量赋值为1<<30，如果小于此数，调用tableSizeFor方法使用位运算将初始化容量修改为2的次方数，都是向大的方向运算.比如输入13，小于2的4次方，那面计算出来桶的初始容量就是16.所以说只要输入小于16的初始容量,最终都会被初始化成16的容量.如果不指定初始容量,默认分配的就是16 (1<<4,1左移4位,相当于2的4次方)当指定的容量大于16时候,最好先估计大小,然后一次性指定,不然重新resize是很耗费内存的操作.指定为0完全就是个废物操作.

* hashMap的ReSize不是简单的把长度扩大，而是经过一下两个步骤： 
  1. 扩容:    创建一个新的Entry空数组，长度是原hashMap数组的2倍； 
  2. ReHash:  因为长度变长，Hash的规则也随之改变了。所以要遍历原Entry数组，把所有的Entry重新Hash到新的数组。

* 合理利用好集合的稳定性(order)和有序性(sort)，稳定性指集合每次遍历的元素次序是一定的。有序性是指遍历的结果是按某种比较规则依次排列的。
  * ArrayList是order/unsort  固序遍历,乱序保存
  * HashMap是unorder/unsort  乱序遍历,乱序保存
  * TreeSet是order/sort      固序遍历,固序保存

* 线程资源必须通过线程池提供，不允许在应用中自行显式创建线程。
* 执行时间开销很大的方法或者需要极高稳定性和可用性的方法的时候最好提前进行参数校验.因为参数校验时间几乎可以忽略不计，但如果因为参数错误导致中间执行回退，或者错误，那得不偿失.
* Math.random() 这个方法返回是double类型，取值的范围 0≤x<1(能够取到零值，注意除零异常)如果想获取整数类型的随机数，不要将x放大10的若干倍然后取整，直接使用Random对象的nextInt或者nextLong方法。
* 任何数据结构的构造或初始化，都应指定大小，避免数据结构无限增长吃光内存。
*  HashMap在容量不够进行resize时由于高并发可能出现死链，导致CPU飙升，在开发过程中可以使用其它数据结构或加锁来规避此风险。

*  java中hashMap的默认大小为什么是2的幂次: 在hashmap的源码中。put方法会调用indexFor(int h, int length)方法，这个方法主要是根据key的hash值
找到这个entry在table中的位置.注意最后return的是h&(length-1)。如果length不为2的幂，比如15。那么length-1的2进制就会变成1110。在hashcode为随机数的情况下，和1110做&操作。尾数永远为0。那么0001、1001、1101等尾数为1的位置就永远不可能被entry占用。这样会造成浪费，不随机等问题。源码如下：
```
    static int indexFor(int hashCode, int length) {
        return hashCode & (length-1);
    }
```

* 不要使用count(列名)或count(常量)来替代count(_*_)，count(_*_)是SQL92定义的标准统计行数的语法，跟数据库无关，跟NULL和非NULL无关。但是注意count(_*_)会统计值为NULL的行，而count(列名)不会统计此列为NULL值的行。

* 禁止使用存储过程，存储过程难以调试和扩展，更没有移植性。
* 不要写一个大而全的数据更新接口。传入为POJO类，不管是不是自己的目标更新字段，都进行update 这是不对的。执行SQL时，不要更新无改动的字段，一是易出错；二是效率低；三是增加binlog存储。

* 如果你需要在一个在线的网站上去执行一个大的 DELETE 或 INSERT 查询，你需要非常小心，要避免你的操作让你的整个网站停止相应。因为这两个操作是会锁表的，
表一锁住了，别的操作都进不来了。如果你把你的表锁上一段时间，比如30秒钟，那么对于一个有很高访问量的站点来说，这30秒所积累的访问进程/线程，数据库链接，
打开的文件数，可能不仅仅会让你的WEB服务崩溃，还可能会让你的整台服务器马上挂了。所以，如果你有一个大的DELETE 或INSERT 语句，批量提交SQL语句你一定把其
拆分，使用 LIMIT oracle(rownum),sqlserver(top)条件是一个好的方法。
```
while(1){
 　　//每次只做1000条
　　 mysql_query(“delete from logs where log_date <= ’2012-11-01’ limit 1000”);
 　　if(mysql_affected_rows() == 0){
　　 　　//删除完成，退出！
　　 　　break；
　　 }
    //每次暂停一段时间，释放表让其他进程/线程访问。
    usleep(50000)
}
```

* 如果在主线程中创建一个子线程，默认情况下这两个线程同属于一个线程组，如果子线程发生异常，主线程可以直接使用try catch捕获的到。同样是在主线程中创建
一个子线程，如果声明了这个子线程是另一个线程组的，即调用了new Thread(ThreadGroup group, Runnable target)，则主线程中是无法直接捕获到子线程的发生
的异常的，不过可以通过声明一个线程组重写uncaughtException，然后把子线程放进去。但是不管怎么说，在主线程中捕获子线程的异常一般是不推荐的，线程设计
的理念：“线程的问题应该线程自己本身来解决，而不要委托到外部。”

* 大批量数据入库优化:
 
  1. 合并插入
```
INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0325', '0326', '0327', 1);
INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0425', '0426', '0427', 2);
修改成:
INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0', 'userid_0', 'content_0', 0), 
('1', 'userid_1','content_1', 1),('2', 'userid_2','content_2', 2);
修改后的插入操作能够提高程序的插入效率。这里第二种SQL执行效率高的主要原因是合并后日志量（MySQL的binlog和innodb的事务让日志）减少了，
降低日志刷盘的数据量和频率，从而提高效率。通过合并SQL语句，同时也能减少SQL语句解析的次数，减少网络传输的IO。
```
  2. 在事务中进行插入处理
```
START TRANSACTION;
INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('0', 'userid_0', 'content_0', 0);
INSERT INTO `insert_table` (`datetime`, `uid`, `content`, `type`) VALUES ('1', 'userid_1', 'content_1', 1);
...
COMMIT;
使用事务可以提高数据的插入效率，这是因为进行一个INSERT操作时，MySQL内部会建立一个事务，在事务内才进行真正插入处理操作。通过使用事务可以减少创建事务
的消耗，所有插入都在执行后才进行提交操作。
```
  3. 有序插入:由于数据库插入时，需要维护索引数据，无序的记录会增大维护索引的成本。我们可以参照InnoDB使用的B+tree索引，如果每次插入记录都在索引的最后
  面，索引的定位效率很高，并且对索引调整较小；如果插入的记录在索引中间，需要B+tree进行分裂合并等处理，会消耗比较多计算资源，并且插入记录的索引定位效
  率会下降，数据量较大时会有频繁的磁盘操作。
  4. 总结:合并数据+事务的方法在较小数据量时，性能提高是很明显的，数据量较大时（1千万以上），性能会急剧下降，这是由于此时数据量超过了innodb_buffer的
  容量，每次定位索引涉及较多的磁盘读写操作，性能下降较快。而使用合并数据+事务+有序数据的方式在数据量达到千万级以上表现依旧是良好，在数据量较大时，有
  序据索引定位较为方便，不需要频繁对磁盘进行读写操作，所以可以维持较高的性能。
  
  
* 如果在某个环境上单独搞一套微服务,不让这个微服务和环境的部署有关,也就是环境杀进程的时候杀不掉我这个微服务需要改如下配置:在你微服务的bin目录下有个
startup.bat文件,打开后在setlocal那一行后面加如下配置:
```
set "JAVA_HOME=%cd%\..\jre"
set "JRE_HOME=%cd%\..\jre"
echo [%JAVA_HOME%]
echo [%JRE_HOME%]

然后再把bin目录下的setcalsspath.bat中的java.exe全部改成你要的进程名字,比如javalite.exe这样环境就没法杀了这个进程.
```

* 使用==比较时,java的integer变量默认比较的是地址值.integer内部维护了一个-128~127的常量池,所以这个范围的integer都是一样的,如果超过了这个范围,那就是一个新的对象,地址肯定不一样. 但是如果比较的某一边有表达式,那就比较的是具体数值. integer本质上是对象,所以用了表达式之后相当于是对对象进行了操作,这样的话用==比较实际上是用equel比较,比较的自然是具体值.  对于long类型来说,使用equel比较的都是数值,long的equel会自动判断这个数值是不是long.























































