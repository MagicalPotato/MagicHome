```
[root@linuxprobe ~]# FreeMem=`free -m | grep Mem: | awk '{print $4}'`
[root@linuxprobe ~]# echo $FreeMem

echo "$PASSWD" | passwd --stdin $UNAME &> /dev/null   # passwd用来修改密码,后面跟要改密码的用户名,本来这个命令需要人机交互,但是加了--stdin参数之
后就可以直接接受标准输入,在脚本中这通常很实用. 由于改完密码无论成功与否都会有相应的信息输出,为了让屏幕上的输出简洁干净,所以将输出重定向到 null文件,相当
于是舍弃了输出. 然后在脚本中用 $0 来检查命令是否执行成功.

at 23:30   # at -l  #atrm

echo "poweroff" | at 23:30

crontab -e  # -l # -r

whereis rm

对目录文件来说:可读--表示能够读取目录内的文件列表；可写--表示能够在目录内新增、删除、重命名文件；可执行--则表示能够进入该目录;

每个文件都有其归属的所有者和所属组,比如一个用户A,当他创建或传送一个文件后，这个文件就会自动归属于A,也就是说A是这个文件的唯一主人。假如我们需要在一个
部门内部设置一个共享目录,让部门内所有人都可以自由创建,读取,传送,那么就可以给这个共享目录设置 SGID 特殊权限位。这个特殊权限位就是说,让所有在这个目录
中操作的人能临时获取该目录(文件)所属组的权限,这样,每个人创建出来的文件权限就都会归属于该目录本来的组,而不是创建文件的那个人的组.
[root@linuxprobe tmp]# chmod -Rf 777 testdir/  # 先把目录权限设成777,确保每个人都能在该文件夹中操作,-f 若该文件权限无法被更改也不要显示错误讯息.
-R : 若目录下已经存在文件和子目录,则对目前目录下的所有文件和子目录进行相同的权限变更(即以递回的方式逐个变更)
[root@linuxprobe tmp]# chmod -Rf g+s testdir/  # 然后再给该文件夹设置组 SGID 权限,让该文件夹里面所有被操作的文件都继承该目录的组
[root@linuxprobe tmp]# ls -ald testdir/   #当赋予了该目录SGID之后会发现这个目录的用户组权限由rwx变成了rws , ls目录的时候如果不加-d,那么会显示
drwxrwsrwx. 2 root root 6 Feb 11 11:50 testdir/  #该目录下的文件信息,加了意思是把该文件夹像文件一样显示.

SUID的概念和SGID的概念类似,能让别人临时拥有一个文件创建者的权限,但是只是对于二进制程序(程序也是文件)而言.比如/etc/shadow(存密码)这个文件,普通
用户是无法通过编辑器来查看和操作该文件的,但是passwd程序却可以操作该文件,就是因为root给passwd添加了 SUID特殊权限,所以普通用户也能将修改写入. 
chmod u+s passwd   # chmod u-s passwd   # 首先你得是root用户,当然这是系统早就设置好的
[root@linuxprobe ~]# ls -l /bin/passwd     #查看添加了SUID之后的passwd程序的权限,我们会发现passwd的用户权限是rws,而非rwx,这就说明passwd这个
-rwsr-xr-x. 1 root root 27832 Jan 29 2017 /bin/passwd   #程序拥有SUID权限. 如果某个程序原本没有x权限,那么就会变成rwS(注意是大写S)

SBIT权限又是啥呢,当你创建好共享目录之后,所有人都能创建删除,但是此时A是可以删除B创建的文件的,这很容易导致误删,而SBIT就是用来解决这个问题的. 但是有
一点要注意,如果管理员在添加SBIT权限的时候并没有指定-R参数,那么其他用户创建的子目录是不会默认继承权限改权限的,也就是A依然可以删除B在testdir下建的
子目录下的文件,所以B自己要注意自己的子目录权限. 
chmod o+t testdir  # chmod o-t testdir    #改权限的前提是你首先得是这个目录的创建人,然后你才有资格给该目录赋予这个权限
[linuxprobe@linuxprobe tmp]$ ls -ald /tmp  #这时候会看到该目录的其他用户权限是rwt而非rwx ,如果原本目录没有x权限则是rwT(大写T)
drwxrwxrwt. 17 root root 4096 Feb 11 13:03 /tmp 

chown root:bin test  # 改属性   chmod是改权限
chattr +a a.sh #lsattr a.sh  #chattr -a a.sh    # change attribute   #隐藏权限

#一般权限、特殊权限、隐藏权限 这三种权限都是对于某一类用户或者用户组而言,而文件的访问控制列表（ACL）则是针对特定的用户或者用户组的一种权限控制
ls -ld /root # dr-xrwx---. 14 root root 4096 May 4 2017 /root  #没加acl权限,ls后看到权限列最后是一个点,加了acl权限可以看到最后那个点变
ls -ld /root # dr-xrwx---+ 14 root root 4096 May 4 2017 /root  #成了一个加号+,就说明这个目录或文件具有针对某个用户或者组特别设置的一些权限
setfacl -Rm u:abc:rwx /root  #给abc用户赋予/root目录rwx权限(普通用户原本无法访问/root),R表示递归,m表示普通文件; u:用户:权限, g:组名:权限
getfacl /root  # 当得知某个文件或文件夹有acl权限的时候就可以通过getfacl来查看其acl权限

su - abc # 由于直接使用root用户有时候会造成一些麻烦,所以大多时候我们都是使用非root用户来操作linux系统.但是如果你有root用户的权限,那么你可以
随时切换到root或者任何其他用户的账号. su命令可以在不退出当前shell的情况下切换到其他用户,在su和用户之间有个-,意思是连同环境变量也一并切成该用户的,
相当于彻底切换,强烈建议切换操作都加中划线. 
root ALL=(ALL) ALL # 尽管普通用户可以通过su切换到root,但很可能输密码时导致泄露,所以有了sudo这个命令.用visudo命令编辑/etc/sudoers文件,仿照此行
abc ALL=(ALL) ALL  # 添加用户abc,这样abc就可以使用sudo来执行命令了. abc用sudo执行命令时要验证自己的密码而非root用户的密码.可以通过配置取消.注意,
如果赋予了一个用户 ALL权限,那么他几乎相当于root用户了,这样依然不太好,所以在配置的时候可以用具体命令的绝对路径来代替最后的那个ALL, 用whereis找到
命令的绝对路径然后配置就行 abc ALL=(ALL) a/b/c, d/e/f 不同命令逗号隔开;  谁可以使用 允许使用的主机=(以谁的身份) 可执行的命令; sudo -l   
```

* 一块完整的硬盘,我们在使用之前首先要对其进行合理的资源分配,这样才能达到最优的使用效率.对于Windows系统来说,你的电脑通常只有一块硬盘,但是在你的文件
系统中却存在C,D,E,F四个盘.这是因为windows将很多底层的操作都给我们做好了,这些操作包括但不限于:对硬盘分区,对分区格式化,还有将分区挂载到文件系统等.对于
linux系统,它的系统是很开放的,所以这些操作都需要我们自己去手动执行. 硬盘可以只分一个区,通常分为多个区只是为了方便使用和提高效率.  接入系统的设备通常
用abcdef等字母来确定其顺序,比如硬盘设备,sda,sdb,sdc都表示硬盘设备,然后我们可以对其进行分区操作,分的区用数字123...表示,一块硬盘比如sda1,sda2,sda3,
sda4,意思就是说我把sda这个硬盘分成了四个区.一块硬盘最多只能分成四个区,每个区管理一块容量.但是我们经常可以看到有sda5,sda6分区,是因为用户用的是三个
主分区+一个拓展分区这种分法,拓展区的node块不是指向容量,而是指向逻辑分区的node块,逻辑分区不是虚拟分区,而是真实的分区,只是这是人为规定这样的,无论是主
分区还是逻辑分区他们都是在同一块硬盘上,所以使用都没啥影响,只是理解上的差异. 分好区后要对该区进行格式化,格式化的目的是为了给你的这个区装一个文件系统
模板,linux系统有特别多的文件系统,不同的文件系统对文件的管理方式肯定不一样,所以你需要将其格式化成你想要的文件系统. 格式化完成之后,最后就是挂载,把你分
好的这个区挂到某个目录就可以正常使用了.

* 硬盘插入linux机器之后,会自动对其进行识别,一般硬盘都是sda,adb这个顺序往下走,对应的硬盘文件都统一放在/dev下. 假设你刚新连了一块硬盘到机器上,机器把
该硬盘抽象成了sdb文件,现在我们来对该硬盘进行分区.
```
fdisk /dev/sdb  #此时会出现一堆提示信息,不用管
Command (m for help): p   #直接输入p来查看当前这个硬盘的信息
   Device Boot Start End Blocks Id System   #这是信息的最后一行,它用来表示该硬盘的分区信息的信息头,如果有分区,就会显示分区信息.
Command (m for help): n   #信息显示完你的光标还是在命令行,直接输入n来添加一个新分区.会出现如下信息:
   p primary (0 primary, 0 extended, 4 free)  # 输入p创建一个主分区
   e extended  #输入e创建一个逻辑分区
Command (m for help): n  #直接输入p创建主分区
Partition number (1-4, default 1): 1   #分区都是按照数字来排序的,我们要创建第一个主分区,那就直接输入1
First sector (2048-41943039, default 2048):    #这里是指定分区的起始扇区,用默认就好,不需要输入任何东西,直接回车
   Using default value 2048   #系统会自动计算出最靠前的空闲扇区的位置,这里就表示当前分区的默认起始位置
Last sector, +sectors or +size{K,M,G} (2048-41943039, default 41943039): +2G   #这里是要指定扇区结束位置.也可直接指定大小,+500M,+2G等
Command (m for help): p   # 设置完大小时候就又回到了命令行,我们输入p来查看一下现在的分区情况,最后两行就变成这样了:
   Device Boot Start   End    Blocks   Id  System
   /dev/sdb1   2048  4196351  2097152  83  Linux    #这就是你新加的分区,可能显示/dev/sdb 也可能直接显示sdb1,都一样
Command (m for help): w   #这时候还没完,我们必须输入w然后回车,这些信息才真正生效,否则是没有的.

[root@linuxprobe ]# file /dev/sdb1  #使用file命令来查看你刚分好的那个区的类型
   /dev/sdb1: cannot open (No such file or directory)  #如果显示信息是这样,那说明显示信息还没有同步到内核
[root@linuxprobe ]# partprobe   #那你就手动执行partprobe命令手动同步,一遍不行就两遍,两遍还不行就重启机器就好了.

[root@linuxprobe ~]# mkfs  #分完区之后要对该分区进行格式化,输入mkfs命令之后敲两下tab键,会出现所有文件系统的格式化命令
    mkfs   mkfs.cramfs   mkfs.ext3   mkfs.fat   mkfs.msdos   mkfs.xfs
    mkfs.btrfs   mkfs.ext2   mkfs.ext4   mkfs.minix   mkfs.vfat
[root@linuxprobe ~]# mkfs.xfs /dev/sdb1  #用xfs的文件系统来格式化刚刚的分区,格式化就这么简单,直接就完成了
[root@linuxprobe ~]# mkdir /newFS  #创建一个挂载目录
[root@linuxprobe ~]# mount /dev/sdb1 /newFS/   #然后将刚刚的分区挂载到文件夹上
[root@linuxprobe ~]# df -h  #然后使用df命令来查看当前系统的所有挂载信息,有一大堆呢,截取最后两条
    /dev/sda1 497M 119M 379M 24% /boot
    /dev/sdb1 2.0G 33M 2.0G 2% /newFS   #这就是你刚挂的
    
如果是新的外部硬件挂到文件夹上,每次重新启动挂载都是会失效的,得重新挂载,为了让其开机自动挂或者插上自动挂,那么需要将挂载信息直接写入到/etc/fstab文件中,
打开该文件会看到一堆已经存在的挂载信息,然后你按照格式把你新分的区挂上去就行了
    /dev/mapper   /rhel-swap      swap swap      defaults   0  0
    /dev/cdrom    /media/cdrom    iso9660        defaults   0  0
    /dev/sdb1     /newFS           xfs          defaults    0(是否备份,0否1是)  0(是否进行磁盘检查,0否1是)
    
[root@linuxprobe ~]# umount /dev/sdb1   #解挂
```
