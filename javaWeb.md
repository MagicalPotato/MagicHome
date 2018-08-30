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
        <servlet-name>MyFirstServlet</servlet-name>  # 而用于请求处理的servlet被配置成了Spring的Servlet
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>  # 全jar包路径引用
        <load-on-startup>1</load-on-startup>  # 多了这个配置,意思是告诉Container容器在启动的时候(而不是请求发过来的时候)就加载这个Servlet
    </servlet>
    <servlet-mapping>
        <servlet-name>MyFirstServlet</servlet-name>
        <url-pattern>/*</url-pattern>    # 这里拦截了所有的请求.
    </servlet-mapping>
    
Sprint接收到请求之后会把请求交给对应的执行类,而这些执行类就是我们通常所指的controller类:相比直接使用Servlet API,编写 Controller 可以说是容易了太多。这里创建一个MyFirstSpringController类,这个类还是在原来的工程src下,至于包名叫什么你可以自己定义,之前我们用的是直接编写servlet类的方式,所以我们建的是servlet类,但是现在我们要改成controllor类了:
package com.skyline;  //你的工程包
import org.springframework.stereotype.Controller;  //这里导入了框架的controller注解包
import org.springframework.web.bind.annotation.RequestMapping;  //下面这三个也是框架里用来表示不同意义的包,Spring的一大优点就是
import org.springframework.web.bind.annotation.RequestMethod;  // 使用一些Annotation(注解)来简化代码的编写 
import org.springframework.web.bind.annotation.ResponseBody;

@Controller   // 通过 @Controller 我们将一个普通 Java 类标记成一个 Controller 类
public class MyFirstSpringController {  //通过@RequestMapping和@ResponseBody我们将一个普通函数标记成可以处理GET请求并返回字符串的Handler。
    @RequestMapping(value = "/hello", method = RequestMethod.GET)  //handler就相当于一个处理器
    public @ResponseBody String Hello() {
        return "Hello, SpringMVC.";
    }
}

当我们编写了一个Controller 之后,怎么样让Spring的DispatchServlet找到我们编写的Controller呢？依然是通过配置。在WEB-INF文件夹下,也就是web.xml同目录,创建一个MyFirstServlet-servlet.xml（注意命名）,注意,这个配置实际上就是Spring的配置,所以可以起包含Spring,servlet等字眼的名字,最好把你的工程名也加上.配置内容如下:
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

controller编写完,配置也配好之后,还是像之前那样将controller编译成class文件并放在WEB-INF/classes对应的目录下,同时由于引入了依赖，我们还需要将 Spring 以及之前提到的 common-logging jar 包拷贝到 WEB-INF/lib,然后启动tomcat,在页面调用 http://localhost:8080/MyFirstServlet/hello,因为你的controller中Hello方法已经用注解指定了调用的地址是/hello,方式是get: @RequestMapping(value = "/hello", method = RequestMethod.GET)
```
