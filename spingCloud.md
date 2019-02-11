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
* 一个典型的工程yml配置:
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
     name: microservicecloud-dept    //这个名称就会你这个微服务暴露给外界使用的名称
   datasource:        //数据库相关的一大堆
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
  client: #客户端注册进eureka服务列表内
    service-url: 
      #defaultZone: http://localhost:7001/eureka
       defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/      
  instance:
    instance-id: microservicecloud-dept8001
    prefer-ip-address: true     #访问路径可以显示IP地址     
 
info: 
  app.name: atguigu-microservicecloud
  company.name: www.atguigu.com
  build.artifactId: $project.artifactId$
  build.version: $project.version$
```

