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
    <servlet-name>HelloForm</servlet-name>  # 这个HelloForm对应了com.runoob.test包下的一个类,类名叫HelloForm,且一定extends HttpServlet,对应
    <servlet-class>com.runoob.test.HelloForm</servlet-class> #的文件就是该包下的HelloForm.java. 该类就是你这个项目处理请求的servlet,里面写了
  </servlet>  #对这个请求作出的各种操作的功能逻辑代码.
  <servlet-mapping>
    <servlet-name>HelloForm</servlet-name>  # localhost:8080/task/HelloForm中的/task/HelloForm,指的就是这个mapping标签中的url-pattern,当
    <url-pattern>/task/HelloForm</url-pattern> #浏览器访问/task/HelloForm这个地址时,后台会让HelloForm这个servlet来处理这个请求,这个servlet必
  </servlet-mapping> # 须要被定义,上面<servlet>标签就是用来定义servlet的,也就是说,<servlet-mapping>和<servlet>标签并无先后顺序和位置顺序,只要
</web-app> # 你的mapping中用的servlet你定义了就行. mapping会根据你写的名称去找对应的servlet来处理这个请求.
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
* 获取http请求中信息的部分方法和往response中设置信息的部分方法注意下:
```
从request中获取信息:
Cookie[] getCookies()  返回一个数组，包含客户端发送该请求的所有的 Cookie 对象
HttpSession getSession() 返回与该请求关联的当前 session 会话，或者如果请求没有 session 会话，则创建一个。
Locale getLocale() 基于 Accept-Language 头，返回客户端接受内容的首选的区域设置
Object getAttribute(String name) 以对象形式返回已命名属性的值，如果没有给定名称的属性存在，则返回 null。
String getCharacterEncoding() 返回请求主体中使用的字符编码的名称
String getHeader(String name) 以字符串形式返回指定的请求头的值。
String getParameter(String name) 这个最常用,以字符串形式返回请求参数的值，或者如果参数不存在则返回 null。
String[] getParameterValues(String name) 返回一个字符串对象的数组，包含所有给定的请求参数的值，如果参数不存在则返回 null。
往response中设置信息:
void addCookie(Cookie cookie)  把指定的 cookie 添加到响应。
void addHeader(String name, String value) 添加一个带有给定的名称和值的响应报头。
void setCharacterEncoding(String charset)  设置被发送到客户端的响应的字符编码（MIME 字符集）例如，UTF-8。
void setHeader(String name, String value)  设置一个带有给定的名称和值的响应报头。
void setStatus(int sc)  为该响应设置状态码。
```
#### 一个例子: 当服务启动,通过地址调用了这个servlet之后页面会显示时间日期并且每隔5秒刷新一次时间日期.
```
public class Refresh extends HttpServlet {
      public void doGet(HttpServletRequest request,HttpServletResponse response) throws ServletException, IOException
      {
          response.setIntHeader("Refresh", 5);  // 设置刷新自动加载时间为 5 秒
          response.setContentType("text/html;charset=UTF-8"); // 设置响应内容类型
          Calendar cale = Calendar.getInstance();  //使用默认时区和语言环境获得一个日历    
          Date tasktime=cale.getTime();  //将Calendar类型转换成Date类型  
          SimpleDateFormat df=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");   //设置日期输出的格式  
          String nowTime = df.format(tasktime);     //格式化输出  
          
          PrintWriter out = response.getWriter();  // 把信息通过打印的方式传回页面
          String title = "这是一个展示时间没隔5秒自动刷新的例子";
          String docType = "<!DOCTYPE html>\n";
          out.println(docType +
            "<html>\n" +
            "<head><title>" + title + "</title></head>\n"+
            "<body bgcolor=\"#f0f0f0\">\n" +
            "<h1 align=\"center\">" + title + "</h1>\n" +
            "<p>当前时间是：" + nowTime + "</p>\n");
      }
}
<?xml version="1.0" encoding="UTF-8"?>  
<web-app>  
  <servlet>  
     <!-- 类名 -->  
    <servlet-name>Refresh</servlet-name>  
    <!-- 所在的包 -->  
    <servlet-class>com.runoob.test.Refresh</servlet-class>  
  </servlet>  
  <servlet-mapping>  
    <servlet-name>Refresh</servlet-name>  
    <!-- 访问的网址 -->  
    <url-pattern>/TomcatTest/Refresh</url-pattern>    //这里就是能调到这个servlet的部分地址,前面再拼上协议,ip和端口就是一个完整的地址
    </servlet-mapping>  
</web-app>
```
#### 过滤器
* 过滤器,或者叫做拦截器,实际上做的工作相当于一个门卫,可以对请求或响应(Request、Response)统一设置编码或者逻辑判断(通常是用户校验)，以此简化操作.如用户是否已经登陆、有没有权限访问该页面等等工作。它是随你的web应用启动而启动的，只初始化一次，以后就可以拦截相关请求，只有当你的web应用停止或重新部署的时候才销毁。无论是过滤器还是servlet,都是对目标而言的,比如当前这个配置是拦截所有请求,那我如果改成<url-pattern>/index.jsp<url-pattern>,那么就是只拦截发到index.jsp这个页面的请求.servlet也是一样.
```
<?xml version="1.0" encoding="UTF-8"?>  
<web-app>  
<filter>
  <filter-name>LogFilter</filter-name>
  <filter-class>com.runoob.test.LogFilter</filter-class>  # LogFilter拦截器实际上就是test包下的LogFilter.java类
  <init-param>
    <param-name>Site</param-name>
    <param-value>在拦截器初始化的时候可以通过指定方法来获取到这个标签中的值,动态配置可以少写重复代码</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>LogFilter</filter-name>  # 在用filter 的mapping之前也是要先在上面定义filter
  <url-pattern>/*</url-pattern>  #所有请求都要先给LogFilter这个拦截类先处理一遍
</filter-mapping>  # 注意了!!!!! servlet的映射没有顺序,但是拦截器有顺序,假设你配置了多个拦截器,会按照配置从上到下去执行.

这里是一个servlet的配置,省略.......... 虽然过滤器和servlet之间并无顺序,但是约定俗成先配过滤器后配servlet

可以看出来web.xml其实也没多少东西,主要就这几个 <filter> <filter-mapping> <servlet> <servlet-mapping>,当然还有异常,
顺便在这里举个异常的例子:
<servlet>
        <servlet-name>ErrorHandler</servlet-name>
        <servlet-class>com.runoob.test.ErrorHandler</servlet-class>  //事先写好处理异常的servlet
</servlet>
<!-- servlet mappings -->
<servlet-mapping>
        <servlet-name>ErrorHandler</servlet-name>
        <url-pattern>/TomcatTest/ErrorHandler</url-pattern>  //当正常请求报错之后请求会被重定向到这个异常请求
</servlet-mapping>       //这个异常请求就会交给ErrorHandler这个servlet去处理
<error-page>
    <error-code>404</error-code>
    <location>/TomcatTest/ErrorHandler</location>  
</error-page>
<error-page>
    <exception-type>java.lang.Throwable</exception-type >  //这里表示无论出了任何异常都是用ErrorHandler这个servlet去处理
    <location>/ErrorHandler</location>   //你可以在这里根据不同的异常指定不同的servlet去处理.
</error-page>


-------------下面是个过滤器----------
public class LogFilter implements Filter   // 过滤器要实现javax.servlet.Filter 接口,里面就三个方法
{
    public void  init(FilterConfig config) throws ServletException {
        String site = config.getInitParameter("Site");  // 这个就是在配置中你配的那个参数,可以通过参数名称取到值      
        System.out.println("网站名称: " + site);  // 用参数的代码. 一般参数都是用来初始化的 
    }
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain)
        throws IOException, ServletException {     // doFilter 里面就是你拦截了请求之后要干的事情
        //http://localhost:8080/servlet_demo/helloword?name=123
        String name = req.getParameter("name");  // 比如你在这里取出了地址中的名称然后去校验
        if("123".equals(name)){
            // 把请求传回过滤链
            chain.doFilter(req, resp); //这段话其实就是放行请求,并不是说又去调用了个别的doFilter,调这个就表示请求继续往下走
        }else{
            resp.setContentType("text/html;charset=GBK"); //设置返回内容类型
            PrintWriter out = resp.getWriter();  // 验证失败则返回页面新的失败信息
            out.print("<b>name不正确，请求被拦截，不能访问web资源</b>");
            System.out.println("name不正确，请求被拦截，不能访问web资源");
    }
    public void destroy( ){
        // 在 Filter 实例被 Web 容器从服务移除之前调用 ,比如一些资源保存之类的善后工作.
    }
}
```
#### cookie
* Cookie 是存储在**客户端计算机**上的**文本文件**，用来保存各种跟踪信息.在浏览器首次访问服务器脚时,服务器会向该浏览器发送一组 Cookie。例如：姓名、年龄或识别号码等,浏览器将这些信息存储在本地计算机上，当下一次浏览器向 Web 服务器发送任何请求时，浏览器会把这些 Cookie 信息发送到服务器，服务器将使用这些信息来识别用户。看一个在doGet方法中处理cookie的例子:
```
public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    {
        // 如果操作的cookie涉及到中文那么需要进行编码和转码
        String   str   =   java.net.URLEncoder.encode("中文"，"UTF-8");            //编码
        String   str   =   java.net.URLDecoder.decode("编码后的字符串","UTF-8");   // 解码

        // 以下是在response中设置一个cookie的例子,注意,创建cookie的键值对时,无论是key还是值，都不应该包含特殊字符
        Cookie name = new Cookie("name", URLEncoder.encode(request.getParameter("name"), "UTF-8")); // 创建名字 Cookie
        Cookie url = new Cookie("url", request.getParameter("url")); //创建地址cookie
        name.setMaxAge(60*60*24);  // 为两个 Cookie 设置过期日期为 24 小时后. 
        url.setMaxAge(60*60*24);  //把过期时间设置成0相当于删除了该cookie
        response.addCookie( name );  
        response.addCookie( url );   // 把cookie添加到响应中
        
        // 这个是设置相应当中的内容格式和编码
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        String title = "一个设置cookie的例子";
        String docType = "<!DOCTYPE html>\n";
        out.println(docType +
                "<html>\n" +
                "<head><title>" + title + "</title></head>\n" +
                "<body bgcolor=\"#f0f0f0\">\n" +
                "<h1 align=\"center\">" + title + "</h1>\n" +
                "<ul>\n" +
                "  <li><b>站点名：</b>："
                + request.getParameter("name") + "\n</li>" +
                "  <li><b>站点 URL：</b>："
                + request.getParameter("url") + "\n</li>" +
                "</ul>\n" +
                "</body></html>");
                
        // 通过HttpServletRequest 的 getCookies( ) 方法得到一个Cookie数组。然后循环遍历数组，并使用 getName() 和 getValue()
           方法来访问每个 cookie 和关联的值。以下是在返回response之前获取cookie并进行一些操作的例子:
        Cookie cookie = null;  
        Cookie[] cookies = request.getCookies(); // 获取与该域相关的 Cookie 的数组
         
        if( cookies != null ){
            for (int i = 0; i < cookies.length; i++){
               cookie = cookies[i];
               if((cookie.getName( )).compareTo("name") == 0 ){
                    cookie.setMaxAge(0); //执行操作cookie的逻辑
                    .............                    
               }
            }  
         }       
    }
```
#### session
* HTTP是一种"无状态"协议，这意味着客户端每次浏览服务器的资源，都会单独打开一个和服务器的连接，用完之后服务器都不会保留客户端的任何记录.cookie和session都是用来保持客户端和服务端回话的技术.Servlet 容器使用HttpSession 接口来创建一个 HTTP 客户端和 HTTP 服务器之间的 session 会话。会话持续一个指定的时间段，跨多个连接或页面请求(注意到没,这就是回话的一个很重要作用,假如你登陆了一个网站浏览了一个页面,现在你要浏览另一个同网站的网页,如果不使用回话保持技术,那你这个回话连接在上一个回话完了就断了,你得重新新建一个连接,然后你就又得登录一遍.......真是可怕!!)。一个例子:
```
public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    {
        HttpSession session = request.getSession(true); // 参数为true表示,如果当前不存在session会话，则创建一个session对象
        Date createTime = new Date(session.getCreationTime()); // 获取 session 创建时间
        Date lastAccessTime = new Date(session.getLastAccessedTime()); // 获取该网页的最后一次访问时间        
        SimpleDateFormat df=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");  //设置日期输出的格式
    
        String title = "一个session的例子";
        Integer visitCount = new Integer(0);
        String visitCountKey = new String("visitCount");
        String userIDKey = new String("userID");
        String userID = new String("Runoob");
    
        if (session.isNew()){         // isNew() 方法来检查该 session 会话是否已点击过相同页面
            title = "一个session的例子";
            session.setAttribute(userIDKey, userID);
        } else {
             visitCount = (Integer)session.getAttribute(visitCountKey);
             visitCount = visitCount + 1;
             userID = (String)session.getAttribute(userIDKey);
        }
        session.setAttribute(visitCountKey,  visitCount);
    
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        String docType = "<!DOCTYPE html>\n";
        out.println(docType + ......页面输出的内容省略.......)
    }
    
  <session-config>
    <session-timeout>15</session-timeout>  # 可以在web.xml中设置session回话的过期时间,这个是以分钟为单位
  </session-config>  # 通过Servlet的getMaxInactiveInterval()方法会返回session会话的超时时间，以秒为单位。当前这个例子返回 900。
   # 当然也可以通过 invalidate() 方法来丢弃整个 session 会话。或者通过setMaxInactiveInterval(int interval) 方法设置会话超时。
```
#### 一个上传和下载的servlet的例子
```
@WebServlet("/UploadServlet")
public class UploadServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    private static final String UPLOAD_DIRECTORY = "D:\\upload";   // 上传文件存储目录
    private static final int MEMORY_THRESHOLD   = 1024 * 1024 * 3;  // 设置内存临界值 3MB
    private static final int MAX_FILE_SIZE      = 1024 * 1024 * 40; // 上传文件最大限制 40MB
    private static final int MAX_REQUEST_SIZE   = 1024 * 1024 * 50; // 整个请求最大限制 50MB

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        if (!ServletFileUpload.isMultipartContent(request)) {  
            PrintWriter writer = response.getWriter();    // 检测是否为多媒体上传,不是的话进入这个分支,返回信息
            writer.println("Error: 表单必须包含 enctype=multipart/form-data");
            writer.flush();
            return;
        }
        DiskFileItemFactory factory = new DiskFileItemFactory(); // 构造磁盘工厂对象      
        factory.setSizeThreshold(MEMORY_THRESHOLD); // 为磁工厂对象设置内存临界值 - 超过后将产生临时文件并存储于临时目录中
        factory.setRepository(new File(System.getProperty("java.io.tmpdir"))); // 设置临时存储目录
        
        ServletFileUpload upload = new ServletFileUpload(factory);  // 创建上传servlet对象并将磁盘工厂赋给该对象 
        upload.setFileSizeMax(MAX_FILE_SIZE); // 设置最大文件上传值
        upload.setSizeMax(MAX_REQUEST_SIZE); // 设置最大请求值 (包含文件和表单数据)
        upload.setHeaderEncoding("UTF-8");  // 这只请求头的编码

        // 构造临时路径来存储上传的文件 // 这个路径取的是当前工程的路径
        String uploadPath = request.getServletContext().getRealPath("./") + File.separator + UPLOAD_DIRECTORY;        
        File uploadDir = new File(uploadPath); // 如果目录不存在则创建
        if (!uploadDir.exists()) {
            uploadDir.mkdir();
        }
        try {    // 解析请求的内容提取文件数据
            @SuppressWarnings("unchecked")
            List<FileItem> formItems = upload.parseRequest(request);
            if (formItems != null && formItems.size() > 0) {
                for (FileItem item : formItems) {
                    if (!item.isFormField()) {
                        String fileName = new File(item.getName()).getName();
                        String filePath = uploadPath + File.separator + fileName;
                        File storeFile = new File(filePath);
                        item.write(storeFile); // 保存文件到硬盘
                        request.setAttribute("message", "文件上传成功!");
                    }
                }
            }
        } catch (Exception ex) {
            request.setAttribute("message", "错误信息: " + ex.getMessage());
        }
        // 跳转到上传成功的uploadSuccess.jsp
        request.getServletContext().getRequestDispatcher("/uploadSuccess.jsp").forward(request, response);
    }
}
######另一个版本
public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    String filename=request.getParameter("name"); //获取要下载的文件名
    filename=new String(filename.getBytes("iso-8859-1"),"utf-8"); //防止读取name名乱码
    //设置文件MIME类型  也就是给文件指定默认程序, 当该扩展名文件被访问的时候，浏览器会自动使用指定应用程序来打开。
    response.setContentType(getServletContext().getMimeType(filename)); 
    //服务端向客户端游览器发送文件时，如果是浏览器支持的文件类型，一般会默认使用浏览器打开，比如txt、jpg等.如果需要提示用户保存，
    就要利用Content-Disposition进行一下处理. 就是下面这个,用来弹框问你是打开还是保存.
    response.setHeader("Content-Disposition", "attachment;filename="+filename);
    ServletContext context=this.getServletContext(); //获取要下载的文件绝对路径，我的文件都放到WebRoot/download目录下
    String fullFileName=context.getRealPath("/download/"+filename);
    
    InputStream is=new FileInputStream(fullFileName); //输入流为项目文件，输出流指向浏览器
    ServletOutputStream os =response.getOutputStream();
    
    int len=-1;
    byte[] b=new byte[1024];  // 这里相当于设置了一个1024的缓冲区
    while((len=is.read(b))!=-1){  // 只要缓冲区还有东西就往os中写
        os.write(b,0,len);
    }
    is.close();
    os.close()
}
```
##### 一点小总结
* 从最开始的单个页面的servlet,到后面在页面上添加其他的功能,再到最后的上传下载,或者说后续的发邮件,听歌看电影等等,可以发现,页面的每个操作都对应着后台的一个servlet来处理,只要你后台写好了处理对应操作的servlet,然后把servlet在web.xml中注册好,前台按要求调用就行了.但是这样也会有麻烦,如果页面要做的功能特别多的话,那你后台的servlet肯定也分门别类地要配置多个才行,这样的话肯定繁琐不堪.所以这也是spring这种框架诞生的一个原因,把所有的请求都交给spring的一个总的servlet去处理,然后该servlet再根据请求去找适配器,并最终找到能处理这个请求的处理器.这样的话原始的结构和框架的运作机制大致上就很清晰了.
* 由于一次网页点击就是一个新的session会话,所以有时候我们可以通过计数的方式来统计某个网页被点击了多少次.在servlet类中定义一个全局变量,在初始化方法中赋值为0,然后每次处理一次doGet或者doPost方法计数就加1,这样当你不断刷新页面这个数字就不断增加,在servlet的destory方法中你可以把这个计数值写入文件或者保存到数据库中去. 同样的道理可以用来对整个网站的访问次数进行统计,只需要搞一个过滤器,把有的请求都交给过滤器去处理,还是定义全局变量,然后每次调用doFilter的时候计数加1,这样就可以对网站的整体访问进行计数.
* Apache Tomcat 是一个开源软件，实现了对 Java Servlet 和 JSP（JavaServer Pages）技术的支持。相当于一个servlet容器.
* 国际化的一些东西:不同的地区肯定有差异,但是无论啥差异,都可以先从请求中获取到当地的信息,然后根据当地信息去格式化和处理诸如日期,数字,货币,语言等等的各种信息:
```
Locale locale = request.getLocale();
String language = locale.getLanguage();
String country = locale.getCountry();

response.setHeader("Content-Language", "es"); //设置语言
String date = DateFormat.getDateTimeInstance(DateFormat.FULL, DateFormat.SHORT, locale).format(new Date( )); 格式化日期
```
