#### 一些javaWeb中需要注意的点
1. J2EE, JavaEE 以及 JEE 其实都是同一个东西，只不过由于历史原因出现了若干名称,这些都可统一称之为JavaEE.JavaEE是对JavaSE(标准版) 的扩展，加入了面向企业开发（实际上就是网络和 Web相关）的支持，包括 Servlet，WebSocket，EL，EJB 等。简而言之,JavaEE 就是JavaSE + 更多的jar包，这些jar包命名以javax开头，例如 javax.servlet, javax.websocket 等。
2. Servlet是JavaEE的诸多组件的其中之一,它只是一套用于处理HTTP请求的API标准,也就是说只是一组接口,真正的实现是靠开发者去实现的.
3. Servlet的运行需要容器的支持,但是JavaEE本身没有提供Servlet Container,这样做是有好处的,因为容器就是环境,servlet不和某个确定的容器相关,意味着Servlet可以跑在任何一个Container当中，避免了对Runtime环境的依赖。比较常见的支持Servlet Container的外部Server软件有 Apache Tomcat，Glassfish，JBoss，Jetty 等等。
4. EJB全称Enterprise JavaBean，它也是JavaEE的一个组件，面向更加复杂的企业业务开发。对于Web来说,EJB不是必须。且和 Servlet 类似，运行EJB也需要专门的 EJB Container。但并不是所有的外部Server软件都支持EJB。Tomcat就不支持EJB，而JBoss提供了对EJB的支持。**后续了解下**
5. Spring是一个非常庞大的框架，包括SpringMVC，SpringBoot以及SpringCloud等用于Web开发的工具.Spring不需要完整的JavaEE内容,仅仅依赖最基础的 Servlet，也不需要EJB Container，只用普通的Servlet Container就可以运行。Spring和JavaEE不是一个层面上的东西。Spring 仅依赖了JavaEE的API 标准，最新版的Spring甚至进一步隔绝了JavaEE的API。开发者可以完全不关心Servlet或者JavaEE等概念，也可以进行Java Web开发。Spring不是唯一的Java Web框架，还有Structs,Spark等
