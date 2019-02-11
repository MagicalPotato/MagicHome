* 微服务是把一个整体应用按照业务拆分成很多功能单一的小应用,每个应用都是一个独立的进程,能够单独启动或销毁,有自己独立的数据库,且各个微服务的数据库不一定
相同,单个微服务可以根据配置随时切换公库和私库,彻底解耦.还有一点就是前后端分离.用任务管理器打开之后能看到多个独立的进程; 当然缺点也是有:分布式的调用
和通讯处理起来不简单,运维人员不好过那么多war包还要写脚本部署,再就是数据一致性,性能监控等.
* dubbo和springcloud的本质区别是:dubbo的通信机制是基于RPC的远程过程调用,而cloud是基于HTTP的restful API,各个微服务之间都是通过轻量级http调用.
* 微服务架构的技术栈:
```
服务开发: springboot, spring,springmvc
服务注册与发现: springcloud的eureka, 别家的zookeeper
服务调用: cloud的rest, 别家的RPC
服务熔断: cloud的hystrix,envoy
负载均衡: cloud的ribbon,别家的Nginx
配置中心: cloud的配置中心
消息队列: kafka, RabbitMQ,activemq
服务部署: docker              
...等等等等....每种技术理念实际上都有很多各个公司的实现,cloud最终一统江山,给出了一个整体解决方案.它就是为分布式而生
```
