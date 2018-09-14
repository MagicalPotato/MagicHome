### ssm框架
* 基础准备
  - springmvc是spring框架的一个模块，springmvc和spring无需通过中间整合层进行整合.springmvc运行过程大概如下：
    1. 客户端发起请求到 前端控制器(DispatcherServlet).
    2. 前端控制器请求 处理器映射器HandlerMappering 查找相应的Handler;之后处理器适配器HandlerAdapter按照特定的规则去执行查到的Handler.
    3. DispatcherServlet将请求提交到Controller；
    4. Controller调用业务逻辑处理后，返回ModelAndView；
    5. DispatcherServlet查询一个或多个ViewResoler视图解析器，找到ModelAndView指定的视图；
    6. 视图负责将结果显示到客户端。
* 重要文件
  - web.xml:当服务启动时首先会去加载web.xml这个资源文件，里面包括了对前端控制器、乱码问题等配置。
  - applicatonContext.xml(也就是spring.xml)一般配置数据源，事物，注解等。本来配置数据源,事务,注解的文件应该叫spring.xml,
  因为是spring整合了这些东西,但是很多项目为了便于管理,将数据源,事务等这些东西分开来配置,每个配置文件单独起一个便于识别的名称,
  最后就演变出了applicatonContext-*.xml这种形式. 将DAO层、Service层、Transaction层分开配置，分别为applicatonContext-dao.xml、
  applicatonContext-service.xml、applicatonContext-transaction.xml. 当然,分开配置时，需要在web.xml中配置这些文件的位置.参考web.xml文件.
  - springmvc.xml: 里面配置的整个项目的控制层的一些东西 ，如视图解析器,静态资源， mvc 文件上传，拦截器等。
  - SqlMapConfig.xml: MyBatis的配置文件，里面无需配置，一切交给spring管理，但是xml文件基础配置要有。有的会把sql.xml配到这里.

* 具体配置文件
  - web.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
   xmlns="http://java.sun.com/xml/ns/javaee"
   xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
   id="WebApp_ID" version="2.5">
  
 <display-name>ssm_boot_crm</display-name> 
 <welcome-file-list>
  <welcome-file>index.html</welcome-file>
  <welcome-file>index.htm</welcome-file>
  <welcome-file>index.jsp</welcome-file>
  <welcome-file>default.html</welcome-file>
  <welcome-file>default.htm</welcome-file>
  <welcome-file>default.jsp</welcome-file>
 </welcome-file-list>
 
 <!-- context我觉得并不应该翻译成上下文,应该翻译为环境,什么环境呢,就是你的项目框架的环境,那contextConfigLocation自然就是环境的
 配置文件的位置.  而classpath即WEB-INF下面的classes目录，在每个J2ee的web项目中都会用到，所有src目录下面的java、xml、properties
 等文件编译后都会放在这里，所以在开发时常将相应的xml配置文件放于src或其子目录下；这里要注意一点,是编译后的那些配置文件都会放到
 这个classpath路径下,也就是说只要你的配置文件是放在工程的src目录根目录或者src/source或者src/xxx...只要是隶属于src的文件夹,最终
 编译过后都会在classpath下创建对应的文件夹并把编译后的配置文件放进去.实际上配置文件编译后和编译前其实没啥差别.若干你的配置文件
 都在classpath下,换句话说也就是配置文件在未编译之前都是放在src根目录下,那么classpath:applicationContext-*.xml这种引用方式就是可
 以的,但是如果你单独建了文件夹放配置文件,那么就要清楚的引用比如: classpath:context/conf/controller.xml .如果你是直接配置了一个
 总的spring.xml,那就是classpath:spring.xml  如果按照这个思路来看的话,那么这个context-param的配置也可以称之为加载容器配置文件-->
 <context-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:applicationContext.xml</param-value>  这里我就不分开配了,把dao,service,事务都配在同一个文件中
 </context-param>
 
 --这个是一个参考:是2012的工程中的配置,它的名字又不一样.
 <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:config/spring-context.xml</param-value>
   </context-param>
 
 <!--Spring监听器. spring本质上是一个容器,无论你的配置文件最后叫applicatonContext.xml还是叫spring.xml又或者叫spring-context.xml,都
 没关系,主要是在这个配置文件中你要对数据源,事务,注解等进行整合.当web工程启动时候,会根据容器配置去加载容器,监听器就是用来监听请求的,
 一旦发现了符合条件的请求,就会去加载真正的容器servlet. 在没有框架的时候servlet才是真正的容器,但是现在有了一个强大的啥都能干的spring,
 那么spring就成了容器了,一个包含了servlet的大容器. 而spring中的始祖servlet就是DispatcherServlet,它是spring的核心控制器.-->
 <listener>
  <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
 </listener>


 <!-- 这就是spring的核心控制器DispatcherServlet,也是前端控制器 ,这个控制器在加载的时候需要一个springMVC的配置文件,这个配置文件默认是放在
 WEN-INF根目录,如果你的文件不是放在根目录,那么init-param标签就是用来指定你这个配置文件的路径的.注意这里的servlet-name一定要和配置文件的名称
 保持一致. lode-on-startup这个标签的作用是告知服务器啥时候创建servlet实例,如果值>=0,那么就是在web服务启动时就初始化这个servlet,数字越小
 创建时间越早,小于0或者不指定时,表明在使用的时候才初始化.-->
 <servlet>
  <servlet-name>springmvc</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <init-param>
   <param-name>contextConfigLocation</param-name>
   <!-- 注意这个servlet在加载的时候会用到springmvc.xml中的配置,里面有视图,拦截,静态资源之类的东西 -->
   <param-value>classpath:springmvc.xml</param-value>
  </init-param>
  <load-on-startup>1</load-on-startup>
 </servlet>
```
```
 <!--这个里面是控制器拦截的请求中类: *.do *.action 拦截以.do结尾的请求 (不拦截 jsp png jpg .js .css); / 拦截所有除了
 .jsp和静态资源以外的请求; /* 拦截所有请求（包括.jsp) 啥都拦截所以不建议使用.   由于/会放行.jsp和静态资源,所以有时候我们会
 看到有的配置文件中会这样配置,先配 / 然后再把需要拦截的静态资源单独配上,这样既不用像/* 那样拦截所有的请求,又可以达到目的-->
 --常见的就是这种配置
 <servlet-mapping>
     <servlet-name>springMVC</servlet-name>
     <url-pattern>/</url-pattern>
 </servlet-mapping>
 --下面这些静态资源可以根据需求进行添加
 <servlet-mapping>
     <servlet-name>default</servlet-name>
     <url-pattern>*.html</url-pattern>
 </servlet-mapping>
 <servlet-mapping>
  <servlet-name>default</servlet-name>
  <url-pattern>*.png</url-pattern>
 </servlet-mapping>
 
 
 <!-- 这个是个 POST提交过滤器 ,也就是拦截前台来的 *.action的编码是UTF-8的post请求. get应该也会拦截,回头确认下-->
 <filter>
  <filter-name>encoding</filter-name>
  <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  <init-param>
   <param-name>encoding</param-name>
   <param-value>UTF-8</param-value>
  </init-param>
 </filter>
 <filter-mapping>
  <filter-name>encoding</filter-name>
  <url-pattern>*.action</url-pattern>
 </filter-mapping>
 
 
 <!-- 有的项目还会有一些别的过滤规则,你只要配置相应的jar包和需要对那种请求进行检验就行.比如这个是个 xss过滤,还有别的跨站点伪造啊
    自定义过滤啊之类的乱七八糟一大堆可以根据需要搜索并配置. -->
 <filter>
     <filter-name>xssFilter</filter-name>
     <filter-class>com.huawei.netcare.lite.common.filter.XSSFilter</filter-class>
 </filter>
 <filter-mapping>
     <filter-name>xssFilter</filter-name>
     <url-pattern>/*</url-pattern>
 </filter-mapping>
 
 <!-- 最后这个是一些异常页面的配置,你在工程中写好了这些页面之后如果有异常就让跳转相应的页面就是了-->
 <error-page> 
  <error-code>401</error-code> 
  <location>/401.jsp</location> 
 </error-page>
 <error-page> 
  <error-code>403</error-code> 
  <location>/403.jsp</location> 
 </error-page>
 ............................ 
</web-app>
```
   - Spring.xml 或者叫applicationContext.xml,又或者叫spring-context.xml,无论叫啥,最终都是是一个意思,
   都是整合数据库,事务,注解等东西,这里我把这些都配到一个文件中,也可以根据功能拆分成三个以方便管理.
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:p="http://www.springframework.org/schema/p"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
 xmlns:context="http://www.springframework.org/schema/context"
 xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
 xmlns:task="http://www.springframework.org/schema/task" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
 xsi:schemaLocation="http://www.springframework.org/schema/beans 
  http://www.springframework.org/schema/beans/spring-beans-4.0.xsd 
  http://www.springframework.org/schema/mvc 
  http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd 
  http://www.springframework.org/schema/context 
  http://www.springframework.org/schema/context/spring-context-4.0.xsd 
  http://www.springframework.org/schema/aop 
  http://www.springframework.org/schema/aop/spring-aop-4.0.xsd 
  http://www.springframework.org/schema/tx 
  http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
  http://www.springframework.org/schema/task
     http://www.springframework.org/schema/task/spring-task-4.0.xsd
  http://code.alibabatech.com/schema/dubbo        
  http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
  
 -----------以下配置可以单独配成applicatonContext-dao.xml   上面的部分三个可以通用
 <!--引入propertise文件这个文件是数据库信息配置文件,这个文件可以单独写个db.properties,也可以把数据库的信息配置到项目使用文件中去,
 比如我们的项目使用的就是pconfig.properties.属性名称是为了在文件的其他地方对这个属性进行引用, value是真正的文件地址和路径.-->
 <context:property-placeholder location="classpath:db.properties" />
 <!--配置数据源:  常见的连接数据库的工具有:jdbc c3p0 dbcp ,druid等/// c3p0和dbcp都是数据库连接池,只不过两者管理连接池的思路不同,
 C3P0提供最大空闲时间，当连接超过最大空闲连接时间时，连接就会被断掉。DBCP提供最大连接数,当连接数超过最大连接数时，所有连接都被断开
 而jdbc就是一套直接和数据库交互的api,它没有连接池的概念,交互一次我就连接一次,性能上肯定是比不了前两个,所以现在很少使用了!!!
 而druid是阿里出的一个用于管理数据库连接的工具,反正比上面三个都吊,很多情况下也能见到的是c3p0,看需求和习惯了-->
 <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
  <!-- 数据库驱动 -->
  <property name="driverClassName" value="${jdbc.driver}" />
  <!-- 连接地址 -->
  <property name="url" value="${jdbc.url}" />
  <!-- 用户名 -->
  <property name="username" value="${jdbc.username}" />
  <!-- 密码 -->
  <property name="password" value="${jdbc.password}" />
 </bean>
 
 <!-- 配置 Mybatis的工厂 -->
 <bean class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource" />  //绑定数据源
  <property name="configLocation" value="classpath:SqlMapConfig.xml" />  //指定Mybatis的核心配置文件所在位置
  <property name="typeAliasesPackage" value="com.company.ssm.crm.pojo" />   //配置pojo别名,这个不太理解是干啥的后续看看
 </bean>
 
 //这个标签的作用暂时不是很理解,后续看看
 <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
  <property name="basePackage" value="com.company.ssm.crm.dao" />
 </bean>
 
 ----------以下配置可以单独配成applicatonContext-service.xml   开头部分三个通用
 //这个和上面那个标签一样,作用和原理还不太理解,不过肯定是分别用于扫描项目的dao层和service层.上面那个应该是个原始标签,下面是个简写
 <context:component-scan base-package="com.company.ssm.crm.service" />
 
 -----------以下配置可以单独配成applicatonContext-transaction.xml  开头部分三个通用
 <!--4.配置事务管理器  :事务就是一系列的动作，它们被当作一个单独的工作单元。这些动作要么全部完成，要么全部不起作用.
 本来数据库的连接和操作是Mybatis来管的,但是现在Mybatis和spring整合了,那么任务的最高权限就交给spring了,虽然与数据库的相关操作都还是
 Mybatis来执行,但是什么时候执行,什么时候结束,遇到错误该咋办...这些Mybatis你就不用操心了,由我spring来处理,这就是事务管理.相当于就是
 个责任委托. spring有很多用于实物管理的类,现在我们只整合了Mybatis所以只需要配置管理数据库的事务,加入你要跟hibernate整合那你就再配置
 用于管理hibernate的事务就行了.需要让啥被spring管理就配啥. 在事务管理中有个事务回滚的概念,当一个有关联的连续事务操作发生了异常,那么
 会进行事务回滚,一次返回到执行事务最开始的状态.-->
 <bean id="transactionManager"   // 这里配置了一个事务管理器,用来管理对于数据库的操作.
  class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  <property name="dataSource" ref="dataSource" />  //事务管理器的属性是数据库,你要管理别的事务你的属性肯定就是别的.
 </bean>
 
 // tx标签指定了一些具体的操作,这些操作和刚刚上面配置的那个事务管理器相关联.也就是说把下面的这些操作交给上面那个事务管理器去管理
 <tx:advice id="txAdvice" transaction-manager="transactionManager"> //这里配置了一个具体的行为,它和你刚配置的那个事务是关联的
  <tx:attributes>
   <!-- 传播行为 -->
   <tx:method name="save*" propagation="REQUIRED" />    //在这个具体的行为中我们看到了对于数据库的增删改查等操作
   <tx:method name="insert*" propagation="REQUIRED" />  // 看到下面update的操作那一条中的 rollback-for="Exception"那个配置了吗
   <tx:method name="add*" propagation="REQUIRED" />    // 这就是给update这个操作设置了事务回滚.回滚的条件是当update的方法发生了
   <tx:method name="create*" propagation="REQUIRED" />   //指定的异常,那么就会进行回滚. 你可以像这个配置这样吧需要回滚的方法配置
   <tx:method name="delete*" propagation="REQUIRED" />  //在配置文件中,也可以直接在类上或者类里面的那个指定的方法上用@Transactional注解
   <tx:method name="update*" propagation="REQUIRED" rollback-for="Exception"/>  //注意这一条的配置,添加了回滚条件
   <tx:method name="find*" propagation="SUPPORTS" read-only="true" />   
   <tx:method name="select*" propagation="SUPPORTS" read-only="true" />  
   <tx:method name="get*" propagation="SUPPORTS" read-only="true" />
  </tx:attributes>
 </tx:advice>
回滚机制 : 
1.Spring事务管理是根据异常来进行回滚操作；
2.Spring与Mybatis整合时，虽然在Service方法中并没有check异常，但是如果数据库有异常发生，默认也会进行事务回滚;
3.Spring 如果不添加rollbackFor等属性,碰到Unchecked Exceptions,RuntimeException,Error都会回滚,如果添加了那么就按你添加的异常进行回滚.
4.如果在需要被管理的方法中(比如刚的那个update方法)捕获了异常并进行了处理，一定要继续抛出异常,这样你在配置文件中的rollback才能识别到异常并进行
回滚,否则捕获了不处理也不抛出是不会回滚的.因为要回滚肯定是遇到了异常,尼玛你把异常都在代码里捕捉了spring自然就没法得知这个异常自然无法回滚.所以
你要么就啥也不干不try-catch,要么就捕获并重新抛出.很多时候我们在代码中的service中调用了dao层,但是我们并没有在调用的时候把那句话用try-catch包起来
就是这个道理.因为你查一次库就相当于一次事务操作,spring用了默认的方式来管理你的这次操作,如果你的这个更新操作发生了异常,比如你往数据库更新数据刚
更新了一半断电了,那么你的这次事务就被取消了,恢复到一个都没更新的状态.

还有一种被我们忽略的事务回滚,其实就是在代码里直接写,比如你写了几行代码,然后对这几行代码的执行结果进行了判断,如果满足条件就继续执行后续的代码,否则
则不执行,事务管理表现在代码上实际上就是一个条件判断,成立则继续,不成立则执行别的,但是这种方式要写很多重复代码而且你得把所有相关的东西都得写全才行,
所以实际中这种情况比较少见.

 <aop:config> //这个就是切点的配置
  <aop:advisor advice-ref="txAdvice"  //上面那一堆操作就是一个切面,这个切面被在这里引用
   pointcut="execution(* cn.itcast.core.service.*.*(..))" />  //而pointcut属性就是切点,也就是你的那个service类,这里配置了
 </aop:config>                                // *.* 所以就是service中的所有方法都是这个事务中被管理的对象
</beans>
```

  - springmvc.xml  这里面才是真正的控制器
```
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
 xmlns:context="http://www.springframework.org/schema/context"
 xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
 xmlns:task="http://www.springframework.org/schema/task" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
 xsi:schemaLocation="http://www.springframework.org/schema/beans 
  http://www.springframework.org/schema/beans/spring-beans-4.0.xsd 
  http://www.springframework.org/schema/mvc 
  http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd 
  http://www.springframework.org/schema/context 
  http://www.springframework.org/schema/context/spring-context-4.0.xsd 
  http://www.springframework.org/schema/aop 
  http://www.springframework.org/schema/aop/spring-aop-4.0.xsd 
  http://www.springframework.org/schema/tx 
  http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
  http://www.springframework.org/schema/task
     http://www.springframework.org/schema/task/spring-task-4.0.xsd
  http://code.alibabatech.com/schema/dubbo        
  http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
 <!-- 加载属性文件 -->
 <context:property-placeholder location="classpath:resource.properties" />
 
 <!--配置扫描器.自动扫描你在这里配的包中使用了注解的类.这个base-package属性就是表示要进行自动扫描的包.spring会自动去扫描你配的那个包或者其
 子包中的java文件,如果扫描到带有@controller或@service或@component等注解的类,则把这些类注册为bean,这里配的是指只扫@controller的,注意自动扫描
 的配置是下面那一行配置,应该把下面那个简化的配置放到这个配置上面比较合适,虽然放哪都没错. 如果我们从浏览器要访问一个web资源,请求会先被servlet拦截,
 然后servlet去扫描配置的路径下有@controller的类,然后你的方法里肯定还有个@requestmapping的参数,里面配了你页面访问的那个资源到底对应到哪个方法.
 只要你有自动扫描的配置,在代码里和url中也正确的把映射关系整对了,那就会自动去找到正确的方法.-->
 <context:component-scan base-package="com.company.ssm.crm.controller" />
 
 <!--  会自动注册spring分发请求所必须的三个bean,这三个bean是@Controllers分发请求所必须的，相当于是你不做处理时的一种默认配置.
    spring3.0以后这三个bean进化成了AbstractHandlerMethodMapping,AbstractHandlerMethodAdapter,AbstractHandlerMethodExceptionResolver,
 只要加了这个配置就会默认初始化这三个bean,以便于让用户更方便的实现自定义的实现类。实际上这是一种简写方式, 完全可以用原始的配置
 方式来更改这三个bean,比如下一个标签就用原始方式重新配置了一个视图解析器,这样在初始化的时候前两个就会用默认,而视图解析用你配的上-->
 <mvc:annotation-driven />
 
 <!-- 配置了一个自己的视图解释器 专门用来解析jsp -->
 <bean id="jspViewResolver"
  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  <property name="prefix" value="/WEB-INF/jsp/" />
  <property name="suffix" value=".jsp" />
 </bean>
</beans>
```
  - SqlMapConfig.xml  大多东西都交给spring来管理了,有些会将sql.xml配在这个里面. 这个后续再看下为啥这个和springmvc的配置一样呢?
```
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
 xmlns:context="http://www.springframework.org/schema/context"
 xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
 xmlns:task="http://www.springframework.org/schema/task" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
 xsi:schemaLocation="http://www.springframework.org/schema/beans 
  http://www.springframework.org/schema/beans/spring-beans-4.0.xsd 
  http://www.springframework.org/schema/mvc 
  http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd 
  http://www.springframework.org/schema/context 
  http://www.springframework.org/schema/context/spring-context-4.0.xsd 
  http://www.springframework.org/schema/aop 
  http://www.springframework.org/schema/aop/spring-aop-4.0.xsd 
  http://www.springframework.org/schema/tx 
  http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
  http://www.springframework.org/schema/task
     http://www.springframework.org/schema/task/spring-task-4.0.xsd
  http://code.alibabatech.com/schema/dubbo        
  http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
 <!-- 加载属性文件 -->
 <context:property-placeholder location="classpath:resource.properties" />
 
 <!-- 配置扫描 器 -->
 <context:component-scan base-package="com.company.ssm.crm.controller" />
 
 <!-- 配置处理器映射器 适配器 -->
 <mvc:annotation-driven />
 <!-- 配置视图解释器 jsp -->
 <bean id="jspViewResolver"
  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  <property name="prefix" value="/WEB-INF/jsp/" />
  <property name="suffix" value=".jsp" />
 </bean>
</beans>
```
 - db.properties  数据库信息的配置文件
```
#数据库的基本配置文件,在整合spring时,要从这个文件当中取出一些属性来使用,可以根据需要配置项目中使用的属性.
#连接基本设置 分别是 驱动,连接地址,用户名,密码
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/sys
username=root
password=root1234

---其他一些使用少的配置,可以按需使用
#初始化连接  就是数据库初始化的时候连接池要保持几个连接
initialSize=10
#数据库连接池的最大连接数量   连接数量是对于dbcp来说,而c3p0是根据最大空闲时间来控制连接数量的
maxActive=50
#最大空闲连接
maxIdle=20
#最小空闲连接
minIdle=5
#超时等待时间以毫秒为单位 6000毫秒/1000等于60秒
maxWait=60000
----剩下这些就很少见了基本不咋用,用的时候再细查.
#JDBC驱动建立连接时附带的连接属性属性的格式必须为这样：[属性名=property;] 
#注意："user" 与 "password" 两个属性会被明确地传递，因此这里不需要包含他们。
connectionProperties=useUnicode=true;characterEncoding=gbk
#指定由连接池所创建的连接的自动提交（auto-commit）状态。
defaultAutoCommit=true
#driver default 指定由连接池所创建的连接的只读（read-only）状态。
#如果没有设置该值，则“setReadOnly”方法将不被调用。（某些驱动并不支持只读模式，如：Informix）
defaultReadOnly=
#driver default 指定由连接池所创建的连接的事务级别（TransactionIsolation）。
#可用值为下列之一：（详情可见javadoc。）NONE,READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE
defaultTransactionIsolation=READ_UNCOMMITTED
```
