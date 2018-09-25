#### linux一些命令再次探究
```
pwd - print working directory

cd - Change directory
     [me@linuxbox usr]$ cd ./bin 几乎所有的情况下，你可以省略”./”。它是隐含的
     cd            更改工作目录到你的家目录。
     cd -          更改工作目录到先前的工作目录。
     cd ~user_name 更改工作目录到别的用户的家目录,cd ~bob 会更改工作目录到用户“bob”的家目录。应该是/home/bob/
 
ls - List directory contents
     这个不是缩写 [me@linuxbox bin]$ ls 通常终端提示符会自动显示当前工作目录;
     以 “.” 字符开头的文件名是隐藏文件。ls 命令不能列出它们，用 ls -a 命令就可以;
     任何时候都不要在文件或文件夹名称中使用空格,如果要分隔最好用下划线,这是一条非常有用的建议!!!!
     [me@linuxbox ~]$ ls ~ /usr /bin 列出多个目录下的文件和目录,目录之间用空格隔开,~代表系统的/home目录,linux会给
     每一个用户在/home目录下创建一个该用户的家目录,并不是每个用户都有个/home目录,/home目录只有一个,root用户的家目录是/root;
     [me@linuxbox ~]$ ls -lt --reverse 两个中划线后用来跟一些长选项,主要是短选项不够用;
     [me@linuxbox ~]$ ls -l  会显示目录的详细信息例如: drwxrwxr-x 2  me  me  4096  2007-10-26  17:20  Desktop ;
     /usr 目录可能是最大的一个。它包含普通用户所需要的所有程序和文件,下面又会包含很多别的文件夹;
     
file – Determine file type  //确定文件类型
    [me@linuxbox ~]$ file picture.jpg  linux中并不用后缀来区分文件,所以.jpg的文件不一定就是图片
    picture.jpg: JPEG image data, JFIF standard 1.01  file命令会打印一些介绍该文件的简要信息;
    
less – View file contents  //浏览文件内容 
    less /etc/passwd  less实际上一个文本读取程序,老式的more的进化版,可以滚动,q键退出
```
```
cp – Copy files and directories  
    cp file1 file2  复制文件1内容到文件2中。若2已经存在,则2所有内容会被1的内容重写。若2不存在,则创建2。
    cp item... directory  //copy一个或多个文件到某个目录下, -a 复制父文件的所有,包括属性等, -r递归 , -i 提示是否覆盖,默认覆盖  
    可以使用通配符来匹配文件,当然在地址路径中也可以使用通配符, /home/*soft
    * 匹配任意多个字符（包括零个或一个）
    ? 匹配任意一个字符（不包括零个）
    [characters] 匹配任意一个属于字符集中的字符
    [!characters] 匹配任意一个不是字符集中的字符
    [:alnum:] 匹配任意一个字母或数字  // BACKUP.[0-9][0-9][0-9] 以"BACKUP."开头，并紧接着3个数字的文件
    [:alpha:] 匹配任意一个字母  //不要用平常用的那种正则逻辑(比如A-Z    [:digit:] 匹配任意一个数字   例如: [![:digit:]]*不以数字开头的文件
匹配字母)来匹配linux文件,用linux自己的匹配规则,否则可能会出错
    [:lower:] 匹配任意一个小写字母  
    [:upper:] 匹配任意一个大写字母  例如: *[[:lower:]123] 文件名以小写字母结尾，或以 “1”，“2”，或 “3” 结尾的文件

mv – Move/rename files and directories 
    mv file1 file2  移动file1到file2。若file2存在,内容会被file1的内容覆盖。 若file2不存在,则创建file2。两种情况下,file1都不再存在,相当于剪切.
    mv file1 file2 dir1  移动 file1 和 file2 到目录 dir1 中。dir1 必须已经存在。
    mv dir1 dir2  移动dir1中的内容到dir2中,无论dir2在不在都没关系

mkdir – Create directories  // mkdir dir1 dir2 dir3

rm – Remove files and directories
    rm -r file1 dir1  删除文件 file1, 目录 dir1，及 dir1 中的内容。
    rm -rf file1 dir1  同上，但是如果文件 file1，或目录 dir1 不存在的话，rm 仍会继续执行。
    一定要小心这个命令,比如 rm *.html,本来想删除html文件,但是如果不小心多写了个空格rm * .html,那么你所有文件都被删了,通配符会匹配可选参数,在使用
    前一定要先用ls命令对其进行测试;

ln – Create hard and symbolic links
    [me@linuxbox playground]$ ln fun fun-hard  为fun文件创建一个硬链接
    假设文件由两部分组成：文件真正的数据部分 和 该文件的名称部分.当我们创建文件硬链接的时候,实际上是为文件创建了额外的名称,并且这些名称都关联到相同
    的数据部分。这时系统会分配一连串的磁盘块给你那个文件的索引节点,然后索引节点把刚建的那些名称和自己关联起来,这样真实的文件的原名称和所有创建出来
    的名称(也就是硬链接)就会共享同一个索引节点.当删除了文件原始名称(也是个硬链接)和所有其他名称后,原始数据文件才会被真正的删除.如果先删了原始文件,
    那么硬链接就成了坏链接,并在ls时以红色显示.  硬链接有两个缺点,不能跨越物理设备,不能关联目录,只能关联文件。所以出现了软链接,也就是符号链接.
    软链接克服了硬链接的缺点.
    [me@linuxbox playground]$ ln -s ../fun dir1/fun-sym  为上一级目录下的fun文件在dir1目录下创建一个符号链接也就是软链接,当我们再dir1下用ls
    命令查看时会看到这个符号链接 lrwxrwxrwx 1 me  me    6 2008-01-15 15:17 fun-sym -> ../fun ,开头字母是l表示就是一个软链接,6是../fun的字符数.
    对于符号链接，执行的大多数文件操作是针对该符号链接的对象，也就是真实的原始文件,而不是链接本身。 但是当你删除链接的时候，删除链接本身，而不是
    链接的对象,删了符号链接后原始文件依然存在.
```
```
命令可以是一个可执行程序,就像我们所看到的位于目录/usr/bin 中的文件一样。 这一类程序可以是用诸如C和C++语言写成的程序编译的二进制文件, 也可以是由
诸如shell，perl，python，ruby等等脚本语言写成的程序 。命令也可以是shell内建程序,或者由shell写成的批处理脚本. 当然这些脚本可以由其他脚本语言来写.
    [me@linuxbox ~]$ type cp    // cp is /bin/cp   可以看到cp就是一个可执行程序
    [me@linuxbox ~]$ type type   // type is a shell builtins  type命令是shell内建的命令
    [me@linuxbox ~]$ type ls     // ls is aliased to `ls --color=tty`  ls是一个命令的别名,把加了参数的命令简写成了ls
 
在环境上有时候我们可能安装了可执行程序的多个版本,那么which命令可以查看到底是哪一个,但是这个命令只对可执行程序命令有效,因为可执行程序是人为可控的,
所以出现多个版本也不足为奇. 如果对内建或者别名命令使用则会报错. 
    [me@linuxbox ~]$ which ls    //   这个ls是 /bin/ls

man的帮助文档结构都是固定的, 1 用户命令, 2 程序接口,内核系统调用, 3 C库函数程序接口, 4 特殊文件,比如说设备结点和驱动程序, 5 文件格式, 6 游戏娱乐
,如屏幕保护程序, 7 其他方面, 8 系统管理员命令
    [me@linuxbox ~]$ man 5 cd  //直接查看cd命令帮助文档的第五部分,也就是文件格式
    cd [-L|-P] [dir]  // 出现在命令语法说明中的方括号，表示可选的项目。一个竖杠字符 表示互斥选项。 
    whatis cd // man出来的帮助文档通常晦涩难懂,而且显示在屏幕上的排版毫无意义.whatis会给出该命令的帮助手册的名称和一行简洁的命令说明
    
命令可以连在一起写,中间用分号隔开. 也可以用alias自定义linux命令,这样能够简化书写
    [me@linuxbox ~]$ cd /usr; ls; cd -   //到/user下并显示其下除隐藏文件以外的文件然后回到/user下 ,我们可以把这个命令自定义
    成一个简洁的命令以方便使用,但是在定义之前要先查看这个命令在已有的命令中是否存在.
    [me@linuxbox ~]$ type foo    // 我想把这一串定义成名叫foo的命令,先type它, 如果该命令不存在则显示 bash: type: foo: not found
    [me@linuxbox ~]$ alias foo='cd /usr; ls; cd -'    // 用alias自定义新命令.  注意foo='你的命令串', =左右无空格
    [me@linuxbox ~]$ type foo   //再次type foo , 会发现foo是一个命令别名, foo is aliased to `cd /usr; ls ; cd -'
    [me@linuxbox ~]$ unalias foo   // 删除这个别名命令  ,再次type foo , 发现 bash: type: foo: not found
    [me@linuxbox ~]$ alias  // 直接使用alias会显示所有系统环境中的命令别名:  alias l.='ls -d .* --color=tty'   ............
```
```
命令会产生结果或状态信息,结果会输送到一个叫做标准输出的特殊文件（stdout），而状态信息则送到另一个叫做标准错误的文件（stderr）。默认情况下，标准
输出和标准错误都连接到屏幕文件(屏幕也是文件),而不是保存到磁盘。程序从一个叫做标准输入的文件（stdin）得到输入，默认情况下,标准输入文件连接到键盘。
    [me@linuxbox ~]$ ls -l /usr/bin > ls-output.txt   // >是把标准输出重定向到别的地方,注意仅仅是标准输出. 假如/user/bin这个路径不存在,那么就
    会报路径不存在的错,这时候我们再次 ls -l ls-output.txt就会发现它的大小竟然变成0了,这是因为 > 会覆盖原有结果,由于路径报错了,错误信息是被写入了
    标准错误文件,但是由于报错ls没有生成结果,那么之前的文件自然就被覆盖成空了.   如果是追加的方式的话用 >>
    [me@linuxbox ~]$ > ls-output.txt  // 利用上面这个特性,只使用重定向符，就会清空一个已存在文件的内容或是创建一个新的空文件(记住这个技巧)。
    [me@linuxbox ~]$ ls -l /bin/usr 2> ls-error.txt  //linux没有专门处理标准错误输出的符号,所以有了文件描述符,stderr是2
    [me@linuxbox ~]$ ls -l /bin/usr > ls-output.txt 2>&1   //把标准输出和标准错误输出都输出到同一个文件中,注意这个命令的理解:首先重定向标准输
    出到ls-output.txt文件，然后把标准错误重定向到标准输出 2>&1。1重定向到了文件,2定向到了1,那2自然也就定向到了文件. 这里要注意顺序,先把1和文件连起
    来然后再把2和1连起来,假如顺序反了 2>&1 > xxx.txt,实际上2是会输出到屏幕的,而只有1会定向到文件.(顺序这个有点不太理解后续再关注下)
    [me@linuxbox ~]$ ls -l /bin/usr &> ls-output.txt  // 1和2重定向到文件的另一种写法
    [me@linuxbox ~]$ ls -l /bin/usr 2> /dev/null  // /dev/null 是系统设备,叫做位存储桶，它接受输入但对输入不做处理。可以用这种方式隐瞒错误信息
    
cat 命令读取一个或多个文件，然后复制它们到标准输出.它经常被用来显示简短的文本文件.
    [me@linuxbox ~]$ cat ls-output.txt  //  cat 1.txt 2.txt > new.txt 当接受多个文件当参数时,会把文件拼接起来,所以可以用来拼接文件
    [me@linuxbox ~]$ cat //不加任何参数的cat实际上在等待键盘的输入,回车之后你就可以输入东西了,输入完按ctrl+d告诉系统你的输入结束.然后cat会把你的
    输入(实际上是输入到了键盘对应的文件中)复制到标准输出也就是屏幕上.
    [me@linuxbox ~]$ cat > lazy_dog.txt   // 开始输入,完了之后Ctrl + d结束   //你输入的东西就会输到文件中去
    [me@linuxbox ~]$ cat < lazy_dog.txt   // cat可以从一个文件中读取输入,并把读取的东西展示在屏幕上.
    [me@linuxbox ~]$ ls -l /usr/bin | less //记得less命令吗,它用来查看文件,但更本质的理解是less接受了一个文件的输入并把这些输入输送到了标准
    输出也就是屏幕上,cat也是一个道理,只不过cat对应的是就是标准输入(键盘),而less可以接受文件的输入. 这个命令就是把ls的结果传递给less,可能有人会问,
    直接ls不也是输出到屏幕上么,虽说如此,区别还是有的,less可以进行别的一些操作比如分页之类的,直接的肯定不行吧.

管道符 | 用来传递文本数据流
    [me@linuxbox ~]$ ls /bin /usr/bin | sort | uniq | less  //列出目录下的东西并排序,然后去重,最后展示. 注意unip必须接受有序列表,所以要先排序
    [me@linuxbox ~]$ wc ls-output.txt  // 统计文本输入流中的行数,单词数,字节数,并在屏幕上打印出这些信息 7902 64566 503634 ls-output.txt
    [me@linuxbox ~]$ ls /bin /usr/bin | sort | uniq | wc -l   // 我们可以用wc来对行数进行计数,-l表示只打印行数
    [me@linuxbox ~]$ ls /bin /usr/bin | sort | uniq | grep zip // grep会打印出包含这个匹配项的行 -i忽略大小写,-v打印不匹配的行
    [me@linuxbox ~]$ ls /usr/bin | tail(或者head) -n 5  // tail和head默认只展示10行,可以用参数来自定义展示的行数
    [me@linuxbox ~]$ tail -f /var/log/messages  // -f表示只要这个文件有新内容生成就及时展示,直到你按下Ctrl+ c 
    [me@linuxbox ~]$ ls /usr/bin | tee ls.txt | grep zip //tee可以接受输入并将其输出到标准输出或文件,同时还能让文本流继续向后流动做其他处理
```
```
echo - Display a line of text  //显示一行文本
    [me@linuxbox ~]$ echo ~  // ~有特殊的含义。当它用在一个用户名单词的开头时,意思是指该用户的家目录名，如果没有指定用户名，则是当前用户的家目录
    [me@linuxbox ~]$ echo Number_{1..5}  // Number_1  Number_2  Number_3  Number_4  Number_5
    [me@linuxbox ~]$ echo {Z..A}  // Z Y X W V U T S R Q P O N M L K J I H G F E D C B A   花括号的妙用
    [me@linuxbox Pics]$ mkdir {2007..2009}-0{1..9} {2007..2009}-{10..12} // 2007-01--2007-12 2008-01--2008-12 2009-01--2009-12
    [me@linuxbox ~]$ echo text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER //text /home/me/ls-output.txt a b foo 4 me  参数全部展开
    [me@linuxbox ~]$ echo "text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER"  // text ~/*.txt   {a,b} foo 4 me  双引号参数部分展开
    [me@linuxbox ~]$ echo 'text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER' //ext ~/*.txt  {a,b} $(echo foo) $((2+2)) $USER 单引号展开失效
    [me@linuxbox ~]$ echo "The balance for user $USER is: \$5.00" //可以在双引号中使用转义来心限制展开 .... is: $5.00
    [me@linuxbox ~]$ mv bad\&filename good_filename  //在文件名中也可以使用转义,但是在单引号''中所有的转义都失效
    
clear - Clear the screen  //清空当前屏幕
```
```
/etc/passwd 里面存用户（登录）名、uid、gid、帐号的真实姓名、家目录 和登录 shell。/etc/group 里面存用户组, /etc/shadow 才是存用户密码的信息
    [me@linuxbox ~]$ id  // 显示当前用户的身份信息 uid=500(me) gid=500(me) groups=500(me),uid是用户id,不同的系统可能起始id不一样,gid是组id

rwx属性是有相互关系的,对于文件来说,r读,w写(但不允许这个文件被删除,文件能不能被删是有文件夹的权限决定的),x可执行;但是对于文件夹来说,r允许你列出该文件
夹下的内容(但必须同时拥有可执行权限),w决定该文件夹下是否能新建和删除(但同时也需要具备可执行权限),x允许你能够进入到该文件夹下,所以对于文件夹来说,可执
行权限是r,w权限的基础和前提.如果文件夹没有可执行权限,也就没法进入到该文件夹,那自然也就不存在什么增删改查了.  对于符号链接来说,其权限是虚拟的,真正的
权限由真实的文件来决定.
    [me@linuxbox ~]$ chmod 600 foo.txt  //实际上数字表示法使用的的八进制来表示权限, 八进制中的一些数字分别表示了不同的权限 ,例如
    7 (rwx)，6 (rw-)，5 (r-x)，4 (r--)，和 0 (---),但是后续大家发现直接用十进制的4,2,1来理解和记忆r,w,x更方便,所以就直接说r代表4,w代表2,x代表1;
    [me@linuxbox ~]$ chmod u+x foo.txt  // 字母表示法u+x 。u-x。+x,等价于 a+x,为所有人添加可执行。go=rw 给组和其他人读写权限,如果组或其他人
    已经拥有执行权限,则移除其可执行权限。 u+x,go=rw  多种设定可以用逗号分开。
    [me@linuxbox ~]$ ls -l foo.txt  //ls既用来列出文件夹内容,也用来列出文件本身的详细信息,-rw-rw-r-- 1 me me 0 2008-03-06 14:53 foo.txt

su 允许你假定为另一个用户的身份，并以这个用户的 ID 启动一个新的shell会话，或者是以这个用户的身份来发布一个命令。sudo 命令允许管理员设置一个
叫做/etc/sudoers的配置文件,并在里面定义一些具体命令,普通用户可以不需要管理员密码来执行这些命令(需要自己的密码). sudo是su的进化,安全性更高,也更
便捷和灵活. 总结: su－以另一个用户身份和组ID运行一个shell , sudo－以另一个用户身份执行命令 , 还有一点注意:su和sudo之间的一个重要区别是sudo不会
重新启动一个shell,也不会加载另一个 用户的shell运行环境。这意味者命令不必用单引号引起来。
    [me@linuxbox ~]$ su - user   // -l表示为指定用户启动一个新的shell,也就是加载此用户的shell环境,并且工作目录会更改到这个用户的家目录。这通常
    是我们所需要的。如果不指定用户,就默认是超级用户。-l可以缩写为-，这是经常用到的形式。所以启动超级用户可以这样 su -
    [root@linuxbox ~]#  //当输入 su - ,回车之后，shell提示我们输入超级用户的密码。如果密码正确,会出现当前这个命令提示行,$变成了# ,这表明这个shell
    具有超级用户权限,并且当前工作目录是超级用户的家目录(/root),这时候我们就能执行超级用户所使用的命令。
    [root@linuxbox ~]# exit  //用超级用户的身份完成操作后,exit返回到原来的shell
    [me@linuxbox ~]$ su -c 'ls -l /root'   //用这种方式可以执行单个超级用户命令,不会新启动一个shell.注意命令一定要有单引号. 
    
chown用来更改文件的用户和用户组,但在旧版本中chown只能更改文件所有权,但不能更改用户组的所有权. 所以后续出现了chgrp这个命令,用法和chown基本一样.
    [janet@linuxbox ~]$ sudo chown tony: ~tony/aaa.txt  // 管理员janet想把tony的 /home/tony/aaa.txt文件的权限用户和用户组改成tony用户和tony
    登录系统时所在的用户组.  tony,把文件当前用户改成tony; tony:group1,用户改tony,用户组改group1; :admins,只改组,用户不变; tony:,就是上面这个例子
    
passwd [user] 命令用于更改账户密码,如果你是管理员可以给别的用户更改密码,注意linux有密码验证机制,如果这次修改的密码和执之前的太相似或者太短或者安全
性比较差,那么linux会提示你重新输入一个新的安全的密码.
```
