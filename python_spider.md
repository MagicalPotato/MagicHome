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
- 还是到上面地址搜索并下载pywin32的库然后用pip安装 pip install pywin32-224-cp37-cp37m-win_amd64.whl ,安装完之后要注意,这个库存还是导不进去,这时候
到你本机这个目录 C:\Python37\Lib\site-packages\pywin32_system32,将该目录下的两个文件pythoncom37.dll和pywintypes37.dll拷贝到C:\Windows\System32
目录下

__pip install scrapy # 安装完上面这一大堆库之后scrapy才能正常安装.完成后在命令行输入命令 scrapy 测试是否安装成功__

浏览器F12进入审查页面,选择Network选项,然后点击左侧的index.html,就能找到User-Agent和各种请求信息

爬取过程中如果报错,重新改了之后会重新从头开始爬取,效率低下,这是代码要用try捕获异常的原因之一. 

requests库主要用于下载网页,主要会报四个错,这些错都继承自requests.exceptions.RequestException:
  - Request抛出ConnetcionError  #通常是网络问题,如:DNS查询失败,服务器拒绝连接等
  - Request抛出Timeout  #请求超时
  - Request抛出TooManyRedirects # 请求超过设定的重定向次数
  - Response.raise_for_stats()抛出HTTPError # 请求失败,这个就和状态码有关,比如404页面不存在
