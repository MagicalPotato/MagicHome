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
