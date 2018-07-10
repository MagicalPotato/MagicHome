### Maven
* maven的安装和使用
1. 从官网下载maven的zip包,然后将其解压在本地D盘或者别的盘下,例如  D:\apache-maven-3.5.4 
2. 像配置jdk那样配置maven的环境变量. 新建系统变量MAVEN_HOME(名字就叫这个,别手欠搞个什么别的花样叫别的名字,用别的名字也可以,但是有一大堆东西要改,
而且我也不会,所以你还是直接叫这个名字),变量值就是刚你解压那个路径. 然后修改已经存在的Path变量,新增 %MAVEN_HOME%\bin;(别忘了后面的分号)
3. 随便在 哪里打开cmd黑框,输入  mvn -v  命令,看看成功没
4. 安装了maven之后,你想要在eclipse中用maven来管理你的工程,那就要把你刚安装的maven引入你的eclipse  Window--preferences--Maven--installations
点击add,把刚你装的那个maven添加上,只选择路径,然后给起个名字,名字最好就是你的maven的文件夹名称,然后finish,然后aplay就行了.
5. 新建一个maven工程  在Project Explorer区域右键,选择new一个Maven Project,注意new的是一个Maven Project,不是默认的Projext,也不是java Project.在
新建Maven Project工程的时候有一个 create a simple project选项.如果你新建的工程有固定的框架可以用
