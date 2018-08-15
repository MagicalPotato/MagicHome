#### 前后台逻辑串联
* servlet是一个小服务器比如tomcat的核心.在tomcat上会放很多静态的html,css,java等文件,在浏览器输入一个地址之后,这个地址会对应一个html文件,比如这个地址: 
localhost:8080/xxxTomcat/xxx.html, 这个地址的意思是你本地启了一个tomcat服务,然后在这个tomcat的WebContent-web-info文件夹下放了一个叫xxx.html的文件,
当页面输入这个地址之后,就会去你的这个tomcat的WebContent-web-info目录下去找这个对应的xxx.html文件,这个文件不一定就是在web-info根目录下,也有可能你另外
新建了一个目录放着xxx.html,但是tomcat也是能够找到的,只要在web-info文件夹下就行了.  如果你这个xxx.html文件写的很屌,那么展示在页面上的话可能会涉及到各种
按钮点击啊或者选择之类的操作,那么这些操作会都会对应相应的后台接口,当你点击了某个按钮,这些数据就会被封装到地址中一并传送给后台,后台拿到数据后,会去实现自己
的逻辑,实现完了逻辑会通过流打印或者别的方式返回给前台需要的数据,前台收到之后展示这些信息.这样一个古老的标准的前后台交互的流程就结束了.
* localhost:8080/HelloForm/index.html  ,假设访问了这个地址之后会输入一些数据传给后台,那么后台肯定会有一个类来处理这个请求,类名叫HelloForm.java,这个
类一定extends HttpServlet或者implements Servlet, 注意:类的名称叫什么和浏览器地址里输入什么都是根据你的业务来定的,加入你的业务是处理数钱,那么你的类名
就叫 countMoney.java,那么你在浏览器的地址里就要相应 的输入localhost:8080/countMoney/index.html. 还有个问题,既然你的类叫HelloForm.java,你的请求也是
让这个类去处理的,为啥地址不写成这样呢localhost:8080/HelloForm.java/index.html,事实上本来就应该这样写,但是你就算写了一个类名也不行啊,浏览器咋知道你的
这个类放在哪个路径下,所以要想用最原生态的写法,你得把这个类的详细地址也写上:localhost:8080/com.runoob.test.HelloForm.java/index.html,也就是说你得把
这个类在哪个包下也得写明白,并且这个包在tomcat的src文件夹下(为啥非要放在src下呢,这是一种约定俗成的规矩,试想,你开发了东西你想放在aaa文件夹下,别人开发了又
想放在bbb文件夹下,世界上那么多人,大家一人一个文件夹,那最终每个人都只能懂自己弄的东西,别人的就用不了了,这不尼玛吗,所以最终经过Apache那些先驱的规定,大家都
统一放在src下面).回到问题,浏览器的地址栏就那么长,你一个包就要占去一大截,那别的东西肯定就少了很多,更不美观,所以为了简练,先驱们就又约定,给每个tomcat中的
每个项目都给定了一个配置文件web.xml,这个配置文件中会配置这个项目的一些映射信息,就是一些对应关系的信息,当浏览器中输入了地址之后,tomcat就会去这个web.xml
中找对应的信息,比如这个地址localhost:8080/task/hello.html,他的意思是去访问task这个微服务下的hello.html这个资源,访问到之后如果你做了一些操作并且这些
操作会调用后台,假设调用后地址变成了localhost:8080/task/HelloForm,其中/task/HelloForm也是代表资源,且代表的是能处理这个请求的servlet,也即是最终处理
请求的HelloForm.java这个类,看下面这个配置文件:
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app>
  <servlet>
    <servlet-name>HelloForm</servlet-name>   # 这个HelloForm对应了com.runoob.test包下的一个类,类名叫HelloForm,且一定extends HttpServlet,对应
    <servlet-class>com.runoob.test.HelloForm</servlet-class> #的文件就是该包下的HelloForm.java. 这个类就是你这个项目处理请求的servlet,里面写了
  </servlet>  #对这个请求作出的各种操作的功能逻辑代码.
  <servlet-mapping>
    <servlet-name>HelloForm</servlet-name>  # 而localhost:8080/task/HelloForm中的/task/HelloForm,指的就是mapping标签中的/task/HelloForm
    <url-pattern>/task/HelloForm</url-pattern> #也就是说地址里的这一串东西/task/HelloForm,会在文件中会被映射成一个叫HelloForm的servlet
  </servlet-mapping> # 而这个叫HelloForm的servlet就是上面com.runoob.test包下的那个HelloForm.java,
</web-app>
```
* 随着项目的不断发展,你页面要调用的借口肯定也越来越多,那么这么多的接口你也都得配置到web.xml这个文件中才行,不然页面还是个找不到,如果你有一千个方法
那么你的配置得配到多长那真是难以想象,而且改起来也是个麻烦事.为了解决这种麻烦事,框架被发明出来了,页面所有的请求都统一交给框架去处理了,web.xml也清净了,
配的东西也少了,皆大欢喜. 但是你把东西丢给框架了那框架也得想办法定位资源啊,总不能继续用老办法一个一个配吧,想来想去,最终注解这个东西就诞生了,不得不佩服
发明框架的这个人,确实很屌,通过扫描你项目里的代码来找注解,根据注解来定位资源这就是框架的妙处.所以后来啊,我们能在代码里看到很多类似
@WebServlet("/HelloForm")的东西,这玩意就是注解:localhost:8080/task/HelloForm,通过框架的各种骚操作,你地址里的/HelloForm就被准确的定位到下面这个
类上面去了.
```
@WebServlet("/HelloForm")
public class HelloForm extends HttpServlet 
{
    private static final long serialVersionUID = 1L;
    后续省略...................
}
```

