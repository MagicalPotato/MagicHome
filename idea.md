#### IDEA的基础设置项 Settings
* Appearance&Behavior-Appearance 设置主题和菜单字体
* Appearance&Behavior- System Settings 把右边最上面的复选框去掉,那么每次打开idea的时候就不会默认打开上次关闭时候的工程了.
* create new project的时候如果要创建一个java web工程,那么在创建的时候就勾选上右边Java EE 里面的webApplication ,同时直接点击Java EE和webApplication还可以在下面分别设置Java EE和servlet的版本.创建好web工程之后单击工程然后会在工具栏看到一个空白的下拉框,里面有个Edit Configuration,点击之后可以配置tomcat等一系列部署或者管理该工程会用到的东西.注意中间有两个复选框分别选Redeploy和Update classes and resources,这样当你改了工程里的东西的时候tomcat就会自动帮你编译并部署,不需要你再去重启了.
* 新建的web工程没有lib文件夹,你需要新建并把要用的jar包拷贝到里面.但是拷贝过去还不能使用.点击工具栏倒数第二个图标,就是很多小方块那个图标(实际上就是Project construction),在libarlies里选择java并把你刚新建的lib文件夹整个添加上,然后到Modules-dependences勾选你刚添加的lib,同时点击框子又上角那个+号,选择libary,里面会出现你之前配置的tomcat,点击它然后确定.这时候这个工程的所有jar包依赖就添加好了. 这里有个问题注意,在写代码的时候有时候会用到HttpServlet等接口,但是这些接口并不是通过jar包引入的,而是在tomcat中,所以我们在添加依赖的时候把tomcat也当成一个依赖源引入了.
* Editor-Font 设置代码字体. 复选框:使用等宽字体(就是每个字符的占用位置一样大)
* Editor-Color Sheme 修改各种样式和颜色,比如控制台字体颜色背景等.注意颜色是填写颜色值不是直接选择的. 
* Editor- File Encodings 改全局编码,新建工程的默认编码,还有工程中的属性文件的编码.属性文件的编码要勾选右边的复选框,否则属性文件中的中文无法正常显示.
* Editor:
  - General 右边Mouse一栏有个 Change font size with Ctrl+Mouse Wheel 的复选框,意思是按住ctrl同时滑动鼠标滚轮能够使得代码改变字体大小.这个注意下. 还有个Show quick documentation on mouse move,是说鼠标放在类上会提示类的文档说明,右边的数字是说要延迟多少毫秒显示还是立即显示.
  - General-Appearance(样式),里面有个显示行号选项,行号下面那个框是说在方法和方法之间显示一条分割线.
  - General-Code Completion,是设置代码提示的,下拉框设置成None那么无论大写还是小写开头都会给你提示信息.
  - General-Auto Import 下拉框选择All,并勾选下面的两个复选框,那么拷贝代码的话就不会询问而是自动导入需要的包.
* Editor-Code Style-java 可以设置代码格式化的一些东西,包括一个tab几个空格,一个回车几个空格等等..
* plugins 安装官方和第三方插件,或者从磁盘安装插件. 插件安装完成后需要重启idea
* Configure-project structure-project 配置工程的java开发环境JDK
* 工程框view中可以选择那些东西展示在页面上,比如工具栏,工程结构栏目等.
* 通过new的方式创建类的时候需要填写类名称,写的时候可以同时指定包名称比如xxx.xxx.xxx.Hello,ieda会自动创建包和类.
* idea有自动保存功能所以不需要频繁去按ctrl+s
* 工程框help-Edit Custom VM Options,如果你的机器配置还不错可以通过这个选项把idea的参数调大一点这样idea的响应速度就会变快一些.注意这是配置ieda的参数不是java虚拟机的参数
* 工程框File中有两个设置,一个是Settings,另一个是Other Settings,两个设置有些一样有些不一样.Settings只会对当前的工程生效,Other是全局设置对所有的工程都生效. 一般配置全局的东西建议都从工程总览那个框有下角的configure中去配置.
* 全局设置中的Build,Execution,Deployment中配置项目管理工具比如Maven.如果你配置好了Maven,那么create new project的时候就可以在左侧进行选择创建一个maven工程了.注意创建maven工程的时候要勾选create from archetype那个复选框.这个复选框的意思是maven给你给定了一些默认的工程骨架结构,你在创建响应的工程的时候需要选择对应的工程骨架.比如你要创建一个单纯的java ee的maven工程那么你就选择Java EE的骨架.而你要创建一个web且由maven管理的项目的时候你就要选择对应的webapp骨架.
* 创建好的maven工程打开后工作台的右边有一个Maven Project侧边条,点开之后就是maven的一些常用命令和操作.包括应用新的修改,自定义操作命令,还有查看当前工程的依赖结构等.
* 工作空间的VCS选项用来进行版本控制,可以选择配置SVN和Git来管理当前工程.
