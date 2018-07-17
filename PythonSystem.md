### Python
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
