* Shell指的是shell脚本编程. 大多linux默认执行shell脚本的Shell程序是bash  #!/bin/sh同样也可以改为 #!/bin/bash  其中#! 是一个约定的标记，
它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell程序。
* echo "xxxxx"  echo用于向窗口输出文本
* 执行脚本有两种方式,一种是让这个脚本文件具有可执行权限,另一种是把脚本文件当成解释器的参数.  第一种:首先cd到你的当前目录,给你当前目录下写的脚本添加可
执行权限 chmod +x ./test.sh ,当前目录是./ 如果不加./会默认去系统的path下找这个脚本,当然是找不到的. 权限加了之后就可以执行了 ./test.sh ,直接执行就是
在命令行输入脚本名称.  第二种: 直接把脚本当成参数传给解释器. /bin/sh test.sh
* Shell语言定义变量时,变量名前不加美元符号$,但是在php语言中需要. your_name="runoob.com" ,注意**shell中的变量和等号之间不能有空格**.可以在代码中使用命令语句for file in ``` `ls /etc` ```(注意这个不是单引号,是反引号,反引号中的内容是一个命令,比如你在反引号中写了data,那么输出的就是当前时间而不是data这个字符串) 或者for file in $(ls /etc) 该语句可以将/etc下目录的目录名称列出来
* 使用一个定义过的变量，只要在变量名前面加美元符号即可，如：your_name="qinjx" , echo $your_name 或者echo ${your_name}. 推荐在使用的时候给所有变量加上花括号，这是个好的编程习惯.
* readonly xxx 将xxx这个变量设置成只读变量  unset xxx 删除xxx变量,但不能删除只读变量,变量被删除后不能再次使用. 
* shell中的字符串可以使用单双引号,但稍有区别. 单引号中不能使用转义,哪怕使用了反斜杠也不行,也不会输出变量,反正就是啥都不能用,只会原样输出其中的内容.而双引号没有这个限制  string="abchhjkhjd",echo ${#string},加了#相当于是求字符串长度. echo ${string:1:4},从第二个字符开始截取四个字符
* Shell 中，用括号来表示数组，数组元素用"空格"符号分割开. array_name=(value0 value1 value2 value3).  也可以用下标来定义 array_name[0]=value0 .......array_name[n]=value0 可以不使用连续的下标，而且下标的范围没有限制.获取数组值的一般格式是使用下标 valuen=${array_name[下标]}, 使用 @ 符号可以获取数组中的所有元素 echo ${array_name[@]}, 取得数组元素的个数 length=${#array_name[@]}或者length=${#array_name[`*`]}
* shell中没有多行注释,只有单行注释 ,使用#号 . 
* echo "It is a test" > myfile 将结果输出到myfile文件  echo后加-e参数说明之后的东西将开始转义, \c的意思是不换行.  read -p "请输入一段文字:" -n 6 -t 5 -s password, echo -e "\npassword is $password" ,把这两条命令写入一个shell脚本中,然后执行该脚本,会输出如下东西: 请输入一段文字: 下一行:password is asdfgh .由此可以看出read的用法: -p参数是显示提示字符串, -n是限制输入文本的长度, -t是限制时间5分钟 ,-s是输入隐藏内容. 另外,read 命令一个一个词组地接收输入的参数，每个词组需要使用空格进行分隔；如果输入的词组个数大于需要的参数个数，则多出的词组将被作为整体为最后一个参数接收。
* printf 是echo的加强版,可移植性更好.但是printf打印出来之后不默认加换行,需要手动加.echo "ddd",那么ddd会默认在下一行显示,但是如果printf的话则是直接跟在后面,要换行的换需要手动添加换行的转义\n ,  printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 ,%s %c %d %f都是格式替代符%-10s 指一个宽度为10个字符（-表示左对齐，没有则表示右对齐），任何字符都会被显示在10个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。%-4.2f 指格式化为小数，其中.2指保留2位小数,注意4和.2是分开的,并不是4.2, 格式替代符%d %s %c %f详解:d: 对应位置参数必须是十进制整数，否则报错！,s: 对应位置参数必须是字符串或者字符型，否则报错！,c: Char--对应位置参数必须是字符串或者字符型，如果是个字符串会自动截取字符串的第一个字符当做输出值,否则报错！, f: Float 浮点 -- 对应位置参数必须是数字型，否则报错！
* 数值的比较用的是特殊的字符:-eq等于(可以用==),-ne不等于,-gt大于,-ge大于等于,-lt小于,-le小于等于,test用于测试条件是否成立,方框[]用于基本运算,不用[]表示是字符串,num1=100,num2=200, 那么比较两个值的方式是: test $[num1] -gt $[num2],这个例子中用了方框才表示num1和num2是数字,否则那么会把他们当成字符串来处理.result=$[5+9], 而字符串的比较用的是=好和!=号,num1="ru1noob",num2="xxx",test $num1=$num2,注意在shell中的所有地方等号左右两边都别加空格.和别的编程语言不一样,不一样,不一样!!!!  对文件的测试也是用特殊符号,if test -e ./bash,-e表示文件存在,-r文件存在且可读,-w,-x同理,-d存在且为目录,其他自查,另外，Shell还提供了与( -a )、或( -o )、非( ! )三个逻辑操作符用于将测试条件连接起来，其优先级为："!"最高，"-a"次之，"-o"最低。例如这个或的条件:if test -e ./notFile -o -e ./bash
* **无论是什么语言,最终都是对数据的处理,都离不开基本变量,条件,循环,判断,这些都是一门语言最基本的元素,而那些乱七八糟的方法或数据结构之类的,亦或者各种算法,最终都是为了数据服务._代码的本质就是完成数据的处理和传递_.所以,任何语言,无论语法如何变,其本质都不会改变**
* shell中的for,while,case:
```
for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done
--------------------------------
#!/bin/bash
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"      # Bash let 命令，它用于执行一个或多个表达式，变量计算中不需要加上 $ 来表示变量
done
--------------------------------
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in              # 取值后面必须为单词in，每一模式必须以右括号结束。
    1)  echo '你选择了 1'  # 取值可以为变量或常数。匹配发现取值符合某一模式后，其间所有命令开始执行直至 遇到两个分号;;
    ;;
    2)  echo '你选择了 2'  # 取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式
    ;;
    3)  echo '你选择了 3'  # case的语法和C family语言差别很大，它需要一个esac（就是case反过来）作为结束标记，
    ;;
    4)  echo '你选择了 4'    #每个case分支用右圆括号，用两个分号表示break。
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'  # 如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令。 
    ;;
esac
----------------------------------
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break                       #    break用于跳出死循环!continue跳出当前循环!
        ;;
    esac
done
---------------------------------
#!/bin/bash
for((i=1;i<=5;i++));do                 # 通常情况下 shell 变量调用需要加 $,但是 for 的 (()) 中不需要
    echo "这是第 $i 次调用";
done;
```
* shell中的函数
```
funWithReturn(){        #所有函数在使用前必须定义。这意味着必须将函数放在脚本开始部分，直至shell解释器首次发现它时，才可以使用
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "      #调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，
    read aNum                    #例如，$1表示第一个参数，$2表示第二个参数...
    echo "输入第二个数字: "      #  $10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数
    read anotherNum             # 所以为所有的参数加大括号是正规的编程
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"        #  函数返回值在调用该函数后通过 $? 来获得,调用函数仅使用其函数名即可。 
```
* 重定向
```
command > file 将输出重定向到 file。  # file内的已经存在的内容将被新内容替代。如果要将新内容添加在文件末尾，请使用>>操作符
command < file将输入重定向到 file。也就是从file中读入东西当做命令的参数.
command >> file将输出以追加的方式重定向到 file。
n > file将文件描述符为 n 的文件重定向到 file。      # 文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）
n >> file将文件描述符为 n 的文件以追加的方式重定向到 file。
n >& m将输出文件 m 和 n 合并。
n <& m将输入文件 m 和 n 合并。
<< tag将开始标记 tag 和结束标记 tag 之间的内容作为输入。

$ who > users  # who 命令将完整的输出重定向在用户文件中users, 执行后，并没有在终端输出信息，因为输出已被从终端重定向到文件
$ cat users  #可以使用cat命令来查看文件中的内容
command1 < infile > outfile  # 同时替换输入和输出，执行command1，从文件infile读取内容，然后将输出写入到outfile中。

$ command > /dev/null  # 如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null
/dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 /dev/null 文件非常有用，
将命令的输出重定向到它，会起到"禁止输出"的效果。
如果希望屏蔽 stdout 和 stderr，可以这样写： $ command > /dev/null 2>&1
```

* 如果有两个脚本a.sh和b.sh,那么在b中引用a可以这样写, . ./a.sh , 注意第一个点表示引用文件,之后有个空格,然后./a.sh代表当前目录下你的a.sh文件,
还有另一种引用方式, source ./a.sh,注意source和目录文件之间也是有空格的.



##### shell脚本重新学习

* 在linux下运行一个脚本要具备三点: 1编写脚本 2给脚本可执行权限 3把脚本放到shell能找到的地方.
```
[me@linuxbox ~]$ ls -l hello.sh
-rw-r--r-- 1  me    me      63  2009-03-07 10:10 hello.sh
[me@linuxbox ~]$ chmod 755 hello.sh    # 755的脚本每个人都能执行,700的脚本只有文件所有者能够执行。
[me@linuxbox ~]$ ls -l hello.sh        # 还有一点要注意:为了能够执行脚本，脚本必须是可读的。
-rwxr-xr-x 1  me    me      63  2009-03-07 10:10 hello.sh

[me@linuxbox ~]$ ./hello.sh   #执行脚本必须要明确指定路径,否则shell会去这几个路径下找,找不到就会报错.
[me@linuxbox ~]$ echo $PATH   # /home/me/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games
export PATH=~/bin:"$PATH"  # 大多linux会将/home/me/bin配到PATH中,你在可以在自己的家目录下建一个bin目录然后把脚本放进去就行.如果没配可这样配.
[me@linuxbox ~]$ . .bashrc # 修改之后让shell重新读取这个 .bashrc文件以应用修改。 整个命令相当于source .bashrc

/home/用户名/bin(~/bin)   # 用户家目录通常用来存放用户个人所用脚本。
/usr/local/bin           #如果想让系统中的每个用户都可以使用该脚本,则应将其放在这个路径下。
/usr/local/sbin          # 管理员的脚本经常放在这个目录
/usr/local              # 本地支持的软件,不管是脚本还是编译过的程序,都应该放到这个目录下
```

* 养成良好的编程习惯,无论在编写脚本还是写代码中,用大写来表示常亮,而用小写来表示变量.

```
[me@linuxbox ~]$ a="thismytestfile"  #定义一个变量并赋值成 thismytestfile
[me@linuxbox ~]$ touch $a   #以a这个变量的值来创建一个文件
[me@linuxbox ~]$ mv $a $a1  #本意是想给刚创建的这个文件重命名,直接写文件名肯定繁琐,所以想用$a后面直接接1的这中形式,但是这样是无法把$a的值和1拼起来
的,shell会把$a1解释成一个新的变量,新的变量没定义就是空,那么就会报错. 想要拼接的话应该这样${a}1,这样shell就只会将{}中的a解释成变量而1不会.

来看一个脚本实例:
#!/bin/bash
# Script to retrieve a file via FTP
FTP_SERVER=ftp.nl.debian.org
FTP_PATH=/debian/dists/lenny/main/installer-i386/current/images/cdrom
REMOTE_FILE=debian-cd_info.tar.gz
ftp -n <<- _EOF_          #脚本中可以用 commond << 标记符 的模式来屏蔽shell中一些单双引号的作用,正常情况下引号中表示一个字符串,但是在这中模式中
    open $FTP_SERVER      #引号会被原样输出,这个特性在一些场景中很有用. 注意commond << 标记符 必须在同一行且, <<-表示忽略后续行开头的tab,这样
    user anonymous me@linuxbox   #可以提高可读性.
    cd $FTP_PATH
    hash
    get $REMOTE_FILE
    bye
_EOF_           #遇到了_EOF_表示这个命令结束了. 标记符可以是任意定义的符号.
```

```
1     #!/bin/bash
2     function funct {    # name () {...}  定义函数的两种方式,要么是用function申明,要么是直接写名字后面接圆括号()
3       echo "Step 2"     # 为了使函数调用被识别出是shell函数,而不是外部程序,在脚本中shell函数定义必须出现在函数调用之前。     
4       return            # 也就是说函数要定义在脚本的上面,但是真正执行的时候并不会先从函数开始执行,而是先从可以执行的地方开始
5     }                   # return会结束函数调用并返回脚本执行权给调用者. 一个函数必须至少包含一条命令,当然单独的return就满足要求.
6     echo "Step 1"       # 这个脚本就先会从这里开始执行.
7     funct               # 函数是为了拆分大模块,并不一定非要用函数,你也可以把所有的东西堆在一起写,你开心就好.
8     echo "Step 3"

foo=0  #全局变量
do () {
local foo  # 在函数中使用local关键字来定义一个局部变量,局部变量和全局变量毫无关系互不影响.
foo=2
}
```
* If 语句真正做的事情是检验命令执行成功或失败,而非判断条件是否成立,判断条件是否成立的是test.所以if常常和test一起使用.中括号是test的另一种写法. 
```
[me@linuxbox ~]$ ls -d /usr/bin
[me@linuxbox ~]$ echo $?   # 结果是0, $?是一个特殊的符号,这个符号会返回一个状态码用来检查上一个命令是否执行成功.0成功,其他都是失败.
[me@linuxbox ~]$ if false; true; then echo "It's true."; fi  # It's true.   if语句如果有多个条件只会执行最后一个.

test expression 等价于 [ expression ],其执行结果是 true或false。结果真时test命令返回一个零退出状态,假时退出状态为1。一下分别是文件测试条件,
字符串测试条件和整型测试条件:
file1 -ef file2	 #1和2拥有相同的索引号（通过硬链接两个文件名指向相同的文件）。file1 -nt file2	 #1比2新。  file1 -ot file #1比2旧。
-b file  # file存在且是个块（设备）文件; -c file # 存且是字符（设备）文件; -d file #存且是目录; -e file #存; -f file # 存且是普通文件。
-L file  # 存且是符号链接; -s file #存且长度大于零; -w file #存且可写;-x file #存且可执行. -r 存且可读.
str # str不为null; -n str # str长度大于零(不为空)。-z str #为空。 =或== 相等; != 不相等; str1>str2 #1排在2之前, < 是之后; 注意:与单方括号[]
连用候,>和<必须用引号引起来(或者是用反斜杠转义)否则它们会被解释为重定向操作符造成潜在的破坏结果,而双[[]]则不需要.
integer1 -eq integer2 #等于; -ne 不等于; -le 小于或等于(less or equel); -lt 小于(less than); -ge 大于或等于; -gt 大于(greater than)

#!/bin/bash
INT=-5
if [[ "$INT" =~ ^-?[0-9]+$ ]]; then   # 在[[ ]]中$本来可以省略,可以直接使用变量,但是一旦字符串变量是空值则会导致错误,所以给变量加了引号,引号是
    if [ $INT -eq 0 ]; then     #为了防止空值,若变量空了,引号里就是空串,至少程序不会挂. 但是加了引号之后$符号就不能省略了,不然会把变量当成字符串.
        echo "INT is zero."   # [[]]中可以通过使用 =~ 来匹配正则; == 来匹配类型,比如 foo.txt == foo.*
    else
        if [ $INT -lt 0 ]; then echo "INT is negative." else echo "INT is positive." fi  #negative负数,positive正数
        if [ $((INT % 2)) -eq 0 ]; then echo "INT is even." else echo "INT is odd." fi    # even奇数,odd偶数
    fi
else echo "INT is not an integer." >&2
    exit 1
fi

if [ $((INT % 2)) -eq 0 ] 等价于 if (( ((INT % 2)) == 0))  # (( ))也是一种特殊的写法,用来执行算术运算。注意if只是判断执行结果是不是0或者
其他值,if并不判断条件是否成立. 在(( ))中我们也可以直接使用<和>,以及==,而且可以够直接通过变量名称识别变量而不需要加$
if [[ INT -ge MIN_VAL && INT -le MAX_VAL ]]  #如果只是当成数字使用,那么就没有加引号的必要,而且[[ ]]中也可以不用$就能直接用变量
if [ ! \( $INT -ge $MIN_VAL -a $INT -le $MAX_VAL \) ] #单()用来分组 ,对于单的()或者[],在其中使用括号,>,<,==都需要转义,所以在单[]中都是用替代
符号, -a等价于且&& , -o等价于或||, !就是非.且单[]中是需啊$的,只有双[]才没有这些限制. 实际中[[ ]]更常用且更易于编码
if [[ $(id -u) -eq 0 ]]  # 直接用$()用于获取一个命令的执行结果,就相当于是在bash输入命令一样,为了不混淆,所以就加了个括号
h
cmd1 && cmd2 和 cmd1 || cmd2  # &&操作符先执行cmd1, 1执行成功后才执行2。对于||,先执行1,且只有1失败后,才执行2。
```

* __评估__
```
#!/bin/bash
FILE=/etc/passwd
read -p "Enter a user name > " user_name  #read命令用来从键盘获取输入,-p指定提示信息,提示你输入一个用户名,你的输入会赋值给user_name这个变量
file_info=$(grep "^$user_name:" $FILE)  #这一行用正则来匹配文件中的一行
if [ -n "$file_info" ]; then   # 如果匹配到的这一行不为空,-n表示长度大于0
IFS=":" read user pw uid gid name home shell <<< "$file_info"   #这是一种特殊的用法:Shell 允许在一个命令之前给一个或多个变量赋值。这些赋值会
    echo "User = '$user'"   #暂时改变之后的命令的环境变量。什么意思呢:原本read读取输入的时候是按照一个空格或一个tab或一个换行来分割输入的文本串,
    echo "UID = '$uid'"   #但是你可以通过IFS这种方式将分割符改成别的,比如这个例子就是把分隔符改成了冒号":",最后从file文件中读取了文本,重定向的时候
    echo "GID = '$gid'"  #用了三个<<<,而不写成 echo "$file_info" | IFS=":" read user pw uid gid name home shell,是因为管道符实际上是重新开了
    echo "Full Name = '$name'"   #一个子shell,管道符后面的内容都会到子shell中去执行,但是子shell执行完后所有东西都会销毁,所以根本得不到结果.
    echo "Home Dir. = '$home'"
    echo "Shell = '$shell'"
else
    echo "No such user '$user_name'" >&2
    exit 1
fi
这个赋值方式要额外说一下,刚这一行等价于下面这几行:在这种场景中改变环境变量只是暂时的,也就是说原本环境变量中分隔符就是空格tab换行,我暂时为此次的read
命令改成冒号":",但是这个read命令完了之后我依然将其赋回原样了. 
OLD_IFS="$IFS"
IFS=":"
read user pw uid gid name home shell <<< "$file_info"
IFS="$OLD_IFS"

invalid_input () { echo "Invalid input '$REPLY'" >&2 exit 1 } #错误校验方法
read -p "Enter a single item > "  
[[ -z $REPLY ]] && invalid_input  # 控制操作符&&在这里不要理解成并且的意思,而是上一个命令执行成功则这个就会执行,在这里这是一种校验措施
(( $(echo $REPLY | wc -w) > 1 )) && invalid_input  # 双括号(( ))是用来评估结果是0还是非0,0是成功,非0是失败.可以说它是一个简版的if.
```
```
#!/bin/bash
while read distro version release; do       # while cmd;do cmd; done   可以用until来代替while,两种方式都可以
    printf "Distro: %s\tVersion: %s\tReleased: %s\n" \
        $distro \
        $version \
        $release
done < distros.txt    #while是可以接受标准输入的

#!/bin/bash   #当然也能接受标准输入,但是由于管道是单独打开一个子shell,所以循环和读取都是在子shell中运行,子shell运行完之后会销毁所有东西,所以在
sort -k 1,1 -k 2n distros.txt | while read distro version release; do     #当前shell中我们是看不到任何输出的
    printf "Distro: %s\tVersion: %s\tReleased: %s\n" \
        $distro \
        $version \
        $release
done
```
* 一个脚本的进化
```
cd $dir_name rm *  # 若name不存在,则cd命令会失败,但是rm会继续执行删除当前目录
cd $dir_name && rm * # 加了预防措施只有字cd执行成功的时候才执行rm,但是有可能目录名称是空,cd空是可以执行的,就是用户的家目录,这很危险.
[[ -d $dir_name ]] && cd $dir_name && rm *  # 这种已经很不错了,但是发生错误能够打印信息通常是个很好的做法,所以最终有了下面这个版本:

echo "number=$number" #通常在脚本的某些地方打印参数是一个不错的DEBUG技巧
if [[ -d $dir_name ]]; then  # 这才是一个标准的执行流程
    if cd $dir_name; then
        rm *    # echo rm *  # 加了echo只会打印出命令和其参数列表,这在测试脚本的时候很有用.
    else
        echo "cannot cd to '$dir_name'" >&2
        exit 1   #运行到异常之后我们手动设置了退出状态为1,说明运行失败
    fi
else
    echo "no such directory: '$dir_name'" >&2
    exit 1
fi
```
```
#!/bin/bash
read -p "enter word > "
case $REPLY in   # case的in不是在里面的意思,而是将要匹配某个模式的意思,右括号是结束标志
    a)             echo "匹配字符a" ;;
    [[:alpha:]])   echo "匹配一个字母" ;;
    [ABC][0-9])    echo "xxx" ;;
    ???)           echo "xxx" ;;
    *.txt)         echo "xxx" ;;
    *)             echo "xxx" ;;
esac

case $REPLY in  #可以使用|来表示选择,这通常在用字母作选择且不分大小写时很有用.
q|Q) echo "Program terminated."  #只要匹配并且进入了分支,那么分支里面可以继续写if,while或者更多的命令等
     exit ;;
a|A) echo "Hostname: $HOSTNAME"
     uptime ;;
esac

#!/bin/bash
read -n 1 -p "Type a character > "
case $REPLY in  
    [[:upper:]])    echo "'$REPLY' is upper case." ;;&    # 传统的 ;;结尾的脚本要求你的输入只能匹配一个分支,且执行完该分支就会退出.否则会报错.
    [[:lower:]])    echo "'$REPLY' is lower case." ;;&    # 若用 ;;& 来结束脚本那么只要满足条件的分支都会进,且不会报错.
    [[:alpha:]])    echo "'$REPLY' is alphabetic." ;;&    # 左边那些待匹配模式叫POSIX字符集(正则字符集)   alphabetic 字母字符
    [[:digit:]])    echo "'$REPLY' is a digit." ;;&       # 数字
    [[:graph:]])    echo "'$REPLY' is a visible character." ;;&     # visible character 可见字符,也就是非空格
    [[:punct:]])    echo "'$REPLY' is a punctuation symbol." ;;&    # punctuation symbol 标点符号
    [[:space:]])    echo "'$REPLY' is a whitespace character." ;;& 
    [[:xdigit:]])   echo "'$REPLY' is a hexadecimal digit." ;;&     #十六进制数字
esac
```
```
[me@linuxbox ~]$ for i in A B C D; do echo $i; done
[me@linuxbox ~]$ for i in {A..D}; do echo $i; done   #花括号展开
[me@linuxbox ~]$ for i in distros*.txt; do echo $i; done  #路径名展开

#!/bin/bash
# longest-word : find longest string in a file
while [[ -n $1 ]]; do   # $1-$10通常用来表示一个命令的参数,以空格分割. $#通常用来表示命令的所有参数数量
    if [[ -r $1 ]]; then  # -r表示该文件是否可读
        max_word=       # 在这里像正常编程语言那里定义最长单词和单词数量
        max_len=0
        for i in $(strings $1); do   #strings命令将第一个文件中的所有单词组装成一个可迭代的列表之类的东西以供循环
            len=$(echo $i | wc -c)  #打印第一个文件中的所有单词,并对每一个单词进行计数并赋值给len
            if (( len > max_len )); then  #对len进行算数比较
                max_len=$len   #赋值操作,注意不能像别的语言那样直接写变量名称,要用$来引用变量
                max_word=$i   #注意是把$i赋值,不是$1
            fi
        done
        echo "$1: '$max_word' ($max_len characters)"
    fi
    shift   # shift命令用来提升参数位置,假如$1并不是一个可读文件,那么所有参数都会往前移动一位,$2的参数移到$1,$3到$2,以此类推...
done

for i; do   #这是另一种写法,在这个写法中省略了while和其判断条件,直接用for循环,用i来代替位置参数进行操作
    ......
    ......  #如果直接用for进行位置参数的循环,那么shift命令也就不需要了
done

for (( i=0; i<5; i=i+1 )); do   #for循环的另外一种写法,直接用双括号
    echo $i
done
```
```
$a实际上是参数展开,而不能理解成对a变量的引用,它等价于 ${a},但是${a}通常用来会产生歧义的地方,比如:
[me@linuxbox ~]$ a="foo"
[me@linuxbox ~]$ echo "$a_file"  # 在这里我们本来是想得到foo_file,但是shell会将a_file当成一个变量展开. 使用${a}_file就可以.
通常我们用$1-$9来表示位置参数,如果参数多于9个,那么可以使用${10},${55},${101}....这种方式
${param:-word}  #若param没值或为空,展开结果是word的值。若param不为空,则展开结果是param的值.注意,位置参数不能以这种方式赋值.
${param:?word}  #若parm没值或为空,则脚本会错误退出,并将word的内容发送都标准错误. 否则展开结果是param 
${param:+word}  #param为空则展开就是空,不为空则用word替换param并展开,但是实际上param的值并没有真的被替换,只是暂时被替换.
${!ABC*} 或 ${!ABC@} #返回以所有以ABC开头的变量名称
${#param}  #若param是个str,则是求字符串的长度,若param是@或者*,则展开成位置参数的个数,如果我们用参数展开形式len=${#j}改写
上个脚本中的len=$(echo $j | wc -c)这行,那么整个脚本效率会有质的飞跃,大概几百倍的效率.从三秒到0.06秒
${param:offset} 和 ${param:offset:length}  #提取字符串,含offset,空格不计数,offset可为负如-1,表示从后往前,且-1前必须有空格防止和上面的命令混淆.
${param#pattern} 和 ${param##pattern}  #清除字符串param开头的子字符串,且子串要匹配提供的正则. 一个#最短匹配,另个#最长匹配; %和%%表示从后往前进行.
${param/pattern/str} #用str替换匹配到的字符串,只替换第一次匹配到的
${param//pattern/str} #替换所有的
${param/#pattern/str} #替换所有且要求匹配项要出现在字符串的开头
${param/%pattern/str} # 替换所有要求匹配项出现在结尾

#!/bin/bash
declare -u A   # 使用declare来声明变量, -u(upper)和-l(lower)分别声明纯大写或纯小写变量,也就是赋值给变量的东西都会被转换成响应的格式
declare -l B
if [[ $1 ]]; then
    A="$1"
    B="$1"
    echo $A $B    
fi        #下面是使用参数展开的方式来对字符进行统一处理,${}本来就是规范的参数引用方式,所以${1,,}这种写法没啥值得疑惑的,就是把第一个参数展开成小写
${parameter,,}	把 parameter 的值全部展开成小写字母。
${parameter,}	仅仅把 parameter 的第一个字符展开成小写字母。
${parameter^^}	把 parameter 的值全部转换成大写字母。
${parameter^}	仅仅把 parameter 的第一个字符转换成大写字母（首字母大写）。

$((expression)) #现在可以不需要在疑惑了,$加上双括号就是算数展开的固定写法,在if,while,for等结构中双括号前的$是不需要的,但是在赋值操作或者一些echo
等单元操作中这就是固定的写法,当然双括号里面的数字如果没有特殊表明那么就会被当成是十进制,如果你要用其他进制那么可以在数字前加上识别符号,比如,0aaa表示
八进制数字,0xaaa表示十六进制; 当然也可以自定义比如: 2#aaa二进制,3#aaa三进制,5#aaa五进制
[me@linuxbox ~]$ echo $(( 5 / 2 ))  #有一点要注意就是shell的运算仅且只能操作整数,所以结果自然也都是整数.如果是浮点之类的需要用别的程序,典型的是bc
[me@linuxbox ~]$ bc <<< "2+3.3"  #可以直接这样来进行计算. 注意这种三连<<<的用法,它相当于是简短的可以直接输入的文本的重定向.
[me@linuxbox ~]$ echo $((foo++))  #假设变量foo=1,那么这种方式是先打印foo的值,然后给foo增1,再次echo的时候foo才是2
[me@linuxbox ~]$ echo $((++foo))  #而这种方式是先给foo增1,使得foo变成了2,然后将foo打印出来,也就是第一次打印的时候就是2,大多时候前缀运算符最有用.
[me@linuxbox~]$ ((a<1?++a:--a)) #注意这个三元运算符,如果写成((a<1?a+=1:a-=1))会报错,但是如果把赋值运算括起来((a<1?(a+=1):(a-=1)))就可以执行
```
```
[me@linuxbox ~]$ declare -a array  #定义一个空数组array
[me@linuxbox ~]$ a[1]=foo  #定义数组a并赋值
[me@linuxbox ~]$ days=(Sun Mon Tue Wed Thu Fri Sat)   # 当然,${#days[@]}依然用来确定数组元素个数
[me@linuxbox ~]$ days=([0]=Sun [1]=Mon [2]=Tue [3]=Wed [4]=Thu [5]=Fri [6]=Sat)
[me@linuxbox ~]$ animals=("a dog" "a cat" "a fish")
[me@linuxbox ~]$ for i in "${animals[*]}"; do echo $i; done  # 符号@和#依然用来代替数组中的变量
a dog a cat a fish
[me@linuxbox ~]$ for i in "${animals[@]}"; do echo $i; done  #这种输出最美观 ,若 ${animals[*]},${animals[@]}没有被引号引起来,则输出不美观
a dog
a cat
a fish
[me@linuxbox ~]$ for i in "${!animals[@]}"; do echo $i; done  #如果是给"${!animals[@]}"中加了叹号,则表示点元素下标而不是值
1
2
3
[me@linuxbox ~]$ animals+=("a chicken" "a sheep" "a horse")  #直接往数组后续追加元素用+=
[me@linuxbox ~]$ unset animals  #使用unset来删除一个数组
[me@linuxbox ~]$ unset 'animals[2]'  #删除其中的一个元素,注意要用引号来防止展开. 无论什么时候,只要是会有展开歧义的地方都要使用引号
[me@linuxbox ~]$ animals=     #手动给数组赋值为空并不会清空数组,还是原样

#!/bin/bash     #注意这个给数组排序的脚本,是把数组中的数据先全部打印了出来然后管道给了sort程序
a=(f e d c b a)
a_sorted=($(for i in "${a[@]}"; do echo $i; done | sort))
```
```
ls -l > output.txt   #覆盖
echo "Listing of foo.txt" >> output.txt  #追加
cat foo.txt >> output.txt #追加
{ ls -l; echo "Listing of foo.txt"; cat foo.txt; } > output.txt  #用组命令方式简化,{}和命令之间必须空格,且最后一个命令后加分号
(ls -l; echo "Listing of foo.txt"; cat foo.txt) > output.txt #用子shell的方式简化
{ ls -l; echo "Listing of foo.txt"; cat foo.txt; } | lpr  #组和子shell的这种方式想当于是把一大堆流组合到一起然后进行后续操作,但是子shell肯定
不能用管道,原因还是子shell执行完之后就清除所有东西了,所以把东西定向到文件可以,但是如果直接让其来输送流肯定不行.相比起来组命令消耗小且更好用.
echo "foo" | read  #由于read是在子shell中执行,所以当我们echo $REPLY的时候结果总是空,可以这样read < <(echo "foo"), <(命令列表)用于产生标准输出,
echo产生输出不用管道而是用重定向符号传给read,这样就可以在当前shell中使用了,  >(命令列表) 用于接受标准输入

ls test.sh >a.txt 2>&1 #将ls的结果冲定向到文件,实际上原始写法是 1>a.txt ,本来ls的结果是显示在标准输出的,现在把标准输出定向到文件,又因为1可以省略,
所以1>a.txt就成了>a.txt, 2>表示2将要冲定向,但是其定向方式和1相同,&表示定向方式和谁相同,那么2自然也就定向到a.txt了,当然可以直接写成2>a.txt,都可以.

$! 最后一个后台进程的pid号, $?上一个命令的执行状态, $#命令的参数个数, $@或$* 所有列表参数, $$是shell本身的pid, $0当前脚本的全路径名称
```
```
[ $user=root ] || echo "success"  #在测试条件下,一个等号并不是赋值操作,这个要格外注意,常常会想当然的认为是赋值符,实际上就是等号,用来比较值是否相等,
通常写成==更好一点, 由于单独执行测试条件是没有输出的,所以我们可以用 $? 来检查上一条语句是否执行成功,或者就用这种 && 和 || 的方式来直接打印一些东西
以此来判断前面的命令是否执行成功.
```
