* Shell指的是shell脚本编程. 大多linux默认执行shell脚本的Shell程序是bash  #!/bin/sh同样也可以改为 #!/bin/bash  其中#! 是一个约定的标记，
它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell程序。
* echo "xxxxx"  echo用于向窗口输出文本
* 执行脚本有两种方式,一种是让这个脚本文件具有可执行权限,另一种是把脚本文件当成解释器的参数.  第一种:首先cd到你的当前目录,给你当前目录下写的脚本添加可
执行权限 chmod +x ./test.sh ,当前目录是./ 如果不加./会默认去系统的path下找这个脚本,当然是找不到的. 权限加了之后就可以执行了 ./test.sh ,直接执行就是
在命令行输入脚本名称.  第二种: 直接把脚本当成参数传给解释器. /bin/sh test.sh
* Shell语言定义变量时,变量名前不加美元符号$,但是在php语言中需要. your_name="runoob.com" ,注意**shell中的变量和等号之间不能有空格**.可以在代码中使用命令语句for file in ``` `ls /etc` ```(注意这个不是单引号,是反引号) 或者for file in $(ls /etc) 该语句可以将/etc下目录的目录名称列出来
* 使用一个定义过的变量，只要在变量名前面加美元符号即可，如：your_name="qinjx" , echo $your_name 或者echo ${your_name}. 推荐在使用的时候给所有变量加上花括号，这是个好的编程习惯.
* readonly xxx 将xxx这个变量设置成只读变量  unset xxx 删除xxx变量,但不能删除只读变量,变量被删除后不能再次使用. 
* shell中的字符串可以使用单双引号,但稍有区别. 尽量使用双引号,限制少.  string="abchhjkhjd",echo ${#string},加了#相当于是求字符串长度. echo ${string:1:4},从第二个字符开始截取四个字符
* Shell 中，用括号来表示数组，数组元素用"空格"符号分割开. array_name=(value0 value1 value2 value3).  也可以用下标来定义 array_name[0]=value0 .......array_name[n]=value0 可以不使用连续的下标，而且下标的范围没有限制.获取数组值的一般格式是使用下标 valuen=${array_name[下标]}, 使用 @ 符号可以获取数组中的所有元素 echo ${array_name[@]}, 取得数组元素的个数 length=${#array_name[@]}或者length=${#array_name[`*`]}
* shell中没有多行注释,只有单行注释 ,使用#号







