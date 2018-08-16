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
    <url-pattern>/task/HelloForm</url-pattern> #也就是说/task/HelloForm这个地址,会在文件中会被映射成一个叫HelloForm的servlet
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

* String name =new String(request.getParameter("name").getBytes("ISO8859-1"),"UTF-8");一般代码中我们获取前台传来的参数都是这样获取的,但是这样
有可能会导致中文全成了乱码或者全是问号.说到这里那就有必要再说一下编码问题,getBytes()这个方法可以带参数,也可以不带参数,不带参数呢就是说我用平台默认的编码字符集将从电路里传过来的信息搞搞成一个字节数组,注意这里是指平台,意思是说,加入你前台传来了一串a编码的信息,但是现在整个环境用的编码是b,那getBytes把这串信息取过来之后就自动把这串信息转换成b编码了. 而带参数的getBytes("ISO8859-1")意思是不管当前平台用的是什么编码,也不管你传来的信息是什么编码,我都是用参数指定的编码格式把这串信息搞成一个字节数组.  那么现在问题来了,tomcat8默认的编码是utf8,一般平台指的就是你的后台服务器,也就是指你的tomcat,那么现在页面是部署在tomcat上,所以整个环境的默认编码就是utf-8,那么前台传过来的信息自然也就是utf-8,可是你用getBytes解码的时候却用的别的编码格式来解,这就肯定会出错.可能有的人会问了,解完码之后不是又用把信息转换成了utf-8的字符串了吗,这不就是又转回utf-8了吗,转过来转过去都是utf8啊,咋还会乱码呢? 所以说你还是不理解编码的本质, 你前台传来的信息是utf8,你解析自然得用utf8才能解出来,你用了别的编码来解析,但是别的编码里面不一定就有你这个码啊,假如你的utf8里面'傻'这个字是用1002这串数字来表示,但是在ISO里面可能人家根本没这个字,可能也没有1002这串码,那你的这串码最终不就被丢弃了吗,丢弃了之后你的码不就少了吗,原本是5555 1002 3699 8888,结果编成ISO之后码成了5555 3699 8888,因为new String的时候是不是就会少一个字,少了一个字那么有可能取码的时候位置就发生了错乱,然后整个信息都屌了,成了乱码或者问号.  所以说,你用什么码编的信息,你就用什么码来解,这样就不会错,平台现在默认就是utf8的编码,那你就自然没有必要再去转换一遍,String name =new String(request.getParameter("name"));直接用默认的码去取就行了. 很多人不理解编码的原理,所以用ISO去解结果自然就屌了. 还有啊,即便以后遇到需要转换编码的场景,也一定要养成习惯要用unicode去编和解,因为Unicode是最全的码,即便以后有人要用你的东西不知道你用的什么码,至少尝试一下还是有希望试出来的,不然你用个乱七八糟的码别人想试也试不出来.
* 获取http请求中信息的部分常用方法注意下:
```
Cookie[] getCookies()  返回一个数组，包含客户端发送该请求的所有的 Cookie 对象
HttpSession getSession() 返回与该请求关联的当前 session 会话，或者如果请求没有 session 会话，则创建一个。
Locale getLocale() 基于 Accept-Language 头，返回客户端接受内容的首选的区域设置
Object getAttribute(String name) 以对象形式返回已命名属性的值，如果没有给定名称的属性存在，则返回 null。
String getCharacterEncoding() 返回请求主体中使用的字符编码的名称
String getHeader(String name) 以字符串形式返回指定的请求头的值。
String getParameter(String name) 这个最常用,以字符串形式返回请求参数的值，或者如果参数不存在则返回 null。
String[] getParameterValues(String name) 返回一个字符串对象的数组，包含所有给定的请求参数的值，如果参数不存在则返回 null。
```
