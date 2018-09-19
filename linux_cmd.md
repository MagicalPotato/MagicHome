#### linux一些命令再次探究
```
pwd - print working directory

cd - Change directory
     [me@linuxbox usr]$ cd ./bin 几乎所有的情况下，你可以省略”./”。它是隐含的
     cd            更改工作目录到你的家目录。
     cd -          更改工作目录到先前的工作目录。
     cd ~user_name 更改工作目录到别的用户的家目录,cd ~bob 会更改工作目录到用户“bob”的家目录。
 
ls - List directory contents
     这个不是缩写 [me@linuxbox bin]$ ls 通常终端提示符会自动显示当前工作目录;
     以 “.” 字符开头的文件名是隐藏文件。ls 命令不能列出它们，用 ls -a 命令就可以;
     任何时候都不要在文件或文件夹名称中使用空格,如果要分隔最好用下划线,这是一条非常有用的建议!!!!
     [me@linuxbox ~]$ ls ~ /usr /bin 列出多个目录下的文件和目录,目录之间用空格隔开,~代表当前用户的家目录,linux会给
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
    cp file1 file2  复制文件file1内容到文件file2中。若file2已经存在,file2所有内容会被file1的内容重写。若file2不存在,则创建file2。
    cp item... directory  //copy一个或多个文件到某个目录下面, -a参数复制父文件的所有,包括属性等等, -r递归 , -i 提示是否覆盖已存在,否则默认覆盖  
    可以使用通配符来匹配文件,当然在地址路径中也可以使用通配符, /home/*soft
    * 匹配任意多个字符（包括零个或一个）
    ? 匹配任意一个字符（不包括零个）
    [characters] 匹配任意一个属于字符集中的字符
    [!characters] 匹配任意一个不是字符集中的字符
    [:alnum:] 匹配任意一个字母或数字  例如: BACKUP.[0-9][0-9][0-9] 以"BACKUP."开头，并紧接着3个数字的文件
    [:alpha:] 匹配任意一个字母  //不要用平常用的那种正则逻辑(比如A-Z匹配字母)来匹配linux文件,用linux自己的匹配规则,否则可能会出错
    [:digit:] 匹配任意一个数字   例如: [![:digit:]]*不以数字开头的文件
    [:lower:] 匹配任意一个小写字母  
    [:upper:] 匹配任意一个大写字母  例如: *[[:lower:]123] 文件名以小写字母结尾，或以 “1”，“2”，或 “3” 结尾的文件

mv – Move/rename files and directories
    mv file1 file2  移动file1到file2。若file2存在,内容会被file1的内容覆盖。 若file2不存在,则创建file2。这两种情况下,file1都不再存在。
    mv file1 file2 dir1  移动 file1 和 file2 到目录 dir1 中。dir1 必须已经存在。
    mv dir1 dir2  移动dir1中的内容到dir2中,无论dir2在不在都没关系

mkdir – Create directories  // mkdir dir1 dir2 dir3

rm – Remove files and directories
    rm -r file1 dir1  删除文件 file1, 目录 dir1，及 dir1 中的内容。
    rm -rf file1 dir1  同上，但是如果文件 file1，或目录 dir1 不存在的话，rm 仍会继续执行。
    一定要小心这个命令,比如 rm *.html,本来想删除html文件,但是如果不小心多写了个空格rm * .html,那么你所有文件都被删了,通配符会匹配可选参数,在使用前一定要先用ls命令对其进行测试;

ln – Create hard and symbolic links
    [me@linuxbox playground]$ ln fun fun-hard  为fun文件创建一个硬链接
    假设文件由两部分组成：文件真正的数据部分 和 该文件的名称部分.当我们创建文件硬链接的时候,实际上是为文件创建了额外的名称,并且这些名称都关联到相同的数据部分。这时系统会分配一连串的磁盘块给你那个文件的索引节点,然后索引节点把刚建的那些名称和自己关联起来,这样真实的文件和所有创建出来的名称(也就是硬链接)就会共享同一个索引节点.当删除了所有的硬链接后,原始文件才会被删除.如果先删了原始文件,那么硬链接就成了坏链接,并在ls时以红色显示.  硬链接有两个缺点,不能跨越物理设备,不能关联目录,只能关联文件。所以出现了软链接,也就是符号链接.软链接克服了硬链接的缺点.
    [me@linuxbox playground]$ ln -s ../fun dir1/fun-sym  为上一级目录下的fun文件在dir1目录下创建一个符号链接也就是软链接,当我们再dir1下用ls命令查看时会看到这个符号链接 lrwxrwxrwx 1 me  me    6 2008-01-15 15:17 fun-sym -> ../fun ,开头字母是l表示就是一个软链接,6是../fun的字符数.对于符号链接，执行的大多数文件操作是针对该符号链接的对象，也就是真实的原始文件,而不是链接本身。 但是当你删除链接的时候，删除链接本身，而不是链接的对象.
```
