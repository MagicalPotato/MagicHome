# 使用注意事项
* 在关闭pycharm之前,选中工程然后 File-Close Project ,可以将当前工程关闭,这时候就会跳转到软件刚打开的那个可以新建工程的界面了.
* File-Settings-Project:Python-project interpretate 可以选择Python的解释器环境.解释器环境下面有一个框子右上角可以添加Python的模块.
* **pycharm对中文支持不太好,有的版本会出现输入法光标不跟随,这时候到pycharm的安装目录下去,将安装目录下的jre64文件夹删了,然后把你自己装的Java的jre
文件夹拷贝过来,一般在C:\Java\jre1.8.0_162,也就是将jre1.8.0_162文件夹拷贝到你刚删了pycharm的jre64文件夹的那个目录下,然后将jre1.8.0_162文件夹的名字
重命名成jre64,最后再到你的Java到jdk目录下的lib目录,一般是这个C:\Java\jdk1.8.0_91\lib,把里面一个叫tools.jar的文件拷贝到pycharm的安装目录下你刚改的
那个jre64目录下的lib目录下就可以了.注意操作之前要先把pycharm关了,操作完了之后再重启就可以了**
* settings-File and codeTemplates,可以设置每次创建的新文件头默认显示的东西. 设置的时候可以参考已有的,里面是可以用全局变量的.
* 有的单词拼写错误或者注释当中有了拼音,编辑器会检查,单词下面会有黄色波浪线,这时候选中该单词然后右键第一行有个spelling,里面有个把该单词加入检查字典.
点一下把该单词加到检差字典中就行了. 其他的编辑过程中的波浪线自行查看.
* 基本快捷键  在Settings-keymap中可以设置快捷方式
  - trl + D 复制一行    ctrl + Y 删除一行    
  - shift + 回车 可以使得光标在一行的某个位置的时候直接换行  
  - ctrl + / 批量注释 
  - ctrl ➕ - 折叠某个方法     ctrl + shift ➕ - 全部代码折叠    打开是 ➕ + 
* 选中一个文件,右键有个copy path,然后把这个路径粘贴在文件中,这个路径就是这个文件的全路径. 右键 show in explorer 会直接在文件夹中打开该文件
* 消除告警
  * settings-editor-inspections 将所有PEP8的选项去除对勾(用于def函数的命名检查和导包时候的检查)
  * settings-editor-inspections 找到spelling选项,下面就一项typo,把typo的对勾去除(去除部分变量的命名)
  * settings-editor-color scheme-general-errors and warnings 把其中的weak warning选中,然后把右边的对勾去掉(还是消除变量命名的告警)


```
有时候安装的第三方库在本地命令行可以导入使用但是在pycharm中却导不进来,这样排查:
import sys
print(sys.path)   #导入sys库先打印当前环境的path看看当前的环境用的包都是从哪些路径下引的,一般结果如下:
['F:\\workspace_pycharm\\PySpider\\spider','F:\\workspace_pycharm\\PySpider', 
 F:\\workspace_pycharm\\PySpider\\venv\\Scripts\\python37.zip', 'F:\\workspace_pycharm\\PySpider\\venv',
'F:\\workspace_pycharm\\PySpider\\venv\\lib\\site-packages', 
'F:\\workspace_pycharm\\PySpider\\venv\\lib\\site-packages\\setuptools-39.1.0-py3.7.egg', 
'F:\\workspace_pycharm\\PySpider\\venv\\lib\\site-packages\\pip-10.0.1-py3.7.egg',   #上面这部分都是pycharm中你当前工程的虚拟环境的
'C:\\Program Files\\JetBrains\\PyCharm 2018.1.4\\helpers\\pycharm_matplotlib_backend',     #这是pycharm这个软件自己用的
'C:\\Python37\\DLLs', 'C:\\Python37\\lib', 'C:\\Python37']  #这部分才是关键,可以发现Python37的lib目录实际上已经有了,那可以推测安装的第三方库
应该没放在lib下,在doc窗口使用pip查看一个你已经安装的库看看其路径在哪:
pip show lxml
C:\Users\王泽>pip show lxml
Name: lxml
Version: 4.2.5
Summary: Powerful and Pythonic XML processing library combining libxml2/libxslt with the ElementTree API.
Home-page: http://lxml.de/
Author: lxml dev team
Author-email: lxml-dev@lxml.de
License: BSD
Location: c:\python37\lib\site-packages     #注意这里,这才是这个库的真实安装路径
Requires:
Required-by: Scrapy, parsel

现在就可以发现pycharm识别不了lib下的子目录,所以我们将这个目录手动添加到你当前工程引用的环境中去:File-Settings-Project:Python-project interpretate
点击右边的设置按钮会出现一个show all,点击然后选中你当前工程用的那个环境,然后点击右侧最下面那个图标把路径添加上即可
```
* 昨天解决了第三方包无法导入的问题,今天又看了下这个问题,有新发现.起因是导入的第三方库在用的时候无法自动提示方法,搜来搜去发现,昨天那个引路径,实际上是
在配置project的解释器,也就是在配置python的环境.pycharm每新建一个工程,都会创建一个虚拟环境,这个环境会拷贝一份你系统本地的python.exe,然后对于其他的库
则是通过路径引用.昨天的那个问题就是因为虚拟环境没有引用系统手动安装的库的路径.事实上我可以直接把工程的环境配置成系统自身的环境,新建一个环境直接指向系
统本身的环境,然后每个工程都用这个环境,那么就不存在引用不到的问题了. 但是虽然这样引用了,我发现有的方法还是没法自动提示.不知何故.后续遇到了再说吧.
