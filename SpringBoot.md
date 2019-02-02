#### springboot整合maven
- maven官网下载maven的二进制包,解压到本地. 配置环境变量.
- 修改maven的conf文件夹下的settings文件.在setting标签里有个profiles标签,放开里面关于jdk配置的标签并将jdk配置成1.8
- 打开idea,在主界面(还没进入工程的那个界面),configure-settings-build选项-build tools(构建工具),选择你自己装的按个maven.勾选下面的两个框并更改,
配置文件选你的maven的conf里面那个settings,在conf同级建个repository目录,然后仓库选你建的这个. 其他不动,保存.
- 在主界面Create new project,选择maven工程,选好jdk,然后next. 指定groupID:com.mozun; Projextid:springboot-01-hello,然后next.  指定工程名称,然后
指定该工程的路径F:\workspace_idea\SpringbootHello,注意idea是单工程理念,所以你要把工程放到哪个目录下你要自己改路径,别直接确认,workspace_idea就直接
成了你当前这个工程的路径了.
- 工程打开之后界面会出现一个弹窗,说maven need import,你就选enable auto import(启用自动导入),这样的话每次你更改了pom.xml文件加了依赖的话,idea就会
自动把这个依赖导入.
- springboot是不需要单下载安装包的,它可以直接通过maven的标签配置来引入.在springboot的官网每个springboot版本都有自己reference.doc文档,打开里面
有pom.xml的配置示例,复制然后粘贴到项目pom.xml,这时maven会自动识别并导入相应的依赖. 已经导入的依赖字是黑色的,没有导入的字是红色,而且idea底部会有提示
告诉你正在下载还是无法导入. 外部依赖都在你的工程External Libraries里面,springboot有一大堆,因为其本身也需要依赖别的东西.
- 新建java类,同时可以指定包名:  com.mozun.test.MainApplication,这时就会自动在目录下创建com.mozun.test包


#### Springboot的使用
- 用@SpringBootApplication注解来把当前创建的类标注成一个springboot的主程序.这个程序就是启动整个工程的入口. 后续springboot的自动扫描也是基于这个
注解.被注解的这个类所在的包同路径和子路径下的所有组件都会被springboot扫描进去,以全路径的格式放到一个String数组里面返回,并被添加到框架的容器中,然后
框架会根据注解去使用相应的组件. 实现这整个功能的是springboot的自动配置类,这些自动配置类是springboot在启动的时候从类路径下的
META-INF/spring.factories中enableAutoconfigure中指定的.我们在springboot里面什么配置都没有做实际上是boot都已经帮我们全部配好了.
- 在刚刚那个test包下新建controller.MyController类,就会自动创建test包下创建controller包并在controller包下创建MyController类.这时候的目录结构是:
主程序MainApplication在test包下,而MyController类就在controller包下.
- 编写好相关的类之后可以直接运行MainApplication类,整个项目就运行起来了. 然后将在idea右侧找到Maven Project选项,找到你的项目,选中LifeCycle-package
然后点击三角图标将项目打成一个jar包.此时jar包是存放在你的项目target目录下.将该jar包拷贝到你环境的任意目录下,使用: java -jar xxxx.jar命令来运行你的
这个包,这个包就被部署成了一个服务.

#### IDEA快速构建springboot应用
- Create New Project--Springboot initializer--选择jdk,指定maven组id--选择模块--指定项目名称-OK
- 快速构建会生成主程序,我们只需要关注业务逻辑即可.在指定maven组id和小组id那个页面最下面的 '组id+小组id'实际上就是你的当前项目的主包名称,而主包名称
上面那个名称实际上最后会被用到主程序里,那个名称可以改不过最好跟小组id名称保持一致,因为最后主程序名称就会叫'小组ID+Application.java'
- 默认还会生成一个resources文件夹里面有三个文件夹:
  - static: 文件夹,放静态资源. 比如css,js,image等
  - templates: 文件夹,放模板页面.springboot内嵌的tomcat不支持jsp,但是我们可以通过使用模板引擎来支持,比如freemarker, thymeleaf等
  - application.propreties: 文件,springboot的默认配置文件.可直接用键值对的方式来改配置.比如server.port=8081就是改tomcat的端口
  - application.yml(yaml): 另外一种配置文件(需要手动创建),也是springboot常用的全局配置文件,和上面那种写法不同. 在写yml配置文件的时候可能需要引入
  一个yam配置自动提示的依赖,可以直接在pom.xml里配上,这样你在写配置的时候能动态提示你要写啥属性.

```
@component //这个注解的意思是把普通的pojo实体类实例化到容器当中.相当于是一个组件注解.@Controller,@service,@dao(Repository)这些都是组件注解,
         // 由于某个组件没有更好的划分种类,所以就用这个注解来标注它们. 
@ConfigurerationProperties(prefix="person") //用这两个注解就可以将配置应用到对应的类上,比如映射到Person类上.这个注解支持松散绑定,就是说你的变量
用lastName或者last-name都行. 但是他不支持表达式,就是不能在配置中给变量赋值的时候用表达式计算出来的值. 而@value这个标签是可以的.还有个区别,那
就是@从figureration...支持复杂类型注入,比如对象,list,map,但是@value不行,它只支持注入简单类型. 还有个注意点,@config....它只能加载全局配置,你要是想
加载自己的配置需要用@propertiesSource这个注解,把你的配置放在全局配置同级,然后通过key-value来应用

// ymal文件中可配字符串,map,list,对象等数据结构,配置好之后可以通过注解来映射到对应的类上
person:           属性和值之间必须要有一个空格. 字符串不需要用引号,如果非要用,那么双引号禁止转义,单引号可以转义
    port: 9999    //层级关系比如server和port之间也有空格但是几个无所谓
    path: xxxx    //同级关系要对齐.  属性大小写敏感. 对象和
// 用properties配置文件也可以实现相同的功能,一般写成这种格式.这种配置方式有可能引起中文乱码,需要在settings中搜索file code选项进行更该编码
  person.port=xxx
  person.path=xxx
```

```
@ResponseBody  //事实上我们可以把这个注解写在类上,表示该类所有的方法返回数据都直接写给浏览器
@Controller
@RestController   //我们也可以直接用这个注解来代替@ResponseBody和@Controller两个,因为点进@RestController可以发现其实它就是上面两个注解的合体
public class MyController {
    @ResponseBody   //这个注解的作用意思是:当前这个方法的返回数据直接写给浏览器,如果是对象则转成json格式
    @RequestMapping("/hello")  //这个注解就是映射请求
    public String SayHello(){
        return "hello nimabi";
    }
}
```

#### 主要的配置
```
    // 项目描述信息:
    <groupId>com.mozun</groupId>
    <artifactId>springboot-01-hello</artifactId>
    <version>1.0-SNAPSHOT</version>

    // springboot的版本和依赖导入
    <parent>
        <groupId>org.springframework.boot</groupId>  //这个是springboot的一个父依赖,它管理了springboot几乎所有要用到的依赖的版本,比如我们
        <artifactId>spring-boot-starter-parent</artifactId> //引入一个mysql依赖,但是不需要指定其版本,就是这个玩意帮我们自动适配的.
        <version>1.5.19.RELEASE</version>   // 这个是个版本仲裁
    </parent>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>  //spring-boot-starter这个玩意是springboot的一个启动器,它有很多的启动器,
            <artifactId>spring-boot-starter-web</artifactId>  //你用那个功能我就给你提供哪个功能个的启动器.这个是web启动器,所有运行web项目
        </dependency>  //的模块和依赖都由这个启动器给你配好了,你只要把这个启动器依赖在pom.xml中就行.版本还是由版本仲裁来自动适配的.
    </dependencies>  //还有其他更多的启动器比在官方文档中都可以看到,用哪一个就引入哪个就行了.
    
    <build>
        // 将项目打成jar包的插件,一般这个插件在引入springboot的时候会推荐一并让你引入
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

- 单体架构和微服务架构的区别: 单体架构所有的东西都是做在一个应用里面,最后打成war包,然后部署在环境的tomcat上,如果业务量大不够用了那就多放一些tomcat,
每个里面都部署一份应用. 微服务呢是把一个整体的应用拆分成很多功能模块,同样也是往环境上的Tomcat里面去复制这些模块,一个tomcat部署一个模块,但是我只是复
制需要用的模块,哪个模块用的多那我就在环境上多部署一些.所以就有很多dataImport,dataClean等,但是分析可能就那两三个.
- spring.propertie.active = 环境1(2,3,4,5) //用来激活使用哪个环境配置
- 配置文件根据放的地方不同有优先级.高优先级会覆盖低优先级,如果高级里面没有某个配置但是低级有则会从低级里面去用.也就是配置互补.可以在项目已经打好包
在运行时通过参数来指定添加额外配置.这样就不需要重新再改项目主配置重新再打包. 命令行还可以直接指定参数比如:  --server.port=2298 ;在加载配置的时候会
先加载和jar包放在相同路径下的config.properties.然后才是打在jar包里面的
- 日志有级别. trace<debug<info<warn<error. 日志级别可以自由设置,默认级别是输出后面三个的日志. 
```
logging.level.包名称=trace(debug/error...)  #修改日志级别
logging.file=abc.log  #将日志输出到当前工程的abc.log文件中,这个日志文件在当前项目的target文件夹中;也可以直接给定路径文件比如,C:\\abc.log
logging.path=C:\\   #file是指定一个文件,而path只是指定路径,不带文件.springboot会默认给我们创建一个spring.long文件.
```
- 工程的java和resources文件夹都是类路径根文件夹,如果说工程的类路径或者根目录就是指这两个目录.放在这两个目录下指定文件夹下的资源都是静态资源.当你没有
配置明确的映射关系时,想要访问某个静态资源默认就是去他们下的文件夹下去找的. 静态资源的默认路径可以通过配置文件来修改.
- 如果修改了template下面的静态资源,重新刷新浏览器页面后没生效.可以先将模板引擎的缓存设置成false,spring.thymeleaf.chach=false,然后再按ctrl+f9重新
编译修改过的文件.
- springboot的内置servlet容器(也就是内置tomcat)是在springboot的ioc容器创建的创建的. 如果用的不是内置tomcat而是外部服务器,那么是先启动服务器
然后才创建ioc容器.内置的通tomcat不支持jsp,外部的支持;
-创建勾选了各种依赖的项目>在resources下创建项目主配置文件>配置所需模块的基本配置>在springboot主程序所在的包下创建各个功能模块文件夹>在各个功能模块
文件夹下写具体的功能类.
- springboot定制实际上就是自己去写一些相关的配置类,这些配置类会实现springboot提供的一些接口,你在配置类中更改你想要用的属性,最后把写好的那个属性对象
返回,并把那个属性对象用@bean注解添加到ioc容器中,springboot就会自动去适配你写的那个配置类.
