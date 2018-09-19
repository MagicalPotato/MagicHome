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
    
less – View file contents  //浏览文件内容 less /etc/passwd  less实际上一个文本读取程序,more的进化版,可以滚动,q键退出
```
