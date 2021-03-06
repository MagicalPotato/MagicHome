* hashmap底层初始化的时候会新建16个数组,然后每个数组有一个索引值.当一个新对象进来后,会根据hash方法来计算出一个索引值,但是无论怎么计算,你的这些索引值
肯定都是在那16个之中,所以说就会出现多个对象对应一个索引值的情况.这中情况就叫哈希碰撞.当发生哈希碰撞的时候,对象会被运到对应的数组里,然后和数组中的对象
进行比较,如果发现两个对象是相同的,那么就用新来的替换后来的.如果不一样,就会生成链表,把老的往后挪,新的加进来. 所以当重写了hashcode方法之后要重写equals
方法.但是无论你的hashcode方法和equals写的多么吊,哈希碰撞都是不可避免的.因此hashmap引入了一个加载因子,值是0.75,它的意思是当元素达到hashmap总容量的
75%的时候就进行扩容.当进行扩容的时候,原本在处于链表上的元素就会被重新hash,得到新的位置.这样碰撞的概率就会降低,那性能就会提高.碰撞多导致的问题就是
性能降低.因为假如你要判断某个元素和hashmap中的哪一个相同,经过hash计算得到了当前这个元素的hash索引,然后根据索引找到了了对应的数组链表,然后你开始和
链表上的元素进行一个个比较,如果元素很多且你找的那个恰好就是最后一个,那显而易见效率肯定低. 
* java8对hashmap的底层做了优化.当某个链上的元素大于8个,并且hashmap的总容量大于64的时候,链表会被直接转成红黑树.红黑树是个树节点,当一个元素过来,节点
上的元素会和当前元素的hashcode进行比较,如果当前的hashcode值大,则往向上的节点添加,小就往下添加,到达上一个或下一个节点还是一样的比较大小,大往上小往下,
这样的话除了添加元素慢一点,其他的效率都会有很大提升.在扩容的时候也不用在重新hash,而是直接加上扩大的容量.比如你当前这棵树是在16的位置,现在扩了10,那么
所有的树都整体往后移10个位置.新元素会放在前10个位置.同样的hashset也优化了,concurrentmap也是一样也进行了优化(以前的版本用的是锁分段机制,1.8用了CAS
算法).不过要注意一点,只有满足条件的时候链表才会转成红黑树,如果不满足条件,那就还是链表.所以1.8的hashmap底层是既有链表又有红黑树.
* 1.8对java的内存机制也进行了优化,以前有方法区(实际上就是堆的一部分,主要存类的信息,也会被回收,到是条件严格),现在堆的永久区和方法区都移除了,出现了一个
Metaspace(元空间).元空间直接用的是物理内存,你的物理内存有多大元空间就能利用多大.这样的话outofmemory异常出现的概率也就更低了(空间大了哪还能溢出).
同时与永久区相关的两个参数premgensize(初始永久)和maxpremgensize(最大永久)也就没了.取而代之就是metaspacesize和maxmetaspacesize.默认的是物理内存
多大元空间就能用多大,当然你可以自己配置.
* 多个相似的方法优化演变: 写多个方法每个不同的就一个地方>>策略模式,写一个接口,然后写多个实现类,实现类里实现不同的地方>>在方法里直接使用匿名内部类>>使
用lambda表达式>>使用Stream Api
* lambda表达式本质是对函数式(里面只有一个抽象方法)接口的扩展.所以使用lambda必须有函数式接口的支持,看如下例子:
```
public interface myInter<T, R>  //指定泛型的好处就是你在使用这个接口的时候想传入什么类型的参数就传什么类型的参数
{
    public R compare(T i,T i2);    //这个是函数式接口中的一个抽象方法.接口中的两个泛型一个是参数一个是返回值.同样
}                                //类型的参数当然你可以指定多个.

在另一个类里有个方法要进行大小比较用到了这个接中的getValue抽象方法
public integer getValue(integer i,integer k, myInter<integer,integer> mi)//第一个integer是参数,第二个是返回值.泛型参数指定一次就行, 
{                           //但可以传入多个.mi是接口的实现.实际上这就是策略模式,直接用一个类来实现接口的抽象方法,然后在使用
      return mi.compare(i,k);  //的时候把已经实现的类对象传递进去,让实现类对象中的方法起作用.lambda的方式
} //是将这个实现留空了,真正的实现在具体使用时才指定,想实现成什么样就实现成什么样.这就就是方法可以当成参数传递的精髓.本质是接口和实现类

integer a = compare(100, (x) -> x*x)  //然后在使用的时候就可以直接使用lambda来对该方法进行实现

在实际的开发中我们并不需要自己先去写接口然后在在用lambda,java8已经内置了四大接口供我们使用:
Consumer<T>    //消费型接口
    void accept(T t)  //传进去一个T,啥也不返回
Supplier<T>   //提供型接口
     T get()     //啥也不传,返回一个T
Function(T, R)   //函数式接口
     R apply(T t)  //传进去一个T,返回一个R
Predicate<T>   //断言型接口
     boolean test(T t)  //传进去一个T,返回一个判断结果.
 
lambda表达式本质就是对函数式接口的实现,所以如果我直接用赋值的方式来使用lambda也是可以的:
 Supplier<String> str = ???  //提供者的接口特性是什么,没有参数,但是有返回值.那么我们就来写一个lambda,让它返回一些东西
 A a = newA()  //创建一个A对象
 Supplier<String> sup = () -> a.getName();  //等号右边是lambda对接口的实现,a.getName会生成一个str让接口来包装,sup是包装后的接口实体
 String str = sup.get();   //实现接口的实体对象调用自己的get方法,返回自己内部包装了的值.
 
 如果a.getName()方法的参数列表和返回值类型与函数式接口的方法列表和返回值类型相同,则可以换一种更简洁的写法:
 Supplier<String> sup2 = a::getName;  //就是这么简洁.但是记住使用条件.感觉种反而不容易理解了.有点简化过度.
 String str = sup2.get();    //这种简化成为方法引用
 
如果lambda表达式有返回值,且有多条语句,无论前面的语句怎么执行,最终的执行结果都需要return.不同语句之间用分号隔开.
```
* Stream是一个中间操作链,类似于linux的管道,从数据源获取数据放入流,然后对流中的数据进行一些操作,最后生成新流.
```
1. Stream<String> stm = list.Stream();  //通过Collection系列集合(list,set,map)的Stream(串行流)或者parelleStream(并行流)生成流.
   stm.map((x) -> x.toUpperCase()).forEach(System.out::println)  //中间操作map可以将lambda方法应用到list中每一个元素上
2. Employee[] emps = new Employee[10];   //map相当于未做任何操作的原始状态,比如filter是一个专业过滤,map就是没有施加任何干扰的原始状态
   Stream<String> arr = Arrays.Stream(emps); //通过Arrays中的静态方法Stream()获取数组流.  注意流都是有泛型的
3. Stream<String> ss = Stream.of("aaa","bbb","ccc") //使用Stream类中的静态方法of()来生成流,of方法中是可变参数,传入数组,list,map都行
4. Stream<Integer> i = Stream.iterate(0, (x) -> x+2)  //第一个参数是标志位,第二个是lambda表达式.可以理解为这个里面包装着无限的偶数
   i.filter((x) -> x+2).limit(10).forEach(System.out::println)  //从流中取出包装的数据. 这是无限流的第一种创建方式. 无限流会一直执行下去.
   其中filter,limit都是中间操作,而forEach是终止操作.在终止操作执行前,无论有多少中间操作都不会执行,当终止操作执行时,中间操作才一次性执行.
   除了forEach,还有如下终止操作:allMatch,anyMatch,noneMatch,findFirst,findAny,count,max,min,详情使用的时候再具体查看.
   Stream.generate((x) -> Math.random()).forEach(System.out::println)  //生成无限流的第二种方式. 这个方法生成无限随机数
5. Optional<Double> op = carArray.stream().map(Car::getPrice).reduce(Double::sum)  //reduce相当于一种迭代累加.map中得到每一辆小车的价格,
然后把第一辆和第二辆价格相加的到一个值,再用这个值和第三个相加,最终得到所有小车总价. 但是这里有个问题,因为我们没有在map中指定标志位,所以最后的结果
并不一定就有值.可能会得到null.所以java提供了Optional来尽量避免null值.用Optional把结果再包装一次.最后用op.get()来取出值.
   Optional<Integer> op = carArray.stream().map((x) -> 1).reduce(Integer::sum) //map中的表达式要应用到每一个元素上,我应用了,但是每次都直接
   返回1,然后用reduce来对map产生的结果进行计数. reduce是归纳,就是连续加,那么最后就能得到Array里面元素的数量了.
6. 把中间操作看成linux的管道操作,每流入一个管道就完成一件事情,linux的管道之间的命令有不同的写法,那么对应在java中也就有各种殊途同归的写法,无论怎么
   写,最终只要能完成任务就行,比如:Car.stream().map((x) -> x.getPrice()).max(Integer::compare)拿到流之后我想对每个元素干点事,而map正好就是干这个
   事的,所以用它. 然后又想从应用完规则之后的东西里挑出最大的那个,所以用了max这个模式,这个模式具体是怎么实现的呢?就是Integer里的compare.
7. parallel() 方法可以使用并行流,底层还是使用fork/jion框架实现的,但是更方便了.
```
* Optional是一个封装类,可以用于封装对象.类似于list等那种东西,就是可以往里面放同类型的东西
```
Optional<Car> o = Optional.of(new Car) //返回值类型是有泛型的.  of()方法用于得到一个封装对象.但是注意传入的参数不能显式为null
Car car = o.get() //从o中拿到对象
```
* jdk1.5以前创建线程都是继承Thread或者实现Runnable,都要重写run()方法,但是run方法的返回值值void,也就是获取不到线程的执行结果,从1.5以后新增了
Callable接口,使用他来创建线程要重写call方法,call方法是有返回值的,这样就可以得到线程执行的结果.一般情况下它要配合ExecutorService使用.
Future可以获取Runnable或者Callable任务的执行结果.该方法会阻塞直到任务返回结果。callable和future都在java.util.concurrent包下.
```
public interface Future<V> {
    boolean cancel(boolean mayInterruptIfRunning);//用于取消任务.取消成功则返回true.参数表示是否允许取消正在执行却没有执行完毕的任务.
    boolean isCancelled();  /如任务已经完成,则无论参数为true还是false都返回false.  isCancelled()用于检测是否取消成功
    boolean isDone();  //检查是否执行成功
    V get() throws InterruptedException, ExecutionException;  //用来获取执行结果，该方法会阻塞，一直等到任务执行完毕才返回；
    V get(long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, TimeoutException;指定时间内未获取结果则直接返回null.
}
future接口有一个唯一实现类叫FutureTask,它实现了RunnableFuture接口,而RunnableFuture继承了Runnable接口和Future接口，所以它既可以作为Runnable
被线程执行，又可以作为Future得到Callable的返回值。看一个例子:
class Task implements Callable<Integer>{   //写一个Task类实现Callable接口
    @Override
    public Integer call() throws Exception {   //重写call方法
        //具体逻辑...............
        return sum;
}
ExecutorService executor = Executors.newCachedThreadPool();  //创建线程池,线程池可以指定数量
Task task = new Task();  //创建线程对象task
## Future<Integer> result = executor.submit(task);   //用Future获取结果:把task直接交给线程池去执行并获取结果
// FutureTask<Integer> futureTask = new FutureTask<Integer>(task); //使用FutureTask获取结果:把task交给FutureTask,
// executor.submit(futureTask);   //然后再把futuretask交给线程池
executor.shutdown();  //执行完之后一定要关闭线程池,不然eclipse上就会发现结束按钮一直是红色,就是线程池没关.关了的话执行完按钮就灰了.
```
* 旧的时间api是线程不安全的,1.8以后的是线程安全的.分别提供了日期,时间,日期时间,时区,格式化等api,他们的now()方法获取当前值,同时计算时间日期也有
相应的加减方法,可以直接加减年月日时分秒. 也有专门用于计算机使用的instant毫秒api.
* 注意,forEach这个方法本身就可以传入lambda表达式,不一定非要在流里面才能传入.比如你有一个list想要遍历,直接list.forEach(Sysotem.out::print)就行.
