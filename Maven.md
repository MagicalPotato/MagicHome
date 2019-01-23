### Maven
* maven的安装和使用
  1. 从官网下载maven的二进制zip包,也就是binary包,然后将其解压在本地D盘或者别的盘下,例如  D:\apache-maven-3.5.4 
  2. 像配置jdk那样配置maven的环境变量. 新建系统变量MAVEN_HOME(名字就叫这个,别手欠搞个什么别的花样叫别的名字,用别的名字也可以,但是有一大堆东西要改,
而且我也不会,所以你还是直接叫这个名字),变量值就是刚你解压那个路径. 然后修改已经存在的Path变量,新增 %MAVEN_HOME%\bin;(别忘了后面的分号)
  3. 随便在 哪里打开cmd黑框,输入  mvn -v  命令,看看成功没
  4. 安装了maven之后,你想要在eclipse中用maven来管理你的工程,那就要把你刚安装的maven引入你的eclipse  Window--preferences--Maven--installations
点击add,把刚你装的那个maven添加上,只选择路径,然后给起个名字,名字最好就是你的maven的文件夹名称,然后finish,然后aplay就行了.
  5. 新建一个maven工程  在Project Explorer区域右键,选择new一个Maven Project,注意new的是一个Maven Project,不是默认的Projext,也不是java Project.在
新建Maven Project工程的时候有一个 create a simple project选项.如果勾选了,那么maven会给你的项目搭一个默认的框架,当然这个框架不是一个面面俱到的框架
只是一个基本的雏形框架,给你节省了一些麻烦. 如果不勾选,那么就是创建一个纯净的项目,什么乱七八糟的框架配置啥的都需要你手动自己去搞. 还有一个选择的地方要注意,如果是web工程,打包的方式选择war.因为web工程一般要部署到tomcat上运行,而tomcat编译web用的是war包.
  6. 更改工程目录结构. 你用maven虽然新建了一个工程,但是这个工程并不是一个web工程,所以目录结构也不是一个web工程的目录结构,所以你要将这个工程手动转换成一个web工程.选中工程右键,properties--project facets,弹出的对话框默认给你勾选了java和java script选项,你需要看看Dynamic web project那个选项有没有默认勾选,如果没有默认勾选你就勾选上.这里要注意选择Dynamic的版本为3.0,java的版本选择1.6以上,对话框的右半部分写了版本的限制可以自己看下.而且选了3.0的web可以使用1.7以上的tomcat,3.0以下的不支持1.7以上的tomcat.至于为啥不支持,我也不知道,这就如同坐火车和开飞机,时代进步了,大家都会开飞机当然就不会去坐火车所以有一些铁路就被拆了,后来外星人来了把我们所有的飞机没收了大家没办法只能先暂时开火车,但是发现有的铁路没了所以火车没法开了,版本冲突大概也就是这个道理. 选择完之后应用就是了,这时候你的目录结构就会发生一些变化成了一个动态web的工程结构.

* 一些注意事项
  1. maven工程建起来之后根据使用需要一般要修改pom.xml,默认的pom.xml只有几行配置,主要配置你的工程的groupid和artifactId,版本还有打包方式.后续可以根据需要对pom.xml进行修改.你会发现eclipse窗口打开的pom.xml左边的名称左边还有一大堆可以点击的分页,其中一个是Effective Pom,就在pom.xml的左边.这个Effective Pom是一个maven默认的pom.xml配置,你新加的配置和这个默认配置加起来就构成了一个maven工程的所有配置.你也可以把这个Effective pom当成是一个模板,当你配置你的项目的pom.xml时可以参考这个模板,比如说你要在你的pom.xml中加一个插件,那你就参考Effective Pom中人家的插件咋加的,都用了哪些标签,标签的嵌套关系是怎么样的,参考好了之后你就照着样子在你的pom.xml中配置就行了.很多人在配置pom.xml的时候直接从网上找个配置拷贝过来就粘上了发现明明标签没有错误,但是左边就是会爆小红叉.原因就是因为网上那些弱智根本自己也不知道啥原因,像个傻逼一样只知道从别人哪里拷贝,没找过原因,拷来拷去还是不行.网上那些傻逼在那里装模作样一副老学究的样子,但实际上不过是牵强附会,真是误人子弟.所以建议大家以后无论看博客还是文章,发现那些不讲原理只让你照着抄的垃圾文章就赶紧躲远点,这种东西最害人. 算了扯远了,只是突然想到略作感慨. 回到正题,你如果发现配置爆红了,如果标签啥的都嵌套都没问题,那肯定 是少了啥标签了,你就参考Effective Pom里面的标签是咋嵌套的,把缺的加上就行了.  还有一种情况是当你改好了pom.xml中的标签之后却发现pom.xml的第一行又报错了,就是最开始那行一堆地址那一行.这一行报错主要是你修改了pom.xml之后maven本身并不知道你改了这个文件,可能你新加了插件,也可能你配了别的但是maven现在还不知道这些,它还是按照你没改之前的状态在加载,所以你可以鼠标放上去看它就会提示你,你配置的插件不存在或者你新加的那些配置里的东西不存在啥的. 这时候你选中你的工程右键 Maven--update project.完了之后报错就没了. update project这个命令就是根据当前的配置更新你的工程,这时候会从仓库里重新给你的工程加载所需要的所有东西.有时候你发现工程的某个文件夹报错了,但是点进去之后发现并没有任何文件报错,也是因为工程状态没有及时更新. 你也可以执行这个命令,如果配置啥的都没问题,那么update之后自然就好了.
  2. maven有本地仓库和远程仓库,远程仓库又包含中央仓库和私服还有一些开源的地址.当你在本地装好maven后,你的本地仓库默认是在C盘\用户\.m2\repositories,但是这个.m2\repositories在你还没有执行任何maven命令之前你是看不到的.你必须得随便执行一个maven命令这个文件夹才会出现,出现之后你需要手动拷贝一个settings.xml文件放到.m2下,然后执行更新仓库的命令之后repository文件夹就被默认创建并且所有需要的资源也被更新到这个目录下了.但是实际开发中并不建议将仓库放C盘,假如重装系统那你这些东西就没了,你又得重搞一遍.最主要的是这个路径刚开始你看不到,还得执行命令,初学者一不小心就执行错了,然后搞得一团糟.所以直接把仓库换一个地方. 在D盘下新建一个MAVEN文件夹,这个名字可以随意起,然到你下载的apache-maven-3.5.4文件夹,里面的conf文件夹下有个settings.xml,打开这个文件并找到localRepository标签,把标签中的路径改成 D:\MAVEN  就是你刚新建那个路径,保存之后把这个文件拷贝一份到 D:\MAVEN下. 改完拷贝完之后打开eclipse并选择Window--preferences--Maven--User settings 这时候弹框中会有一个global setting和一个user setting, global就是全局设置,所以使用这个电脑的人都是这个设置,而user 就是个人的setting,个人的setting会覆盖全局的,所以假如选择了个人的文件路径,那么全局的选不选都无所谓,但是两个都选还是按照个人的进行,update setting那个按钮的作用就是去覆盖全局setting.xml,为啥maven会多此一举呢,主要是因为,当maven进行升级的时候,全局的setting.xml里面的所有东西都会被清除,所以这就是我们最开始把apache-maven-3.5.4下conf中的setting拷贝了一份到MAVEN下的原因.不拷完全可以,你就把
  apache-maven-3.5.4下的conf下的setting改了,把仓库路径改成MAVEN,啥也不用再配置就可以用了,在进行user setting设置的时候直接选全局变量就可以了,但是假如有一天你升级了maven,那你这个全局变量就被清了,假如你只改了几行还好,万一改的多那你就哭去吧,所以我们复制了一份用的是user settings.
  事实上,maven并不会自己自动进行升级,上面所指的升级是人为手动升级,也就是把这个版本的包删了然后下了一个别的版本的包放在之前那个目录下了,这样你的setting.xml肯定就不在了啊,所以提前往MAVEN文件夹中拷贝一份就是这个道理.
* 另一些注意事项
  - Maven 提倡使用一个共同的标准目录结构,也就是约定优于配置,大家尽量约定好一个统一的路径,以减少配置文件.
  ```
  ${basedir} 存放pom.xml和所有的子目录
  ${basedir}/src/main/java 项目的java源代码
  ${basedir}/src/main/resources 项目的资源,比如说property文件，springmvc.xml
  ${basedir}/src/test/java 项目的测试类，比如说Junit代码
  ${basedir}/src/test/resources 测试用用的资源
  ${basedir}/src/main/webapp/WEB-INF web应用文件目录，web项目的信息，比如存放web.xml、本地图片、jsp视图页面
  ${basedir}/target 打包输出目录
  ${basedir}/target/classes 编译输出目录
  ${basedir}/target/test-classes 测试编译输出目录 
  Test.java Maven只会自动运行符合该命名规则的测试类
  ~/.m2/repository Maven默认的本地仓库目录位置
  ```
  - Maven 3.3 要求 JDK 1.7 或以上,Maven 3.2 要求 JDK 1.6 或以上,Maven 3.0/3.1 要求 JDK 1.5 或以上.否则无法运行.
  - 所有POM文件都需要project元素和三个必需子元素：groupId(大ID),artifactId(小ID),version。还有个模型版本标签,需要设置为 4.0。
  - 配置文件详解:
  ```
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0http://maven.apache.org/maven-v4_0_0.xsd">
    
    <!--父项目的坐标。如果当前项目中没有规定某个元素的值,那么父项目中的对应值即为其默认值.坐标包括group ID，artifact ID和version。-->
    <parent>        
        <artifactId />    <!--被继承的父项目的构件标识符 -->      
        <groupId />   <!--被继承的父项目的全球唯一标识符 -->        
        <version />   <!--被继承的父项目的版本 -->
        <relativePath />   <!-- 父项目的pom.xml文件的相对路径。默认值是../pom.xml。Maven首先在构建当前项目的地方寻找父项目的pom,
    </parent>                       其次在你这里配的这个路径下找,后在本地仓库，最后在远程仓库寻找父项目的pom。 -->        
    
    <!--声明项目描述符遵循哪一个POM模型版本。虽然很少改但仍然不可省略,这是为了当Maven引入了新的特性或者其他模型变更的时候，确保稳定性。-->
    <modelVersion>4.0.0</modelVersion>     // 一般配置成4.0.0就行了.
    
    <groupId>asia.banseon</groupId>    <!--项目的全球唯一标识组名称,构建时生成的路径会由此名称生成，
                                       如com.mycompany.app生成的相对路径为：/com/mycompany/app -->
    <artifactId>banseon-maven2</artifactId>  <!-- 同一个组下的唯一工程(也可以叫构件)名称.构件是项目产生或使用的一个东西，
                                              Maven为项目产生的构件包括：JARs,源码,二进制发布,WARs等 -->
    <packaging>jar</packaging>                <!--项目产生的构件类型，例如jar、war、ear、pom-->
    <version>1.0-SNAPSHOT</version>     <!--项目当前版本，格式为:主版本.次版本.增量版本-限定版本号 -->
    
    
    <name>banseon-maven</name>    <!--项目的名称, Maven产生的文档用 -->
    <url>http://www.baidu.com/banseon</url>   <!--项目主页的URL, Maven产生的文档用 -->
    <description>A maven project to study maven.</description>  <!-- 项目的详细描述, Maven 产生的文档用。-->
    
    <!--描述了这个项目构建环境中的前提条件。 -->
    <prerequisites>   
        <maven />     <!--构建该项目或使用该插件所需要的Maven的最低版本 -->
    </prerequisites>
    
    <!--项目的问题管理系统(Bugzilla, Jira, Scarab,或任何你喜欢的问题管理系统)的名称和URL，本例为 jira -->
    <issueManagement>       
        <system>jira</system>    <!--问题管理系统（例如jira）的名字， -->
        <url>http://jira.baidu.com/banseon</url>  <!--该项目使用的问题管理系统的URL -->
    </issueManagement>
```
``` 
    <!--项目持续集成信息 -->
    <ciManagement>
        <system />    <!--持续集成系统的名字，例如continuum -->
        <url />   <!--该项目使用的持续集成系统的URL（如果持续集成系统有web接口的话）。 -->
        
        <notifiers> <!--构建完成时，需要通知的开发者/用户的配置项。包括被通知者信息和通知条件（错误，失败，成功，警告） -->
            <notifier>  <!--配置一种方式，当构建中断时，以该方式通知用户/开发者 -->
                <type />  <!--传送通知的方式 -->
                <sendOnError />  <!--发生错误时是否通知 -->
                <sendOnFailure />  <!--构建失败时是否通知 -->
                <sendOnSuccess />  <!--构建成功时是否通知 -->
                <sendOnWarning />  <!--发生警告时是否通知 -->
                <address />  <!--通知发送到哪里.由于某些原因这个标签不赞成使用->
                <configuration />  <!--扩展配置项 -->
            </notifier>
        </notifiers>
    </ciManagement>
    
    <inceptionYear />  <!--项目创建年份，4位数字。当产生版权信息时需要使用这个值。 -->
    <mailingLists>  <!--项目相关邮件列表信息 -->        
        <mailingList>  <!--该元素描述了项目相关的所有邮件列表。自动产生的网站引用这些信息。 -->            
            <name>Demo</name>  <!--邮件的名称 -->           
            <post>banseon@126.com</post>  <!--发送邮件的地址或链接，如果是邮件地址，创建文档时，链接会被自动创建 -->            
            <subscribe>banseon@126.com</subscribe>  <!--订阅邮件的地址或链接，如果是邮件地址，创建文档时，链接会被自动创建 -->            
            <unsubscribe>banseon@126.com</unsubscribe>  <!--取消订阅邮件的地址或链接，如果是邮件地址，创建文档时,接会被自动创建 -->           
            <archive>http:/hi.baidu.com/banseon/demo/dev/</archive>  <!--你可以浏览邮件信息的URL -->
        </mailingList>
    </mailingLists>
    
    <!--项目开发者列表 -->
    <developers>        
        <developer>  <!--某个项目开发者的信息 -->            
            <id>HELLO WORLD</id>  <!--SCM里项目开发者的唯一标识符 -->            
            <name>banseon</name>  <!--项目开发者的全名 -->            
            <email>banseon@126.com</email>  <!--项目开发者的email -->            
            <url />  <!--项目开发者的主页的URL -->            
            <roles>  <!--项目开发者在项目中扮演的角色，角色元素描述了各种角色 -->
                <role>Project Manager</role>
                <role>Architect</role>
            </roles>            
            <organization>demo</organization>  <!--项目开发者所属组织 -->            
            <organizationUrl>http://hi.baidu.com/banseon</organizationUrl>  <!--项目开发者所属组织的URL -->            
            <properties>  <!--项目开发者属性，如即时消息如何处理等 -->
                <dept>No</dept>
            </properties>            
            <timezone>-5</timezone>  <!--项目开发者所在时区， -11到12范围内的整数。 -->
        </developer>
    </developers>
    
    <!--项目的其他贡献者列表 -->
    <contributors>        
        <contributor>  <!--项目的其他贡献者。参见developers/developer元素 -->
            <name />
            <email />
            <url />
            <organization />
            <organizationUrl />
            <roles />
            <timezone />
            <properties />
        </contributor>
    </contributors>
    
    <!--项目所有License(证书)列表.只列出当前项目的即可,依赖的那些不列.果列出多个license，用户可以选择它们中的一个而不是接受所有license。 -->
    <licenses>        
        <license>  <!--描述了项目的license，用于生成项目的web站点的license页面，其他一些报表和validation也会用到该元素。 -->            
            <name>Apache 2</name>  <!--license用于法律上的名称 -->            
            <url>http://www.baidu.com/banseon/LICENSE-2.0.txt</url>   <!--官方的license正文页面的URL -->            
            <distribution>repo</distribution>  <!--项目分发的主要方式： repo，可以从Maven库下载 manual， 用户必须手动下载和安装依赖 -->
            <comments>A business-friendly OSS license</comments>  <!--关于license的补充信息 -->
        </license>
    </licenses>
    
    <!--SCM(Source Control Management)标签允许你配置你的代码库，供Maven web站点和其它插件使用。 -->
    <scm>        
        <connection>  <!--SCM的URL,该URL描述了版本库和如何连接到版本库.请自行查看看SCMs提供的URL格式和列表。该连接只读。 -->
            scm:svn:http://svn.baidu.com/banseon/maven/banseon/banseon-maven2-trunk(dao-trunk)
        </connection>        
        <developerConnection>  <!--给开发者使用的，类似connection元素。即该连接不仅仅只读 -->
            scm:svn:http://svn.baidu.com/banseon/maven/banseon/dao-trunk
        </developerConnection>        
        <tag />  <!--当前代码的标签，在开发阶段默认为HEAD -->        
        <url>http://svn.baidu.com/banseon</url>  <!--指向项目的可浏览SCM库（例如ViewVC或者Fisheye）的URL。 -->
    </scm>
    
    <!--描述项目所属组织的各种属性。Maven产生的文档用 -->
    <organization>        
        <name>demo</name>  <!--组织的全名 -->        
        <url>http://www.baidu.com/banseon</url>  <!--组织主页的URL -->
    </organization>
```
``` 
    <!--构建项目需要的信息 -->
    <build>        
        <sourceDirectory />  <!--项目源码目录，当构建项目的时候，构建系统会编译目录里的源码。该路径是相对于pom.xml的相对路径。 -->        
        <scriptSourceDirectory />  <!--项目脚本源码目录.大多情况下该目录下的内容会被拷贝到输出目录(因为脚本是解释执行,不需要编译)。 -->        
        <testSourceDirectory />  <!--项目单元测试源码目录，当测试项目的时候，构建系统会编译目录里的源码。该路径是相对于pom.xml的相对路径。 -->
        <outputDirectory />  <!--被编译过的应用程序class文件存放的目录。 -->        
        <testOutputDirectory />  <!--被编译过的测试class文件存放的目录。 -->        
        
        <extensions>   <!--项目用到的构建扩展 -->            
            <extension>  <!--描述使用到的构建扩展。 -->                
                <groupId />  <!--构建扩展的groupId -->                
                <artifactId />  <!--构建扩展的artifactId -->                
                <version />  <!--构建扩展的版本 -->
            </extension>
        </extensions>
                
        <defaultGoal />  <!--当项目没有规定目标（Maven2 叫做阶段）时的默认值 -->
        
        <resources>  <!--项目相关的所有资源路径列表，例如和项目相关的属性文件，这些资源被包含在最终的打包文件里。 -->            
            <resource>                                     
                <targetPath />  <!-- 描述了资源的目标路径。该路径相对target/classes目录（例如${project.build.outputDirectory}）。
                <filtering />  <!--是否使用参数值代替参数名。参数值取自properties元素或者文件里配置的属性，文件在filters元素里列出。 --> 
                <directory />  <!--描述存放资源的目录，该路径相对POM路径 -->                
                <includes />  <!--包含的模式列表，例如**/*.xml. -->                
                <excludes />  <!--排除的模式列表，例如**/*.xml -->
            </resource>
        </resources>
        
        <testResources>  <!--单元测试相关的所有资源路径，例如和单元测试相关的属性文件。 -->            
            <testResource>
                <targetPath />
                <filtering />
                <directory />
                <includes />
                <excludes />
            </testResource>
        </testResources>
                
        <directory />  <!--构建产生的所有文件存放的目录 -->        
        <finalName />   <!--产生的构件的文件名，默认值是${artifactId}-${version}。 -->        
        <filters />  <!--当filtering开关打开时，使用到的过滤器属性文件列表 -->
        
        <!--子项目可以引用的默认插件信息。该插件配置项直到被引用时才会被解析或绑定到生命周期。给定插件的任何本地配置都会覆盖这里的配置 -->
        <pluginManagement>            
            <plugins>                
                <plugin>                    
                    <groupId />  <!--插件在仓库里的group ID -->                    
                    <artifactId />  <!--插件在仓库里的artifact ID -->                    
                    <version />  <!--被使用的插件的版本（或版本范围） -->                    
                    <extensions />  <!--是否从该插件下载Maven扩展,例如打包和类型处理器,出于性能考虑,只在真的需要下载时,才设置成enabled-->
                    <executions>  <!--在构建生命周期中执行一组目标的配置。每个目标可能有不同的配置。 -->                        
                        <execution>  <!--execution元素包含了插件执行需要的信息 -->                            
                            <id />  <!--执行目标的标识符，用于标识构建过程中的目标，或者匹配继承过程中需要合并的执行目标 -->
                            <phase />  <!--绑定了目标的构建生命周期阶段，如果省略，目标会被绑定到源数据里配置的默认阶段 -->
                            <goals />  <!--配置的执行目标 -->                            
                            <inherited />  <!--配置是否被传播到子POM -->                            
                            <configuration />  <!--作为DOM对象的配置 -->
                        </execution>
                    </executions>                    
                    <dependencies>  <!--项目引入插件所需要的额外依赖 -->                        
                        <dependency>  <!--参见dependencies/dependency元素 -->
                            ......
                        </dependency>
                    </dependencies>                    
                    <inherited />  <!--任何配置是否被传播到子项目 -->                    
                    <configuration />  <!--作为DOM对象的配置 -->
                </plugin>
            </plugins>
        </pluginManagement>
        
        
        <!--当前项目使用的插件列表 -->
        <plugins>            
            <plugin>  <!--参见build/pluginManagement/plugins/plugin元素 -->
                <groupId />
                <artifactId />
                <version />
                <extensions />
                <executions>
                    <execution>
                        <id />
                        <phase />
                        <goals />
                        <inherited />
                        <configuration />
                    </execution>
                </executions>
                <dependencies>                    
                    <dependency>  <!--参见dependencies/dependency元素 -->
                        ......
                    </dependency>
                </dependencies>
                <goals />
                <inherited />
                <configuration />
            </plugin>
        </plugins>
    </build>
```
```        
      <!--在列的项目构建profile，如果被激活，会修改构建处理.里面详细的配置内容参考上面已经配置过的 -->
    <profiles>        
        <profile>  <!--根据环境参数或命令行参数激活某个构建处理 -->            
            <id />  <!--构建配置的唯一标识符。即用于命令行激活，也用于在继承时合并具有相同标识符的profile。 -->
            
            <!--自动触发profile的条件,Activation是profile的开启钥匙.profile能够在某些特定环境中自动使用某些特定值,这些环境通过此元素指定。 -->
            <activation>                
                <activeByDefault />  <!--profile默认是否激活的标志 -->                
                <jdk />  <!--当匹配的jdk被检测到，profile被激活。例如，1.4激活JDK1.4，1.4.0_2，而!1.4激活所有版本不是以1.4开头的JDK。 -->
                <os>  <!--当匹配的操作系统属性被检测到，profile被激活。os元素可以定义一些操作系统相关的属性。 -->                    
                    <name>Windows XP</name>  <!--激活profile的操作系统的名字 -->                    
                    <family>Windows</family>  <!--激活profile的操作系统所属家族(如 'windows') -->                    
                    <arch>x86</arch>  <!--激活profile的操作系统体系结构 -->                    
                    <version>5.1.2600</version>  <!--激活profile的操作系统版本 -->
                </os>
                
                <!--如果Maven检测到某一个属性（其值可以在POM中通过${名称}引用），其拥有对应的名称和值，Profile就会被激活。
                如果值 字段是空的，那么存在属性名称字段就会激活profile，否则按区分大小写方式匹配属性值字段 -->
                <property>                    
                    <name>mavenVersion</name>  <!--激活profile的属性的名称 -->                    
                    <value>2.0.3</value>  <!--激活profile的属性的值 -->
                </property>
                
                <!--提供一个文件名，通过检测该文件的存在或不存在来激活profile。missing是如果不存在则激活,exists是存在则激活profile。 -->
                <file>                    
                    <exists>/usr/local/hudson/hudson-home/jobs/maven-guide-zh-to-production/workspace/</exists>                    
                    <missing>/usr/local/hudson/hudson-home/jobs/maven-guide-zh-to-production/workspace/</missing>
                </file>
            </activation>
            
            <build>   <!--构建项目所需要的信息。参见上面的build配置 -->
                <defaultGoal />
                <resources>
                    .............
                </resources>
                <testResources>
                    .............
                </testResources>
                <directory />
                <finalName />
                <filters />
                <pluginManagement>
                    <plugins>                        
                        <plugin> 
                            ............
                        </plugin>
                    </plugins>
                </pluginManagement>
                <plugins>                    
                    <plugin>
                        ............
                    </plugin>
                </plugins>
            </build>
            
            
            <!--模块（有时称作子项目） 被构建成项目的一部分。列出的每个模块元素是指向该模块的目录的相对路径 -->
            <modules />
            
            <repositories>  <!--发现依赖和扩展的远程仓库列表。参考上面的仓库配置 -->                
                ..............
            </repositories>
            
            <!--发现插件的远程仓库列表，这些插件用于构建和报表  包含需要连接到远程插件仓库的信息,参考上面的-->
            <pluginRepositories>                
                <pluginRepository>
                     ..............
                </pluginRepository>
            </pluginRepositories>
           
            <dependencies>  //项目依赖,参考上面的                
                <dependency>
                    ......
                </dependency>
            </dependencies>

            <reports />
            
            <reporting>
                ......
            </reporting>
            
            <dependencyManagement>
                <dependencies>                    
                    <dependency>
                        ......
                    </dependency>
                </dependencies>
            </dependencyManagement>
                        
            <distributionManagement>
                ......
            </distributionManagement>
            <!--参见properties元素 -->
            <properties />
        </profile>
    </profiles>
```
``` 
    <!--模块（有时称作子项目） 被构建成项目的一部分。列出的每个模块元素是指向该模块的目录的相对路径 -->
    <modules />    
    <repositories>  <!--发现依赖和扩展的远程仓库列表。 --> 
    <!--有了releases和snapshots,POM就可以在每个单独的仓库中，为每种类型的构件采取不同的策略。可能有人会决定只为开发开启对快照版本下载的支持。-->
        <repository>  <!--包含需要连接到远程仓库的信息 -->            
            <releases>  <!--如何处理远程仓库里发布版本的下载 -->                
                <enabled />  <!--true或者false表示该仓库是否为下载某种类型构件（发布版，快照版）开启。 -->                
                <updatePolicy />  <!--更新频率.比较本地POM和远程POM的时间戳.always,daily(默认,每日),interval(间隔)：X(分钟),never -->
                <checksumPolicy />  <!--当Maven验证构件校验文件失败时该怎么做：ignore（忽略），fail（失败），或者warn（警告）。 -->
            </releases>            
            <snapshots>  <!-- 如何处理远程仓库里快照版本的下载。--> 
                <enabled />
                <updatePolicy />
                <checksumPolicy />
            </snapshots>            
            <id>banseon-repository-proxy</id>  <!--远程仓库唯一标识符。可以用来匹配在settings.xml文件里配置的远程仓库 -->            
            <name>banseon-repository-proxy</name>  <!--远程仓库名称 -->            
            <url>http://192.168.1.169:9999/repository/</url>  <!--远程仓库URL，按protocol://hostname/path形式 -->
            <!-- 用于定位和排序构件的仓库布局类型-可以是default（默认）或者legacy（遗留）。Maven 2为其仓库提供了一个默认的布局；然 
                而，Maven 1.x有一种不同的布局。我们可以使用该元素指定布局是default（默认）还是legacy（遗留）。 -->
            <layout>default</layout>
        </repository>
    </repositories>
        
    <pluginRepositories>        
        <pluginRepository>
            ......
        </pluginRepository>
    </pluginRepositories>
 
 
    <!--项目相关的所有依赖。 这些依赖组成了项目构建过程中的一个个环节。它们自动从项目定义的仓库中下载。 -->
    <dependencies>
        <dependency>            
            <groupId>org.apache.maven</groupId>            
            <artifactId>maven-artifact</artifactId>            
            <version>3.8.1</version>            
            <type>jar</type>
            <!-- 依赖的分类器。分类器可以区分属于同一个POM，但不同构建方式的构件。分类器名被附加到文件名的版本号后面。
              例如，如果你想要构建两个单独的JAR，一个使用Java 1.4编译器，另一个使用Java 6编译器，你就可以使用分类器 -->
            <classifier></classifier>
            <!--依赖范围。在项目发布过程中，帮助决定哪些构件被包括进来。 
                - compile ：默认范围，用于编译 
                - provided：类似于编译，但支持你期待jdk或者容器提供，类似于classpath 
                - runtime: 在执行时需要使用 - test: 用于test任务时使用 - system: 需要外在提供相应的元素。通过systemPath来取得 
                - systemPath: 仅用于范围为system。提供相应的路径 - optional: 当项目自身被依赖时，标注依赖是否传递。用于连续依赖时使用 -->
            <scope>test</scope>
            <!--为依赖规定了文件系统上的路径。需要绝对路径而不是相对路径。已过时,不推荐使用 -->
            <systemPath></systemPath>
            <!--告诉maven你只依赖指定的项目，不依赖项目的依赖。此元素主要用于解决版本冲突问题 -->
            <exclusions>
                <exclusion>
                    <artifactId>spring-core</artifactId>
                    <groupId>org.springframework</groupId>
                </exclusion>
            </exclusions>
            <!--如果你在项目B中把C依赖声明为可选，你就需要在依赖于B的项目（例如项目A）中显式的引用对C的依赖。 -->
            <optional>true</optional>
        </dependency>
    </dependencies>
        
    <reports></reports>  //过时
    
    <reporting>  <!--描述使用报表插件产生报表的规范。当用户执行"mvn site"，这些报表就会运行。 在页面导航栏能看到所有报表的链接。 -->        
        <excludeDefaults />  <!--true，则，网站不包括默认的报表。这包括"项目信息"菜单中的报表。 -->        
        <outputDirectory />  <!--所有产生的报表存放到哪里。默认值是${project.build.directory}/site。 -->        
        <plugins>  <!--使用的报表插件和他们的配置。 -->            
            <plugin>                
                <groupId />                
                <artifactId />                
                <version />                
                <inherited />                
                <configuration />   <!--报表插件的配置 -->                
                <reportSets>                    
                    <reportSet>  <!--表示报表的一个集合，以及产生该集合的配置 -->                        
                        <id />                        
                        <configuration />  <!--产生报表集合时，被使用的报表的配置 -->                        
                        <inherited />  <!--配置是否被继承到子POMs -->                        
                        <reports />  <!--这个集合里使用到哪些报表 -->
                    </reportSet>
                </reportSets>
            </plugin>
        </plugins>
    </reporting>
```
``` 
    <!-- 继承自该项目的所有子项目的默认依赖信息。-->
    <dependencyManagement>
        <dependencies>            
            <dependency>
                ......
            </dependency>
        </dependencies>
    </dependencyManagement>
    
    <!--项目分发信息，在执行mvn deploy后表示要发布的位置。有了这些信息就可以把网站部署到远程服务器或者把构件部署到远程仓库。 -->
    <distributionManagement>        
        <repository>  <!--部署项目产生的构件到远程仓库需要的信息 -->            
            <uniqueVersion />  <!--分配给快照一个唯一的版本号 -->
            <id>banseon-maven2</id>
            <name>banseon maven2</name>
            <url>file://${basedir}/target/deploy</url>
            <layout />
        </repository>
        <!--构件的快照部署到哪里？如果没有配置该元素，默认部署到repository元素配置的仓库，参见distributionManagement/repository元素 -->
        <snapshotRepository>
            <uniqueVersion />
            <id>banseon-maven2</id>
            <name>Banseon-maven2 Snapshot Repository</name>
            <url>scp://svn.baidu.com/banseon:/usr/local/maven-snapshot</url>
            <layout />
        </snapshotRepository>
        
        <site>  <!--部署项目的网站需要的信息 -->            
            <id>banseon-site</id>  <!--部署位置的唯一标识符，用来匹配站点和settings.xml文件里的配置 -->            
            <name>business api website</name>  <!--部署位置的名称 -->            
            <url>  <!--部署位置的URL，按protocol://hostname/path形式 -->
                scp://svn.baidu.com/banseon:/var/www/localhost/banseon-web
            </url>
        </site>
        
        <!--项目下载页面的URL。如果没有该元素，用户应该参考主页。使用该元素的原因是：帮助定位那些不在仓库里的构件（由于license限制）。 -->
        <downloadUrl />
        
        <!--如果构件有了新的group ID和artifact ID（构件移到了新的位置），这里列出构件的重定位信息。 -->
        <relocation>            
            <groupId />    //新组id          
            <artifactId />  //新构件id           
            <version />     //新版本        
            <message />  <!--显示给用户的，关于移动的额外信息，例如原因。 -->
        </relocation>
        
        <!-- 给出该构件在远程仓库的状态。不得在本地项目中设置该元素，因为这是工具自动更新的. none（默认）,converted（仓库管理员从 
            Maven 1 POM转换过来），partner（直接从伙伴Maven 2仓库同步过来），deployed（从Maven 2实例部 署），verified（被核实时正确)-->
        <status />
    </distributionManagement>
    <!--以值替代名称，Properties可以在整个POM中使用，也可以作为触发条件,格式是<name>value</name>。 -->
    <properties />
</project>
  ```

