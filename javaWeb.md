#### 一些javaWeb中需要注意的点
1. J2EE, JavaEE 以及 JEE 其实都是同一个东西，只不过由于历史原因出现了若干名称,这些都可统一称之为JavaEE.JavaEE是对JavaSE(标准版) 的扩展，加入了面向企业开发（实际上就是网络和 Web相关）的支持，包括 Servlet，WebSocket，EL，EJB 等。简而言之,JavaEE 就是JavaSE + 更多的jar包，这些jar包命名以javax开头，例如 javax.servlet, javax.websocket 等。
2. Servlet是JavaEE的诸多组件的其中之一,它只是一套用于处理HTTP请求的API标准,也就是说只是一组接口,真正的实现是靠开发者去实现的.
3. Servlet的运行需要容器的支持,但是JavaEE本身没有提供Servlet Container,这样做是有好处的,因为容器就是环境,servlet不和某个确定的容器相关,意味着Servlet可以跑在任何一个Container当中，避免了对Runtime环境的依赖。比较常见的支持Servlet Container的外部Server软件有 Apache Tomcat，Glassfish，JBoss，Jetty 等等。
4. EJB全称Enterprise JavaBean，它也是JavaEE的一个组件，面向更加复杂的企业业务开发。对于Web来说,EJB不是必须。且和 Servlet 类似，运行EJB也需要专门的 EJB Container。但并不是所有的外部Server软件都支持EJB。Tomcat就不支持EJB，而JBoss提供了对EJB的支持。**后续了解下**
5. Spring是一个非常庞大的框架，包括SpringMVC，SpringBoot以及SpringCloud等用于Web开发的工具.Spring不需要完整的JavaEE内容,仅仅依赖最基础的 Servlet，也不需要EJB Container，只用普通的Servlet Container就可以运行。Spring和JavaEE不是一个层面上的东西。Spring 仅依赖了JavaEE的API 标准，最新版的Spring甚至进一步隔绝了JavaEE的API。开发者可以完全不关心Servlet或者JavaEE等概念，也可以进行Java Web开发。Spring不是唯一的Java Web框架，还有Structs,Spark等
6. 做web开发必须的两大元素:JDK和tomcat,JDK可以用JavaSE的JDK,标准版的JDK也是大家使用最多的JDK版本。当然还需要下载一个Tomcat.注意jdk的版本和tomcat的版本要尽量匹配,如果jdk用的是1.8的,那么tomcat也用8.0的,这样能避免以后很多莫名其妙的问题.
7. 一般创建一个工程之后都会有src目录,里面放了实现功能的代码,要跑这些代码,我们需要把它部署到Tomcat上。首先在src目录隔壁，创建一个WEB-INF目录（注意名字一定要正确），然后在里面创建一个 web.xml文件,里面会配置请求和处理请求的servlet的信息. web.xml的作用是告诉Tomcat，我们想使用哪个Servlet 来处理对应的请求。Tomcat通过web.xml找到对应的Servlet完成请求以及响应过程。然后到Tomcat的目录，其中有一个webapps文件夹，这个里面相当于就是放一个个的web工程,在里面创建一个新的MyFirstServlet文件夹(就是你刚建的工程)，然后把整个WEB-INF文件夹拷到这个文件夹里面,最后再把代码编译出来的class拷贝到如图对应的目录下:
```
webapps              # 看到没,其实一个简单的web项目用到的最基本的东西就这么一点.
  - MyFirstServlet   # 一个tomcat,一个web.xml,然后是你的工程编译出来的class.
    - WEB-INF        # 由此可以引申到java的一次编译到处运行的概念. 不同的tomcat相当于不同的环境,一次编译出来的
      - classes      # class文件只要部署到对应的tomcat上就可以运行了.
        - com
          - skyline
            - MyFirstServlet.class
      - web.xml
```
8. 工程所需要的东西都在tomcat中部署好之后,工程和tomcat其实就没关系了,相当于你的工程已经放到了环境上,然后启动tomcat,那么你这个tomcat就成了一个单独的环境,就可以被外部进行访问了.启动tomcat用其bin目录下的start脚本. 会遇到环境变量没配置或者端口冲突等问题,自行查找解决.
9. **注意了,重新整理下思路,看下面这个例子:这才是真正的技术演进,从单独的servlet到servlet和jsp的结合,在第11条会演进到Spring框架**
```
public class MyFirstServlet implements Servlet {  # 最原始的时候我们是实现了Servlet这个接口,然后重写其中的一些方法
    public void init(ServletConfig config) throws ServletException {
        System.out.println("Init");
    }
    public void service(ServletRequest request, ServletResponse response)
            throws ServletException, IOException {
        System.out.println("From service");
        PrintWriter out = response.getWriter();
        out.println("Hello, Java Web.");    #  service方法里的东西最终会被展示到页面上
    }
    public void destroy() {
        System.out.println("Destroy");
    }
    public String getServletInfo() {
        return null;
    }
    public ServletConfig getServletConfig() {
        return null;
    }
}
    <servlet>
        <servlet-name>MyFirstServlet</servlet-name>
        <servlet-class>com.skyline.MyFirstServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>MyFirstServlet</servlet-name>
        <url-pattern>/hello</url-pattern>  #在web.xml中我们是这样配置的,让/hello这个资源能够索引到我们的MyFirstServlet上
    </servlet-mapping>
    
这是调用地址 http://localhost:8080/MyFirstServlet/hello,可以看到这时候我们是直接访问写好的servlet,这是在web.xml中配置的

这是另一种实现方式
public class MyFirstServlet extends HttpServlet { # 现在是继承HttpServlet类了
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 这里实现处理逻辑
        
        response.getWriter().write("<html>");  //下面这些都是要返回的结果,但是在代码中写
        response.getWriter().write("<body>");  // 这么一大堆response.getWriter()明显很啰嗦又复杂且不易维护
        response.getWriter().write("<h2>");
        response.getWriter().write(LocalDateTime.now().toString());
        response.getWriter().write("</h2>");
        response.getWriter().write("</body>");
        response.getWriter().write("</html>");
    }
}
可以看到，直接使用 Servlet生成网页，你就得写一大堆response.getWriter().write(""),不仅代码写起来困难，可维护性也不高。
为了把HTML这些非逻辑的部分抽离出来，出现了JSP技术。JSP全称JavaServer Pages,可以理解成一种高度抽象的Servlet。
事实上JSP在运行期间会被编译成Servlet，因此JSP和Servlet可以认为没有本质上的差异，只不过写起来容易了很多.
Tomcat支持JSP 技术。下面我们使用 JSP 重写上面的程序,把这个文件保存成date.jsp并放在和WEB-INF平行的目录下
<%@ page import="java.time.LocalDateTime" %>
<html>
<body>
<h2>
<%
out.write(LocalDateTime.now().toString()); // 这里的实现逻辑是打印当前时间
%>       // jsp里可以用代码直接实现逻辑,然后这些动态逻辑和静态页面的东西在运行时会被编译servlet
</h2>    // 也就是会被编译成那一大堆的response.getWriter().........
</body>
</html>
这时候我们的调用地址变成了这样 http://localhost:8080/MyFirstServlet/data.jsp
```
10. Spring框架实际上就是一堆jar包,直接下载下来放入工程jar包路径下就可以,同时还需下载Apache出品的common-logging,因为Spring依赖它.
11.  SpringMVC是构建在Servlet基础之上的,它对外提供了一个名为DispatchServlet的类，这个类是SpringMVC和Servlet API的一个交界点,也是 SpringMVC 当中对于请求处理的一个分发器,它将Servlet传递过来的请求根据URL分发给对应的Controller。在前面的内容中我们可以看到,通过在web.xml当中配置url-pattern也可以起到分发请求的作用，不过当大量的URL都需要在web.xml当中进行配置的时候，整个web.xml就变成了一个灾难。**因此广大 Web 框架都选择避免 web.xml，并在web.xml之上构建自己的请求分发机制**,当web.xml的url请求被Spring托管之后,我们便有了Spring的配置文件,这个配置文件中关于请求分发的就变成了这样:
```
    <servlet>
        <servlet-name>MyFirstServlet</servlet-name>  # 用于请求处理的servlet被配置成了Spring的Servlet
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  # 全jar包路径引用
        <load-on-startup>1</load-on-startup>  # 多了这个配置,意思是告诉Container容器
    </servlet>                                # 在启动的时候(而不是请求发过来的时候)就加载这个Servlet
    <servlet-mapping>
        <servlet-name>MyFirstServlet</servlet-name>
        <url-pattern>/*</url-pattern>    # 这里拦截了所有的请求.
    </servlet-mapping>
    
Sprint接收到请求之后会把请求交给对应的执行类,而这些执行类就是我们通常所指的controller类:相比直接使用Servlet API,
编写 Controller 可以说是容易了太多。这里创建一个MyFirstSpringController类,这个类还是在原来的工程src下,
至于包名叫什么你可以自己定义,之前我们用的是直接编写servlet类的方式,所以我们建的是servlet类,但是现在我们要改成controllor类了:
package com.skyline;  //你的工程包
import org.springframework.stereotype.Controller;  //这里导入了框架的controller注解包
import org.springframework.web.bind.annotation.RequestMapping;  //下面这三个也是框架里用来表示不同意义的包,Spring的
import org.springframework.web.bind.annotation.RequestMethod;  // 一大优点就是使用一些Annotation(注解)来简化代码的编写 
import org.springframework.web.bind.annotation.ResponseBody;

@Controller   // 通过 @Controller 我们将一个普通 Java 类标记成一个 Controller 类
public class MyFirstSpringController {  //通过@RequestMapping和@ResponseBody我们将一个普通函数标记成可以处理GET
    @RequestMapping(value = "/hello", method = RequestMethod.GET)  //请求并返回字符串的Handler,handler就相当于一个处理器
    public @ResponseBody String Hello() {
        return "Hello, SpringMVC.";
    }
}

当我们编写了一个Controller 之后,怎么样让Spring的DispatchServlet找到我们编写的Controller呢？依然是通过配置。
在WEB-INF文件夹下,也就是web.xml同目录,创建一个MyFirstServlet-servlet.xml（注意命名）,注意,这个配置实际上就是Spring的配置,
所以可以起包含Spring,servlet等字眼的名字,最好把你的工程名也加上.配置内容如下:
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

        //  在这里我们将MyFirstSpringController加入了beans列表，SpringMVC就能找到我们的Controller并且进行初始化了,上面那一堆暂且不知道
        // 是干啥的也先不管. Spring后续提供了自动扫描并创建bean的功能,也就不需要手动去配置controller了,最初的版本还是需要配置的.
        <bean class="com.skyline.MyFirstSpringController"/> 
</beans>

controller编写完,配置也配好之后,还是像之前那样将controller编译成class文件并放在WEB-INF/classes对应的目录下,同时由于引入了依赖，我们还需要将
Spring 以及之前提到的 common-logging jar 包拷贝到 WEB-INF/lib,然后启动tomcat并调用 http://localhost:8080/MyFirstServlet/hello即可展示内容.
因为你的controller中Hello方法已经用注解指定了调用的地址是/hello,方式是get: @RequestMapping(value = "/hello", method = RequestMethod.GET).
到此为止,一个前后台的交互流程就从简单的servlet演进到httpServlet + web.xml的形式并最终演进到spring框架.
```
12. Java本身就自带了一些注解,例如@Override用来标记方法是一个重载方法,@Deprecated用于标记代码为废弃。注解不会对源代码产生任何影响,但是我们可以
通过在**编译器**或者**运行期**检查代码中的注解,为代码引入更多的功能。
13. Java本身就自带框架属性,为了使用java的框架功能,需要继承特定的类,实现特定的接口,就比如我们刚刚写的那个MyFirstServlet类,这个类就是继承了接口HttpServlet,事实上这样做会加大代码的耦合度.像这样通过继承或者实现java原生接口产生的类我们一般认为其依赖比较重,我们姑且称之为重(zhong)类,以此
来区分即将登场的POJO轻类. Spring出现之后,出现了POJO这种概念,它的全称是Plain Old Java Object,即普通的Java对象,我们随便建一个类new出来的对象实际上都可以称之为POJO,但是这个概念在框架出来之后用来特指那些能够尽量脱离java原生框架束缚的松耦合的类,就比如上面演进到框架之后的那个controller类,那个类我们没有去继承java原生框架任何东西,而是用一些注解来实现额外的功能.注解刚也提到了,并不会对代码产生影响,既然不影响代码,但又能实现一些功能,又脱离了java原生框架,降低了耦合,何乐而不为!所以POJO这个概念是在历史发展的特定环境下产生的一种概念,是给老概念赋予了新的意义; 同样,JavaBean也是这样一种概念,最开始的时候,JavaBean是对用 Java编写数据Model层类的一种叫法,这些model类一般要有如下特点：1有无参的构造器,2所有的成员都是private,对外暴露getter和setter,3实现Serializable.但是后来随着历史进程框架兴起JavaBean这个概念也逐渐的泛化,不再局限于Model类,出现了所谓的业务Bean等,对于Serializable的要求也可有可无,刚刚那个controller类,它不仅仅是一个POJO,也是一个 JavaBean。再到后来,Spring 把很多东西包括自己的一些**组件**也都称为Bean,可以看成是对JavaBean进一步的泛化。总而言之,在框架当中,一个有具体功能的类就是一个组件,也就是一个bean,比如你写的一个service类,或者一个model类,都可以称之为一个bean.
14. spring_servlet.xml中我们把controller配置成了一个Spring的bean组件,那么Spring在运行的时候就会去加载这个controller,然后在Controller类中,
我们又用@RequestMapping做了标记, 这样在初始化Controller的时候，SpringMVC对@RequestMapping注解当中的信息进行处理,就可以得到这些信息并把请求分发
到对应的Controller中. @ResponseBody 表明函数的返回值应该被用作HTTP返回的body处理。SpringMVC用我们返回的字符串生成HTTP响应并最终返回给了客户端。
这就是SpringMVC大体的工作流程,可以看到SpringMVC通过Annotation以一种低侵入性的方式,提供了一套简洁好用的Web开发的API。
15. Sprint最常被提及的几个特性是控制反转,依赖注入还有面向切面,单纯的去解释这些概念实在很笼统,我们来从一个具体例子看看到底什么是控制反转,谁控制了谁,谁反转了谁以及谁依赖了谁,又注入了什么玩意,看完这个例子或许你对这些概念会有一种全新的认识: 我们平常所说的ioc,实际上是一个动词,指的是一系列的动作,事实上ioc这个动作是有主语的,它的主语是Spring的IoC Container,也就是Spring的控制反转容器,很多人根本不理解本质,张嘴就说控制反转,到底反转了啥,反转了谁又是一头雾水.现在你可以试试这种理解方式:Spring框架有个容器,这个容器能够完成一些控制权转换的动作,我们把Spring容器的这种能力称为控制反转.**首先要说明的是,在编程领域,无论听起来是多么高大上的名称,最终都是一堆代码,Spring的容器是个啥?没错,它就是一堆代码.这堆代码被组织成了一些固定的方法,或者被打成了一个固定的jar包,我们往这个jar包中传入一些配置进去,然后这些配置在jar包里会发生各种变化,所以后来人们称这一堆代码或这个jar包为容器,这堆代码属于Spring,所以叫它Spring的容器,main方法就可以称为一个容器,你在main方法里面可以写各种调试或者临时验证问题的方法,那这个main不就像一个平台吗,一个小平台你叫他容器也没错啊**,下面我们就用一个main方法来进行一个角色扮演,让他扮演Spring中的容器,你刚不是在你的工程里建了一个controller类吗,你在建另外一个类,这个类里面有个main方法,类名可以随便起,类名我们就叫MySpringContainer吧,但你要记住这个是假装的container:
```
public class MySpringContainer {  // 这就是我们要假装它是Spring容器的类
    public static void main(String[] args) {  // 现在容器里我们啥也没干,待会我们会添加一些东西
    }
}
在往里面添加东西之前,我们先了解一下Spring IoC当中的几个概念：
1.Spring当中的IoC Container被称为Context,代码上体现为ApplicationContext接口.Spring自带了若干实现，
例如 ClassPathXmlApplicationContext 和 FileSystemXmlApplicationContext 等。
2.Spring的Container依赖于Configuration metadata（也就是配置文件）去初始化对象。
3.被Spring的容器Container 管理的对象，称为 Bean。根据Sun公司文档的定义,把可重用的Java组件称为Bean,
但在Spring里面,这个bean的意义发生了变化,注意不要和之前理解的定义混淆。

看第二条可以发现,在Spring最开始的概念中,容器是靠配置文件去初始化对象的,都是xml文件.
后来会逐渐演进到不用xml,但是我们先从用xml来开始:定义一个service接口和实现类:
public interface MyService {   // 这是接口类
    String sayHello();  //接口类里就定义了一个简单的接口
}

public class MyServiceImpl implements MyService {  //接口的实现类
    private String name;    //里面有个属性并且属性有get和set方法,由这个类new出来的对象我们
    public String getName() {  //实际上就可以称之为一个javaBean,也就是一个简单的java对象
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    @Override
    public String sayHello() {
        return "Hello " + name;   //实现类中的操作
    }
}

以前的时候你要用这个service实现类的对象,你的先new这个对象,然后再在代码里去用,但是Spring用
配置文件来初始化这个service实现类,不需要你在代码里去new了,在你的src包同级,建一个service.xml,
配置文件名称随意,但是你是要初始化service实现类,当然你的配置文件也就叫service.xml比较好,看到没
这里就会发现弊端,如果初始化的类特别多,配置文件肯定是个灾难,不过现在我们先不关注这个,先看配置文件:
<?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">  //上面这一堆我们暂且不管
    
    <bean id="myService" class="com.skyline.MyServiceImpl">  这里我们用一个id:myService和一个包路径唯一确定了我们的那个实现类
      <property name="name" value="Chester"/>  // 然后我们给那个实现类里面的name属性赋了个值,叫Chester
    </bean>  //这里再次发现了配置的弊端,加入一个类有一百个属性,那你配一百次也是个灾难
</beans>

当类准备好了,配置文件也准备好了,现在我们让我们的main容器去初始化这个service实现类:
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext; //会用到的框架包

public class MySpringContainer {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("services.xml");
        MyServiceImpl service = context.getBean("myService", MyServiceImpl.class);
        System.out.println(service.sayHello());  //看到没,容器自己去找了工程路径下的services.xml配置
    } //然后根据这个配置初始化了你的service实现类,最后say了个Hello
}  //现在只要直接运行你的这个容器类,那么这个sayHello的动作就可正常完成.很多让你就会有疑问了,你在你的main容器
中直接new一个service的实现类不是更方便吗,为啥非要再搞个配置文件多此一举呢? 很多东西啊不能只看表面,new一个对象
确实方便,但实际中我们的这个对象几乎不可能就只是say个hello就完事了,很多情况下都是在容器里构造出一堆对象之后把
这些对象给别的类去用,你如果在原本用这些类的那个类中去new,那么用的类和被用的类之间耦合度是不是一下就上去了,后续你要
改动一下那很可能你整个流程都或多或少得动一动,动完之后呢你又得重新编译重新打重新部署...所以啊,每个时期每种技术的出现
都是有它的背景和意义的,等到后续你会发现,其实配置也有很多弊端,到SpringBoot,我们连配置也不用了.但整个过程都是一点一点
慢慢演进的,并非一蹴而就,只有先了解了历史,后续你才能知道如何去修正历史.
```
16. 有了上面的基础,那后续也就好理解了,最开始容器只是初始化了一个类干了一件事,现在我们有了多个类,这多个类之间还有了依赖关系,那这个咋办呢,还是靠配置,假设现在我们的service实现类sayHello的时候它不say自己的name了,它要say另一个类的name,假设这个类叫Person类,那么原始情况下service实现类肯定是先在自己
的类中new一个person,然后取出person的属性,最后say这个属性.现在来看容器咋做:
```
public class Person {   // 这是个service实现类依赖的person类
    private String name;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
public class MyServiceImpl implements MyService {  
    private Person person;   //现在它有个别的类的属性
    // private String xxx   //假设自己的属性也还在,这个属性也可以在配置中配置
    public MyServiceImpl(Person person){  //这个service类初始化的时候它要传入一个person对象
        this.person = person;            //并赋给自己的person属性
    }
    @Override
    public String sayHello() {
        return "Hello " + this.person.getName();  //然后这里say的是传入的那个对象的属性
    }
}
//配置文件上面那一堆先省略,只看用到的
    <bean id="aPerson" class="com.skyline.model.Person"> // 首先是唯一确定了person类
      <property name="name" value="Chester"/>   //然后给person类的属性赋了值
    </bean>
    <bean id="myService" class="com.skyline.service.MyServiceImpl">  // 然后确定了service实现类
      <constructor-arg ref="aPerson"/>  // 最后用一个特定的标签引用了那个person类
      // <constructor-arg type="java.lang.String" value="Hello"/> 如果还有别的属性,也可以这样来给它赋值,但是属性多的话也是个麻烦,先记住
    </bean>  //通过这样的方式,Spring会先初始化aPerson,然后使用它初始化myService。运行代码,结果和之前是一样的。
```
17. 进一步简化配置,还是刚刚那个类,我们用注解的方式来引用:
```
<?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context" <!-- 新增 -->
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd"> <!-- 新增 -->

    <context:annotation-config/>      //在配置文件中加入这个标签就说明打开了根据注解自动识别的动能
    <bean id="aPerson" class="com.skyline.model.Person">
      <property name="name" value="Chester"/>
    </bean>
    <bean id="myService" class="com.skyline.service.MyServiceImpl"> //虽然现在配置了实现类,但是并没有用ref去引用上面的person类
      <property name="greeting" value="Hello"/>      //真正的引用我们交给了注解,看下面代码:
    </bean>
</beans>
```

