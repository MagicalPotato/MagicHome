```
[root@linuxprobe ~]# FreeMem=`free -m | grep Mem: | awk '{print $4}'`
[root@linuxprobe ~]# echo $FreeMem

echo "$PASSWD" | passwd --stdin $UNAME &> /dev/null   # passwd用来修改密码,后面跟要改密码的用户名,本来这个命令需要人机交互,但是加了--stdin参数之
后就可以直接接受标准输入,在脚本中这通常很实用. 由于改完密码无论成功与否都会有相应的信息输出,为了让屏幕上的输出简洁干净,所以将输出重定向到 null文件,相当
于是舍弃了输出. 然后在脚本中用 $0 来检查命令是否执行成功.
```
