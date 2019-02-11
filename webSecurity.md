* windows下比较重要的几个文件夹: Program Files默认安装64位软件,Program Files(x86)默认安装32位的软件的,Program Data保存程序运行时临时日志同时
也是黑客最喜欢感染的目录,windows是系统的安装目录,里面的文件都很重要,其中\system32\config\Sam文件保存着系统用户权限.
* XSS跨站脚本:通过html的漏洞往服务器注入脚本获取信息.
* CFRS跨站请求伪造:黑客到你的cookie,伪造一个转账页面并隐藏在网页的弹窗中,你不小心一点击弹窗就相当于点了转账页面,最终默默的完成了转账.
* SQL注入:登录时要输入用户名和密码,后台会这样查询select name from table where name='xx' and passwd=xx,当我在页面填写登录密码的时候我把
用户名写成admin' -- ,admin单引号空格杠杠空格,这样sql就变成select name from table where name='admin' -- and passwd=xx,--在数据库中是注释
的意思,所以--后面的内容都被注释了,那么如果数据库中有admin这个用户,自然就能直接访问成功.
