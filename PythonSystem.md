### Python 2
##### part1
```
>>> 5 ** 2  # 5 squared
25

>>> 3 * 'un' + 'ium'        #字符串可以由 + 操作符连接(粘到一起)，可以由 * 表示重复
'unununium' 

>>> word = 'Python'         #字符串也可以被截取(检索)
>>> word[0]  # character in position 0
'P'

    P   y   t   h   o   n   # 字符串的切片操作 [start:end:step)  注意这是个左闭右开区间  定义一个变量指向某个字符串或者数组或者list,
    0   1   2   3   4   5  6  # 实际上这个变量指向的就是这个数组或者list或者字符串(字符串本质上就是个数组,但是当成整体就是一个字符串)
-7 -6  -5  -4  -3  -2  -1   # 的第一个元素,计算机底层 的索引方式是基址+索引,那么比如一个数组,第一个元素的索引就是数据变量的索引,
那么要获取第一个元素只能是+0,你要是+1那么元素就索引到下一个了,这就浪费了0所在的那个位置,所以大多数语言的下标索引都是从0开始.
这种从0开始的索引方式在其他方面还有更多的优点.当然,现代语言中也有一些语言是从1开始的.
# 如果step是正数n,那么从左往右切n个, 如果step是负数n,那么从右往左切-n个,符号只表示方向,实际上意识还是从右往左切了n个
# start,end,step都可以不写, start默认为0,end默认为长度,step默认为1[::],更常见的是这种[:2],[6:]  把一个字符串倒过来可以这样[::-1]
# 正向的索引从0开始.-0就是0,所以逆向没有从-0开始的说法,逆向从-1开始,那么要索引完整个字符串应该是[-1:-7:-1],-7是上一个数据的最后一个地址
# [:6:2]的结果是pto,可以发现当步长大于1时,可以理解成每被截取的两个字符串之间的地址距离,p隔两个地址是t,t隔两个地址是0,左开右闭所以p是被包含的
# 如果end大于字符串总长,那么end就是总长len(str),如果end小于start则会返回空,比如[5:1]

>>> s = 'supercalifragilisticexpialidocious'  # Python的字符串长度方法是len()
>>> len(s)

>>> u'Hello\u0020World !'  # 前面加u表示这会创建一个 Unicode 字符串。在其中包含特殊字符，可以这样.  \u0020表示Unicode的空格
u'Hello World !'

>>> str(u"abc") # str方法默认将一个其他编码的str转成ASCII，ASCII只能接受 0 到 127 这个范围的编码，否则报错。
'abc'
>>> u"盲枚眉"  
u'\xe4\xf6\xfc'
>>> str(u"盲枚眉")  #可以看到这个装换失败
Traceback:UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-2: ordinal not in range(128)

>>> u"盲枚眉".encode('utf-8') #将一个Unicode转成其他类型
>>> unicode('\xc3\xa4\xc3\xb6\xc3\xbc', 'utf-8')  # 将一个其他类型转成Unicode
```

##### part2
```
两个列表可以直接用 + 号相加,可以正向索引也可以逆向索引,列表也可以用分片操作,可以对分片进行赋值和修改,使用append()进行追加,len(arr)同样适用
>>> letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g']
>>> letters[2:5] = ['C', 'D', 'E']
>>> letters
['a', 'b', 'C', 'D', 'E', 'f', 'g']
>>> letters[2:5] = []
>>> letters
['a', 'b', 'f', 'g']
>>> letters[:] = []  #整个置空

if elif else.........

>>> range(10)      # range()得到一个数值列表
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> range(5,10)
>>> range(0,10,2)  #还可以指定范围和步长

>>> a = ['Mary', 'had', 'a', 'little', 'lamb']  # 同时打印一个列表的序号和值
>>> for i in range(len(a)):
...     print i, a[i]
...
0 Mary ............

def cheeseshop(kind, *arguments, **keywords)  # *name接收一个元祖,而**name接手一个字典,这种既可以解包右可以拆包,详情再看看

list.remove(x)  # 删除第一次遇到的这个元素,如果没有会报错
list.insert(1,5) # 在某个位置插入
list.pop([5])  #删除指定位置的元素,如果没有指定位置,则删除最后一个,[]表示这个参数可选,并不是让你输入一个参数
list.index(x) #索引x的位置
list.count(x)  # 计算x出现的次数
list.sort()  #排序
list.reverse()  #倒排序

squares = [x**2 for x in range(10)] #列表推导式  得到[0,1,4,9,16.......]

>>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]

vec = [[1,2,3], [4,5,6], [7,8,9]]
>>> [num for elem in vec for num in elem]
[1, 2, 3, 4, 5, 6, 7, 8, 9]

>>> from math import pi
>>> [str(round(pi, i)) for i in range(1, 6)]
['3.1', '3.14', '3.142', '3.1416', '3.14159']

>>> matrix = [
...     [1, 2, 3, 4],
...     [5, 6, 7, 8],
...     [9, 10, 11, 12],
... ]
>>> [[row[i] for row in matrix] for i in range(4)]
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]

>>> t = 12345, 54321, 'hello!'  #元组由好几个逗号分隔的数据组成,这样就可以直接定义一个元祖,元组不可更改,虽然不写括号没错,但是建议写上
>>> t
(12345, 54321, 'hello!')

python中提供集合,就是可以去重的set,但是要生成一个set必须显示的调用set()方法,不能直接用{}定义set,大括号是用来定义字典的.

字典.keys()可以获取字典的key列表. dict()方法可以直接创建字典.sorted()方法对字典进行排序,当然sorted()同样可以对列表排序
>>> dict(sape=4139, guido=4127, jack=4098)
{'sape': 4139, 'jack': 4098, 'guido': 4127}
>>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
{'sape': 4139, 'jack': 4098, 'guido': 4127}
>>> {x: x**2 for x in (2, 4, 6)}  #注意这个字典推导式,和上面的元祖推导式有差别,有冒号无括号
{2: 4, 4: 16, 6: 36}
>>> for k, v in 字典.iteritems():  # iteritems()可以同时得到键值对
...     print k, v

>>> for i, v in enumerate(['tic', 'tac', 'toe']):  #循环时可以使用enumerate方法同时得到位置和值
...     print(i, v)

a < b == c 判断是否 a 小于 b 并且 b 等于 c 
A and not B or C 等于 (A and (notB)) or C  # not的优先级最高,下来是and,最后是or
```

#### part3
```
# .py结尾的文件就是python中的模块,模块的模块名(做为一个字符串)可以由全局变量 __name__ 得到。import aa.py as uu, print uu._name_
#  在当前目录下创建一个aaa.py文件,那么在当前目录下打开解释器,就可以直接import aaa.py,但是这只是导入了模块名称,里面的内容并未导入,你需要在使用的地方显示的用模块名称来调用模块中的方法.  如果只用某个模块中的某个方法,也可以只导入方法,但这种方式不会导入模块名,所以用_name_得不到模块名
# 除了包含函数定义外，模块也可以包含可执行语句。这些语句一般用来初始化模块。它们仅在 第一次 被导入的地方执行一次。
# 出于性能考虑，每个模块在每个解释器会话中只导入一遍。因此，如果你修改了你的模块，需要重启解释器或者如果你就是想交互式的测试这么一个模块，可以用 reload() 重新加载 .  导入的时候不要使用from aaa import *,这种全部导入的方式,这使得代码很难读. 这种方式不到导入以下划线_开头的属性.
### 模块也可以被称为脚本, 可以在控制台直接用 Python aa.py 参数 的方式来执行这个脚本,但是你的脚本中必须有main的定义:
if __name__ == "__main__":
    import sys
    fib(int(sys.argv[1]))
这就跟java一样,可以直接当做jar包使用,如果你想直接运行,那么代码中需要有main方法的入口.
# 当导入一个模块的时候,解释器会先在当前路径查找这个.py模块,注意解释器的当前路径不是桌面,而是你的python解释器安装的那个目录. 如果当前目录没找到,就
去python的安装目录的bin目录下找,最后去python的安装目录找.都没找到就报错. 如果你有个模块和python自有的某个模块冲突了,那么尝试加载的时候会报错.
# 有时候在你的aa.py文件的目录下可能存在一个aa.pyc文件,这个文件是aa.py的预编译文件.这个文件有用处.可以先不管但是要知道这个问价.

# 一个python包中可以包含很多模块,包中必须要有个空_init_文件以用来标识这是一个包
import sound.effects.echo  #导入包中的echo, 但是使用时必须用全名sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)
from sound.effects import echo  #用这种方式导入可以直接用echo.echofilter()这种方式调用
from sound.effects.echo import echofilter #这种方式可以这从模块导入函数直接使用

>>> print 'We are the {1} who say "{0}!"'.format('knights', 'Ni') # python中的format方法,字符串中是用大括号进行占位.大括号中
We are the ni who say "knights!"                                  #可以自己定义要展示哪一个字符串,根据位置进行判断.
>>> print('The value of PI is approximately {0:.3f}.'.format(math.pi))  #字段名后允许可选的 ':' 和格式指令。下例将 Pi 转为三位精度
The value of PI is approximately 3.142.   
>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678} # 在字段后的 ':' 后面加一个整数会限定该字段的最小宽度，这在美化表格时很有用
>>> for name, phone in table.items():    #这里从字典中取出每一个键值对
...     print '{0:10} ==> {1:10d}'.format(name, phone)  #打印的时候我先打印键值对中的第一个数值,也就是key,长度是10,然后打印了个==>
...                                                    # 最后打印了键值对中的位置为1的数据,也就是value,长度是10个字符而格式是数字.
Jack       ==>       4098
Dcab       ==>       7678
Sjoerd     ==>       4127

>>> f = open('workfile', 'w') #第一个参数是文件路径,第二个是模式. 模式有：'r'，只读；'w'，只写(对于同名文件，该操作使原有文件被覆盖),
>>> print f                  # 'a'，以追加方式打开文件；'r+'，以读写方式打开文件；如果没有指定，默认为 'r' 模式。
#在 Windows 平台上，'b' 模式以二进制方式打开文件，所以可能会有类似于 'rb'， 'wb'， 'r+b' 等等模式组合。Windows 平台上文本文件与二进制文件是有区别的，读写文本文件时，行尾会自动添加行结束符。这种后台操作方式对 ASCII 文本文件没有什么问题，但是操作 JPEG 或 EXE 这样的二进制文件时就会产生破坏。在操作这些文件时一定要记得以二进制模式打开。在unix上这样加个'b'也无害,可以用.
# 要读取文件内容，需要调用 f.read(size)，该方法读取若干数量的数据并以字符串形式返回其内容，size 是可选的数值，指定字符串长度。如果没有指定 size 或者指定为负数，就会读取并返回整个文件。当文件大小为当前机器内存两倍时，就会产生问题。反之，会尽可能按比较大的 size 读取和返回数据。如果到了文件末尾，f.read() 会返回一个空字符串(''); f.readline()读取一行,每读取一行会给一行后加一个换行符\n,如果文件中途有空行,会返回一个只包含换行符'\n'的字符串,
否则如果返回的是一个真正的空串'',那说明文件已经读完了.  readlines()读入文件的所有行并返回一个列表.
# f.write('xxx') 将数据写入文件并返回写入的字符串长度,其他非字符串数据要写入也必须先转换成字符串. str()
>>> with open('/tmp/workfile', 'r') as f: #用关键字 with 处理文件对象是个好习惯。它的先进之处在于文件用完后会自动关闭，就算发生异常也没关系。
...     read_data = f.read()              # 它是 try-finally 块的简写
>>> f.closed

json.dump(x, f) # x为一个数据对象,f是一个写入模式的文件对象, 用这个方法可将x系列化到f中
x=json.load(f)  # f是读模式的文件对象.  load会将里面的数据再反序列化出来. 

# try ... except 语句可以带有一个 else子句 ，该子句只能出现在所有 except 子句之后。当 try 语句没有抛出异常时，需要执行一些代码，可以使用这个子句






```
