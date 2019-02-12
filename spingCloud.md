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
      #defaultZone: http://localhost:7001/eureka
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
server:    //这个是eureka注册中心模块的配置,三个模块当然得有三份配置,把一些小细节修改一下即可
  port: 7001    //端口要改
eureka: 
  instance:   //为了让localhost能对应三个Eureka,需要修改host文件,让本地ip对应三个Eureka实例> 127.0.0.1 eureka7001(2)(3).com .....
    hostname: eureka7001.com #eureka实例名称.如果不想和localhost对应,那就不管了,也不用改host了
  client: 
    register-with-eureka: false     #false表示不向注册中心注册自己。
    fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url: 
      # defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/  # 单机配置,只有一个eureka时候用这个
      defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/   #这个是集群配置,每个中心要把另外两个配上
```
