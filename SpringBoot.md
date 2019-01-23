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




单体架构和微服务架构的区别: 单体架构所有的东西都是做在一个应用里面,最后打成war包,然后部署在环境的tomcat上,如果业务量大不够用了那就多放一些tomcat,每个
里面都部署一份应用. 微服务呢是把一个整体的应用拆分成很多功能模块,同样也是往环境上的Tomcat里面去复制这些模块,一个tomcat部署一个模块,但是我只是复制需要
用的模块,哪个模块用的多那我就在环境上多部署一些.所以就有很多dataImport,dataClean等,但是分析可能就那两三个.

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
