### Maven
* maven的安装和使用
  1. 从官网下载maven的zip包,然后将其解压在本地D盘或者别的盘下,例如  D:\apache-maven-3.5.4 
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
  1. maven工程建起来之后根据使用需要一般要修改pom.xml,默认的pom.xml只有几行配置,主要配置你的工程的groupid和artifactId,版本还有打包方式.后续可以根据需要对pom.xml进行修改.你会发现eclipse窗口打开的pom.xml左边的名称左边还有一大堆可以点击的分页,其中一个是Effective Pom,就在pom.xml的左边.这个Effective Pom是一个maven默认的pom.xml配置,也可以说是一个模板,这个模板是没法修改的,当你要修改你的项目的pom.xml时可以参考这个模板,比如说你要在你的pom.xml中加一个插件,那你就参考Effective Pom中人家的插件咋加的,都用了哪些标签,标签的嵌套关系是怎么样的,参考好了之后你就照着样子在你的pom.xml中配置就行了.很多人在配置pom.xml的时候直接从网上找个配置拷贝过来就粘上了发现明明标签没有错误,但是左边就是会爆小红叉.原因就是因为网上那些弱智根本自己也不知道啥原因,像个傻逼一样只知道从别人哪里拷贝,没找过原因,拷来拷去还是不行.网上那些傻逼在那里装模作样一副老学究的样子,但实际上不过是牵强附会,真是误人子弟.所以建议大家以后无论看博客还是文章,发现那些不讲原理只让你照着抄的垃圾文章就赶紧躲远点,这种东西最害人. 算了扯远了,只是突然想到略作感慨. 回到正题,你如果发现配置爆红了,如果标签啥的都嵌套都没问题,那肯定 是少了啥标签了,你就参考Effective Pom里面的标签是咋嵌套的,把缺的加上就行了.  还有一种情况是当你改好了pom.xml中的标签之后却发现pom.xml的第一行又报错了,就是最开始那行一堆地址那一行.这一行报错主要是你修改了pom.xml之后maven本身并不知道你改了这个文件,可能你新加了插件,也可能你配了别的但是maven现在还不知道这些,它还是按照你没改之前的状态在加载,所以你可以鼠标放上去看它就会提示你,你配置的插件不存在或者你新加的那些配置里的东西不存在啥的. 这时候你选中你的工程右键 Maven--update project.完了之后报错就没了. update project这个命令就是根据当前的配置更新你的工程,这时候会从仓库里重新给你的工程加载所需要的所有东西.有时候你发现工程的某个文件夹报错了,但是点进去之后发现并没有任何文件报错,也是因为工程状态没有及时更新. 你也可以执行这个命令,如果配置啥的都没问题,那么update之后自然就好了.
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

