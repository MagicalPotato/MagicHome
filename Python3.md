### Python3

#### part1
```
int(x) 将x转换为一个整数。
float(x) 将x转换到一个浮点数。
complex(x) 将x转换到一个复数，实数部分为 x，虚数部分为 0。
complex(x, y) 将 x 和 y 转换到一个复数，实数部分为 x，虚数部分为 y。x 和 y 是数字表达式。

>>> 17 / 3  # python3中两个整数的除法返回浮点型,如果要返回正数要用两个斜杠//  
5.666666666666667
>>>
>>> 17 // 3   # 当然如果你的分子本身就是浮点数3.0,那么返回的肯定就是一个浮点数
5

abs(x)返回数字的绝对值，如abs(-10) 返回 10
ceil(x)返回数字的上入整数，如math.ceil(4.1) 返回 5cmp(x, y)
fabs(x)返回数字的绝对值，如math.fabs(-10) 返回10.0  # 注意这个是浮点的

# \ 在行为表示续行符 , \000 空 ,\n换行,\r回车
# 表示原始字符串前的r可以大写也可以小写

#字符串的格式化新版还是大括号,大括号的格式化有很多便利和新特性. 旧版是%占位,%s占位一个字符串,%d格式化整数,%f格式化浮点数,%%输出一个单一的%
# 字符串的三引号和markdown的语法一样,比如a = '''xxxxxxxxxxx''',无论引号中是什么格式,都可以原封不动的保留.

capitalize()            #将字符串的第一个字符转换为大写
count(str, beg= 0,end=len(string)) #返回 str 在 string 里面出现的次数，如果 beg 或者 end 指定则返回指定范围内 str 出现的次数
encode(encoding='UTF-8',errors='strict') 
# encoding 指定的编码格式编码字符串，如果出错默认报一个ValueError 的异常，除非 errors 指定的是'ignore'或者'replace'
bytes.decode(encoding="utf-8", errors="strict")
#Python3中没有decode解码方法，但我们可以使用 bytes 对象的 decode() 方法来解码给定的 bytes 对象，这个 bytes 对象还是可以用 str.encode() 来编码。

#列表中删除元素用del方法   del list[2]  获取长度还是用len(list)  max(list)返回列表元素最大值, min(list)最小值
# print(b, end=',')  关键字end用于在打印b之后打印一个你指定的字符,在这个例子中就是给b后面打印了一个逗号

# strings, tuples, 和 numbers 是不可更改的对象，而 list,dict 等则是可以修改的对象。变量赋值 a=5 后再赋值 a=10，这里实际是
新生成一个 int 值对象 10，再让 a 指向它，而 5 被丢弃，不是改变a的值，相当于新生成了a。   变量赋值 la=[1,2,3,4] 后再赋值 la[2]=5 
则是将 list la 的第三个元素值更改，本身la没有动，只是其内部的一部分值被修改了。

# 整数、字符串、元组。如fun（a），传递的只是a的值，没有影响a对象本身。比如在 fun（a）内部修改 a 的值，只是修改另一个复制的对象，
不会影响 a 本身。 列表，字典。如 fun（la），则是将 la 真正的传过去，修改后fun外部的la也会受影响
python 中一切都是对象，严格意义我们不能说值传递还是引用传递，我们应该说传不可变对象和传可变对象。

a = 10
def test():
    a = a + 1
    print(a)
test()   #这段代码会报错,因为 test 函数中的 a 使用的是局部，未定义，无法修改。修改 a 为全局变量，通过函数参数传递，可以正常执行输出结果为：
a = 10
def test(a):
    a = a + 1
    print(a)
test(a)       # 也就是说python一个类中的全变量不是在这个类中任何地方都可以随便使用的,你得传入到方法中去使用.和java有区别.


# 可以用大括号{}创建集合。注意：如果要创建一个空集合，你必须用 set() 而不是 {} ；单独的{}创建一个空的字典
>>> a = set('abracadabra')
>>> a                                  # a 中唯一的字母
{'a', 'r', 'b', 'c', 'd'}

>>> import sys
>>> sys.path
['', '/usr/lib/python3.4', '/usr/lib/python3.4/plat-x86_64-linux-gnu', '/usr/lib/python3.4/lib-dynload', 
'/usr/local/lib/python3.4/dist-packages', '/usr/lib/python3/dist-packages']
# sys.path 输出是一个列表，其中第一项是空串''，代表当前目录（若是从一个脚本中打印出来的话，可以更清楚地看出是哪个目录），
亦即我们执行python解释器的目录（对于脚本的话就是运行的脚本所在的目录）。
```
#### part2
```
#一个模块被另一个程序第一次引入时，其主程序将运行。如果我们想在模块被引入时，模块中的某一程序块不执行，我们可以用__name__属性
来使该程序块仅在该模块自身运行时执行。每个模块都有一个__name__属性，当其值是'__main__'时，表明该模块自身在运行，否则是被引入。
if __name__ == '__main__':
   print('程序自身在运行')
else:
   print('我来自另一模块')
$ python using_name.py      #和java中的main方法用法一样.
程序自身在运行
>>> import using_name
我来自另一模块

pickle.dump(obj, file, [,protocol]) #新版的序列化和反序列化模块成了pickle. 这个将对象obj保存到文件file中,有个可选参数
x = pickle.load(file)

# __xxx：两个下划线开头，声明该属性为私有，不能在类地外部被使用或直接访问。在类内部的方法中使用时 self.__xxx。如果创建了一个对象A,
用A.__xxx来调用这个类的私有属性是不行的,和java一样,私有属性不能在类外部使用.
# 类中以两个下划线开头的方法__foo()为私有方法.  注意和__foo__这种特性方法的区别,这种特性方法的标准叫法是析构方法,类似于默认函数
class people:
    def __init__(self,name,age):
        self.name=name
        self.age=age

    def __str__(self):
        return '这个人的名字是%s,已经有%d岁了！'%(self.name,self.age)

a=people('孙悟空',999)
print(a)
例子中的析构函数就相当于java中的toString,打印结果是: 这个这个人的名字是孙悟空,已经有999岁了！
如果没有重载函数的话输出的就是一串看不懂的字符串：<__main__.people object at 0x00000272A730D278>

```

