* useradd 添加用户,任何版本都通用
* passwd 给用户设置密码,linux系统在任何场景下设置密码光标都不会有任何反应
* userdel 删除某个用户,有个-r的选项,可以一并删除该用户的/home目录,root下要慎用,不但杀人还抄家.linux的每个用户都有一个home目录,存放着个用户的所有东西
* xxx -help 显示该命令简单的使用帮助; man xxx 或 man 配置文件 显示详细的帮助文档,输入q退出浏览 ; info xxx 或 info 配置文件 比man更详细.空格翻页
* su 某个用户  切换到某个用户,需要输入密码; su root或 su 后面啥都不写 ,则默认切换到root,也会要密码. 如果你当前在/haha目录下,那么切到root后也是
root的/haha目录;如果输入 su -,则会切到root的/home目录下,也就是/root,root用户的home目录就是/root. 如果你当前在root用户下,那么你可以使用su someuser -
切换到任意用户的/home/someuser目录下,注意这个someuser就时你切的那个用户的用户名. 如果当前在某个用户,切到了另一个,办完事你又想回刚那个用户,就输入exit
* 
