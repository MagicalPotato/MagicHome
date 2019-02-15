* 微服务是把一个整体应用按照业务拆分成很多功能单一的小应用,每个应用都是一个独立的进程,能够单独启动或销毁,有自己独立的数据库,且各个微服务的数据库不一定
相同,单个微服务可以根据配置随时切换公库和私库,彻底解耦.还有一点就是前后端分离.用任务管理器打开之后能看到多个独立的进程; 当然缺点也是有:分布式的调用
和通讯处理起来不简单,运维人员不好过那么多war包还要写脚本部署,再就是数据一致性,性能监控等.
* dubbo和springcloud的本质区别是:dubbo的通信机制是基于RPC的远程过程调用(牺牲性能),cloud是基于HTTP的restful API,各微服务之间是通过轻量级http调用.
* 微服务架构的技术栈:
```
服务开发: springboot, spring,springmvc  //boot是微观,它构建服务,cloud是宏观,它管理服务.boot可以离开cloud单独用,但是反过来不行
服务注册与发现: springcloud的eureka, 别家的zookeeper
服务调用: cloud的rest, 别家的RPC
服务熔断: cloud的hystrix,envoy
负载均衡: cloud的ribbon,别家的Nginx   //Nginx是负载均衡器, netflix是cloud的熔断器.
配置中心: cloud的配置中心
消息队列: kafka, RabbitMQ,activemq 
服务部署: docker              
...等等等....每种技术理念实际上都有很多各个公司的实现,dubbo就是阿里出的一个分布式框架,但是后来停止维护5年,2017年才重启.这五年里springcloud不断
崛起,最终一统江山出了一整套解决方案,它就是为分布式而生.dubbo只有一些重要的功能才有实现,其他都得用第三方,自然很难和各方面都有解决方案的cloud抗衡了.
```
* 分布式的正确打开方式是分包分部署.也就是你创建一个主工程之后把这个工程当成父工程,然后再在这个父工程下创建各个子模块. 创建maven父工程的时候打包
方式packing选择pom,后续把各个子模块的公共jar包提出来放到pom中,这个工程就相当于一个抽象父父类.在它下面继续创建别的子工程.
```
fatherProject    //这就是父工程,创建工程的时候我们就不是使用new maven project了,而是new-other-maven module了,然后一步步往下,
    sonProject1  //packing也就不是pom了而是jar,因为后续的子模块都是要用来真正使用的,而父抽象工程实际上起到的是提出子模块公共内容的作者
    sonProject2  //当子工程创建好之后就会在挂在父工程的目录下
    src        
    pom.xml   //父目录的pom文件中引入了所有子类都会用到的一些依赖和其版本.当子模块创建好之后,父pom中会有一个<module>标签,里面放的就是子类的名称
    
sonProject2   //子工程在父工程下有一个快捷方式,然后和父工程平级则是自己的工程真正的目录,一个maven工程的经典目录结构
    src/main/java
        各个要用的包  //lombok是一个可以通过注解来自动生成对象类的get,set,构造方法,toString,equals,hash等一堆方法的一个jar包,在子工程
            xxx.java   //引入之后创建实体类时引入直接用.这样避免了后续增删属性还要对应增删get,set等方法.当然如果需要特定构造方法可单独加.
    src/main/recources  //resources下放一些配置,静态资源之类的文件.  有时候在配置文件中写的classpath就是指这个文件夹下
    src/test/java
    src/test/recources   // 注意一个开发技巧,实体类编写的时候最好实现序列化接口.
    API Library
    src
    target
    pom.xml  //子类的pom中用<parent>标签引入父pom的依赖,如果子类有单独需要的依赖再单独引入即可.
    
sonProjext3  //新建好一个子工程之后,先配置pom,首先是引入父工程,然后如果用到了模块2中那个实体类,这时候你就直接在依赖中把模块2引入,groupid是父id,
           // artifactid是子模块id,版本用表达式.这样当你将模块2 clean install之后,就可以在这个模块直接用其中的实体类,也不用管模块2的版本之类的
           //配置好pom之后就是配置各个子模块各自的工程配置,也就是properties.yml,按照yml中的条目配好文件夹都建好配置文件也建好这个整合就完成了.
```
* 这个是生产者的yml配置,生产者要完成各种功能,所以要整合数据库信息,boot信息,eureka客户端信息等:
```
server:
  port: 8001   //你这个服务暴露的端口
  
mybatis:     //这快配置就是boot和mybatis的整合,把这些配上就行了.事实上当和boot整合之后mybatis自己就没有配置了,都在这个yml的配置中.有需要就建.
  config-location: classpath:mybatis/mybatis.cfg.xml        # mybatis配置所在路径,意思就是你要在你工程resources下创建对应的文件夹和文件
  type-aliases-package: com.atguigu.springcloud.entities    # 所有Entity别名类所在包,就是你那些实体类所在的包,到时候mybatis就会去扫该包
  mapper-locations:
  - classpath:mybatis/mapper/**/*.xml                       # mapper映射文件, 就是你的sql的xml放的目录
    
spring:
   application:
     name: microservicecloud-dept  //注册到Eureka之后,管理界面上Application一栏你的微服务显示的名字,也就是该服务暴露给外界的名字
   datasource:        //数据库相关的一大堆                                             //Eureka会自动将其改成全大写.
     type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
     driver-class-name: org.gjt.mm.mysql.Driver              # mysql驱动包
     url: jdbc:mysql://localhost:3306/cloudDB01              # 数据库名称
     username: root
     password: 123456
     dbcp2:
       min-idle: 5                                           # 数据库连接池的最小维持连接数
       initial-size: 5                                       # 初始化连接数
       max-total: 5                                          # 最大连接数
       max-wait-millis: 200                                  # 等待连接获取的最大超时时间
      
eureka:
  client: #客户端注册进eureka服务列表内 //这快就是要把一个微服务注册到eureka的时候在自己工程的yml中增加的配置
    service-url: 
      #defaultZone: http://localhost:7001/eureka  //注意这是单机时候的配置,下面是添加了集群时候的配置.也就是这个服务要同时注册到三个中心中
       defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/      
  instance:  //下面这个id是你给你这个微服务在Eureka中指定的名字,本来是自动分配,但是为了简单你可以自己指定,页面上这个名字是可以点进去的,若不处理,
    instance-id: microservicecloud-dept8001  //点进去是个error页面.在工程中引入boot的actuator启动器,然后在父pom中添加构建信息,最后在你当前 
    prefer-ip-address: true     #工程的pom中添加页面跳转的info信息即可.
    //上面这个是你改了instance-id之后鼠标放上去浏览器左下角会显示当前微服务的ip和端口,false就不显示
info: 
  app.name: atguigu-microservicecloud
  company.name: www.atguigu.com
  build.artifactId: $project.artifactId$
  build.version: $project.version$
  
一个工程的流程:maven的pom,工程的yml>创建数据库,建表>dao层和mapper.xml>service>Controller>boot主启动类.
dao层类上要加@Mapper,本来dao要有实现层,但是整合了mybatis之后直接使用mapper.xml就行了,不用实现了;主启动类别忘了@springbootApplication

spring时代bean组件还是通过配置来指定的,到了boot时代,配置类代替了配置文件,只要在类上加了@configuration,那就说明这个类是个配置类.然后在配置类中
再创建@bean注解的方法,在方法里直接返回组件对象,就是我们的bean实例.以往我们的一个工程都是Controller调service,service再调dao,现在还有中方式,我们
在配置类中写个这样的方法:
@bean                        
public RestTemplate getRestTemplate(){  //注意这个东西就是学boot的时候遇见的那个模板接口,springboot提供了很多模板接口,rest模板接口主要是用来调
    return new RestTemplate();   //rest方法的. 你直接在Controller中自动依赖这个模板类,然后用模板类的各个方法去调用生产者的方法查询东西.
}            //为啥我不写service呢,是因为我这个模块是个消费者,消费者没有业务逻辑,所以不需要逻辑,要得到东西就用生产者现成的,所以模板实际上是把
   //生产者的方法给调用了一遍, 避免了重复类和代码,你的消费者模块中就少写很多东西.

@Autowired
private RestTemplate restTemplate
@RequestMapping(value='/consumer/car/get{id}')  //看到没,暴露给外面的调用url是加了consumer的
public Car get(@PathVariable long id){        //但是呢实际上完成任务的url还是生产者模块中的,方法也调的是生产者中的方法.
    return restTemplate.forObjext('http://localhost:8001'+'/car/get'+id,Car.class)
}
```
* Eureka server用于提供注册,Eureka client用于查看各个微服务的状况.在刚刚上面那个例子中我们可看到,消费者调用生产者是直接调用了生产者的接口,相当于
是多了个中间调用过程,而有了Eureka之后我们就可将生产者注册到Eureka上,然后消费者从Eureka中来消费生产者; Eureka有个自我保护机制,默认情况下,注意是默认
,当一个微服务超过一定时间没和注册中心联系,Eureka就会注销这个微服务;但是注意,此时这个服务可能并不是不服务了,或许是因为网络拥堵消息没到,所以我们本意
上并不想将其注销.所以Eureka有了这个自我保护机制.当Eureka在短时间内丢失过多客户端时,它就会自动开启自我保护机制,在这个机制中,Eureka不会注销任何实例,
或许在这个机制过程中真的有客户端是不用了人已经干掉了,但Eureka还是会保留其信息,直到退出保护机制才进行处理.自我保护可以通过配置关闭,但不建议.还有再
提一点,yml的配置冒号后面必须要有一个空格.
* 创建一个注册中心的过程:
  - 像创建模块2那样新建一个Eureka的注册中心模块
  - 接下来还是先搞依赖,在pom中引入Eureka server启动器
  - 然后写yml配置,端口(每个微服务都要配),注册中心实例名称,是否注册自己啊之类的当前工程会用到的配置
  - 建主包,在主包下建主启动类,主启动类加@springbootApplication,注意此时你的这个服务是个Eureka server,也引入了相关依赖,自然你这个主启动类就要
  开启相关的功能,所以还要加@EnableEurekaServer,表示服务启动了注册中心,能接受别的微服务过来注册.
* 要将一个微服务注册进eureka注册中心的过程:
  - 修改该微服务的pom,新增两个依赖,一个是eureka(带server的是服务端,不带的都是客户端,也就是都是可注册的服务)启动器,另一个是config启动器
  - 改yml配置,将当前微服务注册到eureka,参考上面那个完整的yml配置
  - 在当前微服务的主启动类上加上@EnableEurekaClient,表示这是一个Eureka的客户端,一个可被注册到服务端的微服务
* 注册进Eureka的微服务可以添加发现功能.比如你当前这个生产者服务,你注册进了Eureka,然后你想知道Eureka中还有哪些服务,它们的信息是什么,这时候可以通过
在当前这个服务的Controller中引入DiscoveryClient来实现,同时主启动类添加@EnableDiscoveryClient. 不过服务发现不是重点
```
@Autowired
private DiscoveryClient client
@RequestMapping(value='/Car/discovery/')
public Object discovery(){
    client.getxxx   //循环获取或者查找单个已经注册的client
    //也就是你的这个生产者中自己提供了一个对外的接口,可以直接通过自身这个接口来查询注册中心有哪些服务. 当然,在自身的服务中查这个没什么意义,你要让
    //消费者能调用到你的生产者,获得信息,那即是一个道理,在消费者中新建方法,把生产者的这个方法调用一遍即可.
}
```
* 高可用:那Eureka来说,你配一个注册中心当然没问题,但是假如有一天你的这个中心突然被闪电劈死了你的整个系统咋办?所以说就有了集群的概念,有了集群你的系统
可用性当然就高了.当然,集群对机器是有要求的,因为你的一个模块启动通过任务管理器看到大概就要占用三四百兆的内存,一个集群少说也得配个三五个注册中心吧,所以
要使用分布式大集群,你的物理设置不能太简陋啊.配置集群也是一样:
  - 建新Eureka模块,把老的模块中的pom中的依赖拷过来
  - 改新Eureka模块的主启动类的类名,老的叫Eureka1,那拷过来得叫Eureka2
  - 改各个eureka的yml配置
```
server:    //这个是eureka注册中心模块的配置,三个模块当然得有三份配置,把一些小细节修改一下即可.还要注意,加了集群之后你的其他模块当然也要把集群
  port: 7001    //端口要改                //地址配上,看生产者中的那个配置.
eureka: 
  instance:   //还有一个点很重要,加了集群之后要做端口映射,单机的时候127.0.0.1对应的是localhost,你直接用localhost:端口即可访问服务,但是有了
    hostname: eureka7001.com #eureka实例名称.   //集群之后你用localhost就不行了,因为计算机不知道localhost到底要对应哪个,所以要改host文件
  client:               //让127.0.0.1 分别对应三个注册中心地址eureka7001(2)(3).com ...,这样就可以用地址:端口来访问对应的服务了
    register-with-eureka: false     #false表示不向注册中心注册自己。
    fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url: 
      # defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/  # 单机配置,只有一个eureka时候用这个,集群就不用了
      defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/   #这个是集群配置,每个中心要把另外两个配上
```
* ACID:原子性,一致性,独立性,持久性   CAP:强一致性,高可用性,分区容错性
* 任何一个分布式系统不能同时满足CAP,传统的关系型数据库大多是CA,而新兴的nosql数据库大多是CP,要满足分布式,P分区容错性必须要保证,剩下就是在一致性
和可用性中做选择,作为一个架构师,如果是对双十一,京东618这样的东西,那么并发量大保证可用才是关键,所以选AP,而对于数据量不打但是数据更重要的系统同步才更
重要,所以选CP. zookeeper有个leader,当leader挂了会后会选举出一个新leader,但是选择通常会耗费30秒到2分钟,所以它不适合高可用,而只适合强一致.所以
zookeeper是CP;Eureka的节点不分主次,当一个挂了其他照样可以服务,除非服务都死绝了,服务挂了之后数据同步不及时,拿到的数据不一定就是最新的,所以Eureka
不保强一致性,而是在并发非常大的时候的首选.这是他们之间一个重要区别.Eureka也是借鉴了zookeeper的缺点所以选择了AP.

* springCloud Ribbon是基于Netflix Ribbon实现的一套客户端负载均衡工具. 注意这里的客户端,这可不是我们平常听的手机软件客户端,而是你的模块,比如刚刚
我们做的Eureka server模块是服务端,那么你的那个消费者就是一个客户端,也就是colud的这个ribbon是给类似消费者那样的模块用的.不过好像两个客户端理解起来
也差不了多少.  负载均衡(load balance),所以简称LB,常见的Nginx就是一个负载均衡服务器. 
  - 集中式: 直接在客户端和服务端之间加硬件设施,比如F5,请求来了看那个服务器轻松就发哪个,效果好,但是贵,小公司肯定用不起
  - 进程内: 偏软件,就是在消费者端实现均衡,比如排队,消费者查看一下注册中心看看哪个人少我就排哪个队,而不是在那里死等
* 一定注意,ribbon是客户端的负载均衡,所以你要配置当然是在你的消费者模块中配置它,改消费者模块中的pom添加相关依赖,然后是yml:
```
server:       //这是消费者的yml配置
  port: 80
eureka:
  client:
    register-with-eureka: false   //不往注册中心注册自己
    service-url: 
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/  
之前我们在消费者的Controller中调用生产者的接口的时候是通过RestTemplate调用的,当时这个模板是从配置类中直接返回的,我们在放回这个模板对象
的方法上加了@bean,现在我们要让客户端有负载均衡的能力,那还是在那个方法上加上@LoadBalance即可,当然不能忘主启动类@EnableEurekaClient.
return restTemplate.forObjext('http://localhost:8001'+'/car/get'+id,Car.class)  //之前我们在消费者中是通过地址调生产者
现在改成 return restTemplate.forObjext('http://直接用Eureka中注册的服务名称'+'/car/get'+id,Car.class),只有改了名称之后才是真正的通过
Eureka来调用的生产者,也就是说当Eureka和ribbon整合之后消费者就可以直接通过Eureka中的服务名称来调用各个服务而不是在代码中写死地址来调用了.

改好了消费者你的生产者肯定要增加,那就拷贝呗,把已经弄好的那个生产者拷贝两份,然后注意,此时除了端口,朱启动类名称等那些固定要改的东西以外,还有一个重要
的,就是数据库,既然是微服务,那么一个服务一个库,那么生产者2和3肯定需要不同的数据库,此时你就建库,建表,然后在2和3的配置中把数据库信息配上.同时一定注意
对外暴露的那个名称三个都是用同一个microservicecloud-dept,不能写成三个,也就是三个生产者是同等级的,不然你改了之后三个生产者就是三个独立的服务提供者,
根本谈不上负载均衡还是轮询,三个名称一样才有负载均衡这一说.
还有一点即使说为什么会有消费者和生产者这中分类,因为最终广大用户面对的肯定是你的消费模块,也就是客户端,但是你的客户端肯定不是用于处理后台逻辑的,后台
逻辑应该是私有的不能让人看到,所以消费者要调用生产者,消费者public,生产者private或者public,但是总之,生产者不能直接暴露给用户.
```
* ribbon的默认算法有七种,默认中用的默认是轮询(挨个使用),如果想要换算法,就直接在配置类中新建一个@bean方法,返回类型是IRule接口类型,实际返回对应
算法的一个对象即可.ribbon的机制是如果你没有特别指定返回什么对象那么就用轮询,指定了就用你指定的那个.改了配置类之后所有服务要重新启动.实际中如果
服务多一个个手动启也是麻烦,所以后续又有了自动化脚本这玩意. 还有一点要注意,你的ribbon是给消费者用的,那么切换算法自然也是在消费者模块中,所以你的
配置类自然也是建在消费者模块中.就如同之前那个模板配置一样,都是在消费者模块的配置类中.
* 在消费者主启动类上添加@RibbonClient(name='你要特殊定制的那个服务对外暴露的名称',configuration='myRule.class'),注意,你自己写的这个自定义类
不能放在加了@ComponentScan注解所在的类所在的包或者子包下,而我们的主启动类@SpringbootApplication这个注解点进去之后就有@ComponentScan,所以说
你自己写的这个自定义类是不能放在你的模块的主启动类那个包下的,你得重新建一个包.既然是配置类,那么类上就要加@Configuration,这个注解就是告诉boot,
启动的时候要默认加载我的这个配置类,当然返回实现对象的方法少不了@bean.
* 在muRule.class同级创建你自己的算法类,继承AbstractLoadBalanceRule,然后把源码算法拷过来改巴该巴,在配置类的返回对象方法中直接返回你自己写的这个,
新轮子,搞定.

* Feign集成了Ribbon,Ribbon是使用Template模板的方式来调用生产者,而Feign是把模板又转换成了接口+注解的方式.在公共接口模块中新增service接口,加上
相应的注解(类似于dao层+注解完成sql查询的方式),然后在新的Feign消费者类中直接引入公共接口包中的service接口,用接口调生产者即可.

* hystrix是断路器,就是当你的某个服务出故障了,不会导致整个系统宕机,而是向调用方返回一个备选响应.避免级联故障,保证系统弹性.它是用在生产者上的,不是
消费者.新建模块之后把老的生产者pom该拷的拷过来然后加上hystrix启动器,之后老的yml整个拷过来,只需要改一个名字,不是这个服务暴露给外部的那个名称,是能
点进去显示info那个,应为你是加了熔断的服务,名称自然改个带hystrix的名字,以和老的生产者区别开来. 改好之后就是用,我们之前无论是用模板调生产者还是用接口
注解的方法调生产者,是不是都没对方法加任何保护措施,而熔断就是加保护而已,比如在一个查询接口上写上@HystrixCommand(fullbackMethod='xxx'),当这个查询
接口里面出了问题之后,就会调用你同类(就是你的Controller类,当然也或许是你新建的别的类,后续再研究)里面的xxx方法,你在xxx方法中做了一些熔断处理.比如你的
get接口返回一个Car对象,而我的熔断xxx方法中也返回一个对象,只不过我这个对象中设置了一些失败信息,这样调用过程不会中断,信息也能返回;这么来看,熔断器其实
就是个try catch,一旦方法出了问题,就直接在catch中返回同类型的东西就行了,只不过try catch性能差点罢了.当然,主启动类的@EnableCircuitBreaker不能忘.
* 服务降级是说系统整体资源快不够了,先忍痛将其中一些服务先关掉,等系统度过难关之后再重启.服务降级只用在客户端的.也就是说当你的一个生产者里面方法出了
问题或者你**关了它,外部访问的时候不会报错,而是看上去还是能访问,只不过得到的结果是一些提示信息,类似暂停营业**,再次注意降级是客户端的,和服务端没
关系,服务端随时可以停掉忙别的事,客户端会处理.具体的处理就在把原本客户端中的兜底方法全部提到公共模块当中去.
* 这种把熔断方法和功能方法放在同一个类(生产者中的查询service)中的做法不好,假如功能方法有一百个,那么你的的熔断处理方法自然也要有一百个,那这个类不就
太杂太大,和解耦的思想不符合.所以我们要把专门处理熔断的方法写在公共的模块当中.我们之前不是在公共的模块当中写了查询生产者的接口类么,现在我们在那个接口
类的同级新建一个熔断类,这个类要实现同时实现FallbackFactory和原本那个查询接口,这样写: xxxx implements FallbackFactory<原本那个查询接口>,注意这里
的尖括号不是泛型的意思,而是xxx这个类同时实现了两个接口的意思,之前一直不明白还以为是泛型.继承只能单继承,而实现可以多实现,多实现就这样写.实现了这两个
接口后当然要重写两个接口的方法,fallbackfactory只要一个create方法,直接重写它,然后在这个create中用内部类的方式新建一个查询接口类对象然后在对象中重写
功能方法.此时你的这些重写方法就是熔断信息,当你的功能方法出了问题或者因为系统降级暂时不让外部访问,那么你就在对应的方法中写对应的信息就行了.这里有一点
要千万注意,你这个xxx类上**不能加@Compnent,加了这个类不能用**.完成这些之后,在原有那个功能接口类上修改原有的注解,把原来的@FeignClient(value='xxx'),
改成@FeignClient(value='负载均衡的客户端',fallbackFactory='刚建的那个xxx.class'),意思就是说当你的这个接口中这一大堆功能方法出了问题你们都去找
xxx这个接口,然后在消费者类的yml中增加Feign:Hystrix:enable: true,maven clean install,搞定.实际上这个就是一个典型的面向切面思想,原本我们会给每个
功能方法头上添加一个熔断注解,然后在同类中写这个熔断方法,那么每个方法头上这个熔断注解不就相当于一个切面.我把这一堆切面统一提出来,弄成一个大面整合到
一起,在公共的接口中实现他们的真正逻辑,这就完成了切面编程.想必那些日志之类的东西也是这样实现的.
* 创建监控dashboard微服务,pom引入启动器,yml配端口,工程主启动类上@EnableHystrixDashboard开启熔断监控的管理页面, 然后各个生产者的pom中添加
spring-boot-starter-actuatur,这个之前加过了,确保都好着,然后启动该服务,访问响应的地址就可监控各个生产者的信息了.

* zuul是gateway网关,注册进Eureka之后获取所有服务信息,之后所有请求由它统一代理+路由+过滤.还是老套路,建工程,pom,yml,@EnableZuulProxy; 访问的时候
就不再是直接访问生产者的地址了,而是:  ttp://gateway-9527.com/ + 对应的Eureka中的服务比如是abc+ 调用地址xxx    
```
server: 
  port: 9527
spring: 
  application:
    name: microservicecloud-zuul-gateway  //注册进Eureka中的名称
eureka: 
  client: 
    service-url: 
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka,http://eureka7003.com:7003/eureka  
  instance:
    instance-id: gateway-9527.com   //看到这个就记住,要改host文件做映射了,映射这个地址
    prefer-ip-address: true  
zuul:   
  #ignored-services: microservicecloud-dept   //不让这个服务对外暴露,只能通过域名映射来访问它,但是只配置一个肯定不行
  prefix: /xyz   //这个是访问地址前加的统一前缀.也就是ttp://gateway-9527.com/xyz+其他那些乱七八糟的 
  ignored-services: "*"  //这个是隐藏,就是说你的哪个服务要对外屏蔽,不能直接通过该服务访问,*代表所有访问都要经过网关
  routes:    
    mydept.serviceId: microservicecloud-dept   //这个是域名映射,就是不让你看到原本服务的真实名称abc
    mydept.path: /mydept/**    //访问的时候只能用这个路径来访问
info:
  app.name: atguigu-microcloud
  company.name: www.atguigu.com
  build.artifactId: $project.artifactId$
  build.version: $project.version$
```
* 配置中心config server本身就是一个微服务,它来管理其他所有服务的配置信息,它能和外部配置进行绑定.比如把它和github的某个仓库配置进行绑定,当一个人改了
这个仓库的配置bootstrap文件之后,config server能自动从github上面去拉取新的配置信息引用到各个服务上.它有服务端和客户端(就是页面);但是实际上这个东西
是个鸡肋,原本配置中心的目的是为了去掉每个工程的配置文件,结果呢,你搞出来了这么个东西,原本的配置文件是去掉了,但是同时你又得在模块下重新建两个另外的配置
文件,这不脑子有病吗.东西压根没减少,只不过配的东西转移到了一个别的地方而已.与其花那么多精力去整合这个配置,不如就把配置放在各个模块下,配置这种东西一旦
弄好了又不会经常改动,费那种额外的劲作甚. 就当是一种思想大概了解一下吧. 具体过程:
```
1. 在GitHub上新建一个配置仓库,获取到仓库的地址
2. 在本地建一个文件夹把abc,进入到abc调出Git bash,通过clone命令将刚新建的仓库克隆下来.注意此时新仓库的文件夹是在abc下.
   git status/git add ./git commit -m ''/git push origin master
3. 在本地刚clone的仓库文件夹中创建你的配置文件.一定注意!!!!保存文件的格式是UTF_8!!!!!!,用记事本打开另存为,选择UTF_8,覆盖原有然后编辑
4. 老套路,建模块,引依赖,改配置,@EnableConfigServer,改host(非必须,服务多了端口肯定记不住,改成地址好记)TransportConfigCallback,那是git插件问题,
需要引入一个依赖.具体看工程中的依赖.
server:    //server端的配置
  port: 3344  
spring:
  application:
    name:  microservicecloud-config
  cloud:
    config:
      server:
        git:   #下面的GitHub上面的git仓库名字,就是你clone的地址,当服务启动之后服务会自动去该地址获取相应的配置文件.git的配置可以通过三个杠---
          uri: git@github.com:zzyybs/microservicecloud-config.git //来区分环境,服务启动后可以在地址中指定具体要用哪个环境的配置,不指定用默认

客户端的配置大概类似,就是你在Git上传了一份配置,然后在客户端中使用bootstrap来指定用Git上的配置中的哪个部分.
```

##### 微服务第一季总结
* 首先所有微服务都是部署在docker容器中,前后端完全分离,前端调后端,后端相互调
* 前端: 页面H5+thymeleaf  样式CSS+Bootstrap  框架JQuery+nodeJs
* http请求过来之后,负载层: 量小的话Nginx,量大的话就是硬件F5,或者偏软件的LVS+keepalive
* 请求通过LB(load balance)后,到达网关zuul: 内嵌Ribbon做客户端负载均衡,Hystrix做熔断降级
* 服务注册Eureka:已经注册到Eureka的zuul从Eureka中获取其他已发布的微服务信息,并把请求转发到相应的微服务上去
* 后端相互调用: 请求转发过来之后,后端各服务之间采用标准的http+rest+json的方式相互调用,主要技术有Ribbon,Feign,httpclient,hystrix
* 统一配置:服务多了配置就乱,所以集中起来和GitHub整合
* 微服务自己的整合:每个微服务都可以和第三方的框架进行整合,比如缓存服务redis,图片存储服务fashDFS,搜索服务elasticsearch,安全管理shiro
* 最后就是数据库:每个微服务都可以连主库,也可以连自己的库.库多了就是mysql集群,或者分库分表mycat
