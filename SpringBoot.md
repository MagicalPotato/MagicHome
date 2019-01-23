# springboot整合maven
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
- 新建java类,在新建java类的时候可以直接指定包名:  com.mozun.test.MainApplication,会自动在目录下创建包com.mozun.test

单体架构和微服务架构的区别: 单体架构所有的东西都是做在一个应用里面,最后打成war包,然后部署在环境的tomcat上,如果业务量大不够用了那就多放一些tomcat,每个
里面都部署一份应用. 微服务呢是把一个整体的应用拆分成很多功能模块,同样也是往环境上的Tomcat里面去复制这些模块,一个tomcat部署一个模块,但是我只是复制需要
用的模块,哪个模块用的多那我就在环境上多部署一些.所以就有很多dataImport,dataClean等,但是分析可能就那两三个.


