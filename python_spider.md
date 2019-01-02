###### 爬虫的基本流程
1. 下载页面
2. 提取页面数据
3. 提取下载好的页面中的连接
4. 爬取
###### 要考虑的问题
1. 避免重复爬取相同的页面(URL去重)
2. 网页搜索策略(深度优先还是广度优先)
3. 爬虫访问边界

###### 安装scrapy框架之前要先安装这些库(一般能用pip安装的就用pip安装,不能装的再用whl文件手动装)
- pip3 install lxml
- pip3 install zope.interface
- 到https://www.lfd.uci.edu/~gohlke/pythonlibs/ 下载与本地python环境和计算机系统相匹配的twisted库文件Twisted‑18.9.0‑cp37‑cp37m‑win_amd64.whl,
下载好之后放到本地某个文件夹,然后在命令行cd到该文件夹然后使用pip安装该库 pip3 install Twisted‑18.9.0‑cp37‑cp37m‑win_amd64.whl
- pip3 install pyOpenSSL
- 还是到上面地址搜索并下载pywin32的库然后用pip安装 pip install pywin32-224-cp37-cp37m-win_amd64.whl ,安装完之后要注意,这个库还是导不进去,这时
到你本机 C:\Python37\Lib\site-packages\pywin32_system32,将该目录下的两个文件pythoncom37.dll和pywintypes37.dll拷贝到 C:\Windows\System32下

__pip install scrapy # 安装完上面这一大堆库之后scrapy才能正常安装.完成后在命令行输入命令 scrapy 测试是否安装成功__

浏览器F12进入审查页面,选择Network选项,然后点击左侧的index.html,就能找到User-Agent和各种请求信息

爬取过程中如果报错,重新改了之后会重新从头开始爬取,效率低下,这是代码要用try捕获异常的原因之一. 

requests库主要用于下载网页,主要会报四个错,这些错都继承自requests.exceptions.RequestException:
  - Request抛出ConnetcionError  #通常是网络问题,如:DNS查询失败,服务器拒绝连接等
  - Request抛出Timeout  #请求超时
  - Request抛出TooManyRedirects # 请求超过设定的重定向次数
  - Response.raise_for_stats()抛出HTTPError # 请求失败,这个就和状态码有关,比如404页面不存在

beautifulsoup库能将requests获取到的网页解析成soup文档,更利于后续数据的提取和处理,支持四种解析器,一个python官方,三个第三方,bs官方推荐lxml解析器:
  - BeautifulSoup(res.text , 'html.parser')  # python的官方解析器
  - BeautifulSoup(res.text , 'lxml')         # 官方推荐,效率高, 需要C库
  - BeautifulSoup(res.text , ['lxml, xml'])  # 唯一支持xml的解析器,但需要安装C语言库
  - BeautifulSoup(res.text , 'html5lib')     # 容错性四个中最好,生成html格式的文档
  
```
import  requests
headers  = {'User-Agent' .........................}
-------------------------------------------------
from  bs4  import  BeautifulSoup  #通过bs来处理网页信息
res  = requests.get ('http://www.kugou.com' , headers=headers)
soup=  BeautifulSoup(res.text , 'html.parser')
list = soup.find_all('div', 'item') 或('div', class = 'item') #查找div标签,属性class=item
list = soup.find_all('div', attrs ={'class':'item'}) #查包含特殊属性的div标签,这个特殊属性就是class,其值是item
imgTag = soup.selector('body > section > div > div:nth-child(9) > a > img')  #从大到小提取信息,类似于中国>安徽省>合肥市.. 在一个网页图片上
右键检查,在右侧高亮地方继续右键,出现一个copy选项,里面有个copy select,单击之后就能得到该元素的secletor. 这个方法得到的是单个标签.需要继续调用别的
方法来获取标签内容.比如Tag.get_text()获取文本,imgTag.get('src')获取图片. 如果我们将这个selector略作修改,就能得到所有的同类图片,返回值是标签列表:
body > section > div > div:nth-child(9) > a > img  #单个
body > section > div > div> a > img   #略作修改,通过循环再获取标签中的图片

find_all(tag ,  attibutes ,  recursive ,  text ,  limit ,  keywords ) #soup主要方法之一,查找所有满足条件的标签,返回值是一个集合
find(tag ,  attibutes ,  recursive ,  text ,  keywords)  #查找单个标签,返回值就是一个标签
-----------------------------------------------------------------------
import  re   # 通过正则表达式来爬取信息,有的网页标签结构基本相同只需替换标签中的一小部分信息,可以考虑使用正则来匹配
f = open('D:/book.txt' 'a+')  #以追加的方式打开一个文本放着,然后代码就循环爬取东西,爬出来了就写到文本里,循环结束后关闭f
res  = requests.get ('http://www.kugou.com' , headers=headers)
if res.status_code == 200 :   # res可以用来判断状态,获取成功了再爬
    txtlist = re.findall  ('<p>(.*?)</p>', res.content.decode('utf-8'), re.S)  # 爬取文本小说 
    for txt in txtlist:
        f.write(txt + '\n')
-------------------------------------------------------------------
from lxml import etree   # 通过lxml库直接处理网页信息
res  = requests.get ('http://www.kugou.com' , headers=headers)
html=  etree.HTML(res.text)
result = etree.tostring(html)
-----------------------------------------------------------
#以前一直不理解zip的作用,现在明白了,它可以将同类型有关系的东西放一起来迭代,比如这四个list将其数据放到一起整体展示才是一整条, 你如果分开循环,那就得四
次循环,完了之后还得组合数据,那将会非常麻烦:
for a, b, c, d in zip(alist,blist,clist,dlist):  
    data = {'春天':a.xxx,'夏天':b.xxx,'秋天':b.xxx,'冬天':b.xxx}  
    
urls  =  ['http://www.kugou.com/yy/rank/home/{}-8888.html'.format(str(num)) for num in range(l , 14)] #结构固定的页面url可以这样构造
注意: format语法的{}是个占位符,写成{0:s}意思并不是直接把后面的数字给你format成一个字符串,而是表示第一个位置这里要放一个字符串,放进来
之前就已经是个字符串了,直接放别的会报错.所以在后面的表达式中要先对数字进行转换,转成str之后才往里面放.
```
##### Xpath语法
```
<people>                    # people  选取people节点下所有的子节点
  <user>                    # /people  选取根元素people,起于正斜杆的路径都表示绝对路径
    <name>小明</name>     # people/user  选取people下的所有user节点
    <sex>女</sex>         # //user   选取所有的user元素而不考虑其在文档中的位置
    <id>34</id>           # people//user 选取people下的所有user而不考虑其位置
    <goal>89</goal>      # //@abc 选取名为abc的所有属性
  </user>                # /people/user[1]  以绝对路径,选根people下第一个user
</people>               # //li[@abc]  选所有的li元素,且li元素要有abc属性,不管值是多少,属性名一定要叫abc
                       # //li[@abc='kk']  选所有的li,且li元素要有abc属性,且属性值为kk
                       
xpath语法也可以从浏览器拷贝到,如同之前拷贝selector路径那样,copy里面同样有个copy xpath:
res = requests.get(url,headers=headers)
selector = etree.HTML (res.text)
img = selector.xpath('/html/body/section/div/div[1]/a/img') #这就是之前那个图片的xpath
```
