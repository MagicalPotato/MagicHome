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
    </bean>  //通过这样的方式,Spring会先初始化aPerson,然后使用它初始化myService.还是运行刚刚那个main容器的代码,
    //结果是一样的.容器会先去初始化一个person,然后再用person去初始化service实现类.用了ref这种方式相当于是把person传入了
    //service的构造方法.注意到配置的标签了没,constructor-arg就是说这是一个构造方法的参数,这个参数应用了一个person对象,
    //下面还有一个构造方法参数,类型是个String,值是hello. 
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
    <bean id="myService" class="com.skyline.service.MyServiceImpl"> //虽然现在配置了实现类,但是并没有用构造方法参数
      <property name="greeting" value="Hello"/>  //标签,也没有用ref去引用上面的person类,真正的引用换成了代码中的注解
    </bean>
</beans>
//这是换了注解的代码,由于我们不用构造方法来传参了,所以配置没用构造方法参数标签,代码中也没用构造方法,
但是用注解的方式代码中也是可以用构造方法的,并不是说你用了注解代码里就不能写构造方法那种了,你用get,set
给service实现类赋属性和你用构造方法然后传入对象的方式给它赋属性都是一样的.只不过构造方法是public,
而get和set是private,你根据你的类的情况选择就可以了.这个例子就没用构造方法
public class MyServiceImpl implements MyService {
    private Person person;   
    private String greeting; 
    public Person getPerson() { 
        return person; 
    }

    @Autowired // 新增
    public void setPerson(Person person) {   //这里新增了一个注解,自动依赖,这也是依赖注入的原型
        this.person = person;  //当容器初始化实现类的时候,先初始化了一个person,并给person的name属性赋了值
    } //而后又初始化了一个service实现类对象,并并给greeting属性赋了值,但没有在配置中标明person和实现类的关系
    //关系在类中用Autowired这个注解来标注了.当代码运行的时候,那个被初始化的person对象就被自动的赋给service实现类.
    public String getGreeting() {
        return greeting;
    }
    public void setGreeting(String greeting) {
        this.greeting = greeting;
    }
    @Override    //也就是说在这种方式下,容器只是初始化了所有需要的对象,但是对象的依赖关系并未确定,而是在代码
    public String sayHello() {   //执行的时候再去根据确定不同的类之间的依赖关系.
        return this.greeting + " " + this.person.getName();
    }
}

// 你当然可以把注解用在构造方法上
    @Autowired
    public MyServiceImpl(Person person)
    {
        this.person = person;
    }
// 还可以直接用在实例属性上,几种用法都是可以的.
    @Autowired
    private Person person;
```
18. 上面的例子里用注解来指导Bean的依赖关系,然而Bean本身的定义还是写在XML当中,下面我们换种方式,直接在代码中创建出 Bean,这种方法的官方叫法是 Component-Scan,component的意思是组件,构件,元件,零件等.那这种叫法就可以叫做组件扫描.
```
<?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context  
        http://www.springframework.org/schema/context/spring-context.xsd">
        //上面这一大堆配置到底是干啥的后续一定要搞清楚!!!
    <context:component-scan base-package="com.skyline"/>  //这就是组件自动扫描的配置
</beans> //有了自动扫描,那些乱七八糟的类的关系就不用配置了.注意我们还去掉了
// <context:annotation-config>就是那个自动注解的配置,因为组件扫描默认会开启自动注解

Spring 中定义了若干用于注册Bean的Annotation,包括@Component以及继承自@Component的@Service和@Repository。
所谓的component-scan也就是告诉Spring去扫描加了这些注解的类,并把他们实例化成bean对象. Spring中推荐使用
@Service和@Component标示逻辑和服务层的组件,推荐使用@Repository标示持久层的组件。实际上@Service和@Repository
都继承自@Component,因此在Bean实例化的层面,它们没有本质上的区别,也就是说在初始化类的时候你用任何一种注解都可以,
不过@Repository在持久层会有一个exception转换的作用,先暂且放下后续再看.现在我们来新建一个类;

import com.skyline.model.Person;  //这个类中用到了我们之前建的person类
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;

@Component //这个注解说明这个类可以被spring注册成一个组件,这个组件是专门用来生产对象的
public class MyComponent {
    @Bean("aPerson")  //相当于是配置文件中的那个唯一id
    public Person testPerson() {   //原本是在配置中配置了这个person类的信息,让spring去初始化
        Person aPerson = new Person(); //现在不在配置中配了,而是直接在代码的一个类中用一个方法
        aPerson.setName("Chester");  //来真实的new这样一个对象,并给对象赋值,然后返回这个对象.
        return aPerson;
    }
    @Bean("myService")  //这个也是一样,原本在配置中配的现在都写在代码里
    public MyServiceImpl testService() {  
        MyServiceImpl service = new MyServiceImpl();
        service.setGreeting("Hello");  //但是原来这个实现类还依赖了person的,为啥没把person也
        return service;  //set上呢,是因为person上有个@Autowired注解,还在起作用呢.只要加了自动 
    }  //依赖注解,无论是什么类,只要初始化的时候用到了person类,那么都会自动依赖过去.这样在用的时候
}      //才去依赖注入,明显降低了耦合度.
通过这样的方式,我们就把原本在配置中配置的依赖关系转移到了java类中,在java类中统一生产那些用到的对象
```
19. 上面的例子中我们把Bean和Bean的依赖关系都通过Annotation来表达,但还是没有彻底去掉service.xml这个古老的配置文件,虽然一再的演进精简,但最终还是在.这个例子我们就来去掉这个配置文件,完全使用Java代码进行配置。首先我们需要创建一个配置类,叫 AppConfig：
```
import org.springframework.context.annotation.ComponentScan; //原本的组件扫描配置被搞成了jar包导入
import org.springframework.context.annotation.Configuration; // 当然还导入了必要的配置包

@Configuration  //@Configuration是指示该类为Bean配置的源头,就跟service.xml一个作用。
@ComponentScan("com.skyline") //这个就是配置中组件自动扫描的注解,要把你要扫描的包配进去
public class AppConfig {
}
// 注意:只有标记了@Configuration的类当中才能出现类似XML中的Bean之间互相依赖关系,这个注解也继承自@Component,因此也可以用于自动扫描。
然后我们来修改我们的main容器类:
package com.skyline;  //你的包名
import com.skyline.service.MyServiceImpl; //你用到的实现类
import org.springframework.context.ApplicationContext;  //spring的容器接口
import org.springframework.context.annotation.AnnotationConfigApplicationContext; //自动配置容器的实现

public class MySpringContainer {
    public static void main(String[] args) {  //可以看到,以前我们是在容器里加载配置文件,现在加载配置类
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        MyServiceImpl service = context.getBean("myService", MyServiceImpl.class);  //然后获取了一个bean
        System.out.println(service.sayHello());  //运行我们的容器类,结果没变.
    } // 到现在为止,我们就可以完全去掉那个原始的services.xml配置文件了.但是我们发现在配置类中根本啥也没写啊,这是因为
} //我们再配置类中配置了自动扫描注解,当加载配置类的时候,就会去扫描你配置的包,然后在包里会找到你的加了@component注解
//的MyComponent组件类,然后组件类中的各个bean就继续初始化,最终当你在容器中用context来getBean的时候就可以拿到对象了.

事实上,那个组件类是完全可以去掉的,我们完全可以把组件类中的内容移到配置类中,只要能完成功能,随你折腾,你写哪里都行
@Configuration
@ComponentScan("com.skyline")
public class AppConfig {
    @Bean   //看到没,我把组件类的代码全都移到了配置类,这完全可以,流程没有任何影响
    public Person aPerson() {
        Person aPerson = new Person();
        aPerson.setName("Chester");
        return aPerson;
    } //有个注意点:我们用了简化的@Bean定义,这样Spring会直接把函数名称aPerson和myService作为Bean的名称,运行后结果没变。
    @Bean  //前面那个组件类中我们是指定了bean的名称.各有各的好处,你在容器中getBean的时候注意就行了
    public MyServiceImpl myService() {
        MyServiceImpl service = new MyServiceImpl();
        service.setGreeting("Hello");
        return service;
    }
}
```
20. 到此为止,我们从最简单的一个项目开始,一路演进,从单纯的使用java提供的servlet接口到使用框架,框架中又从有配置演进到没配置...当然后续可能还会有各种新奇的玩意,我们姑且不去管.当前来看,大多的项目还是停留在框架 + 配置的阶段.使用XML的好处在于更新Bean之后不需要重新编译代码，同时有利于将若干Bean组织
在一个文件里,方便集中管理。缺点在于xml书写很繁琐,也很容易写错。使用代码的好处在于更加简洁清晰,不容易出错,缺点在于Bean的配置会分散在各个文件当中,以
及改了文件之后需要重新编译代码才能更新配置。从Spring本身的发展来看,使用代码（即 Annotation）进行配置逐渐取代了XML成为主流的配置方式,到最新的SpringBoot框架,XML已经基本不见踪影,因为随着各种版本管理工具诸如maven的不断进化,改个代码并编译一下实际上变得越来越简单.
21. spring在初始化一个bean的时候实际上是有一些隐藏操作的:
```
@Bean  //当我们指定了一个bean后,实际上这个bean是有个默认的注解的,就是单例注解
// @Scope("singleton") // 这是默认的,也就是说如果未加干涉,所有的bean都只会生成一份,这也是为啥改了类要重新编译的原因
// @Scope(value = ConfigurableBeanFactory.SCOPE_SINGLETON)   //这是单例的另一种写法
// @Scope("prototype") //而这个就是多例了,如果显示地配了多例模式,那么你改了bean的东西就不需要刻意去编译,加载的时候自动会去搞一个全新的
public Person aPerson() {
    Person aPerson = new Person();
    aPerson.setName("Chester");
    return aPerson;
}
还有一个问题也注意下:还是刚刚那个组件类,加入组件里有两个person类,spring如何去自动依赖呢,事实上当容器开始加载的时候这种会报错,
No qualifying bean of type 'com.skyline.model.Person' available: expected single matching bean but found 2: aPerson,bPerson
不像xml中使用id来引用对象, @Autowired依赖注入从本质上就是一种基于类型的依赖机制,就是说是根据返回类型来判断的,如果有了同样类型的对象
那么就屌了,找不到了.
@Component
public class MyComponent {
    @Bean
    // @Primary 一种解决方式是在组件中定义优先级
    // @Qualifier("Chester") 另一种方式是像xml那样定义id,这个叫chester
    public Person aPerson() {
        Person aPerson = new Person();
        aPerson.setName("Chester");
        return aPerson;
    }
    @Bean
    // @Qualifier("Mike")  这个叫Mike
    public Person bPerson() {
        Person aPerson = new Person();
        aPerson.setName("Mike");
        return aPerson;
    }
    @Bean("myService")
    public MyServiceImpl testService() {
        MyServiceImpl service = new MyServiceImpl();
        service.setGreeting("Hello");
        return service;
    }
}
然后在你的service实现类中用到person的地方用你刚加的那个id来标记
@Autowired
@Qualifier("Mike") //我标记了我要用Mike那个 
private Person person;
---------------------------------拿个真实项目的例子来看-----------------------
controller被页面调用,在调用的过程中会注入一个service,service里面又注入一个dao完成相应功能:

@RestController
@RequestMapping("/import")
public class AlarmImportControllor
{
    @Autowired
    @Qualifier("alarmServiceImpl")  //控制类需要依赖一个名字叫alarmServiceImpl的组件类,然后spring就去扫描整个包找这个类
    private IAlarmService alarmServiceImpl; //这样的话接口实际上就可以省略了,直接用实现类的类型就可以,因为注入是根据类型来识别的
    
    .................. 
}

@Service("alarmServiceImpl")  //实现类用注解表明自己是一个可以被容器初始化的组件bean,并且名字是alarmServiceImpl
public class AlarmServiceImpl implements IAlarmService
{
    @Autowired                 //service里面又去依赖dao
    @Qualifier("coreNetNeDao")
    public CoreNetNeDao coreNetNeDao;
    
    .....................
}

@Repository(value = "coreNetNeDao")  //而dao最后就用这个注解来唯一标识自己,可以看出service用@Service,而dao用@Repository
public interface CoreNetNeDao        // Service在注解括号中直接指定名称,而Repository则是用了键值对的方式,最终殊途同归
{
    ...............  //dao里没有可以依赖的了
}
```
