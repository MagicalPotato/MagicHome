* repository  一个项目对应一个仓库,多个开源项目那就有多个仓库
* fork 复制克隆别人的项目或代码  张三有个仓库叫test,李四只要点击了仓库下面的fork,那么李四的个人中心就会就会有一个一模一样的仓库,这个仓库是独立存在的,
  独立存在的意思就是李四要是改了fork过来的这个仓库的东西,那么张三那里是不会对应修改的,fork的仓库会带有从哪fork过来的信息,比如fork from 张三.
* pull request  李四要是改了fork过来的仓库的东西,那么李四可以发起一个pull request给张三,张三后来发现这个请求修改的东西还不错,那么张三可以将这个修改
  合并到自己到仓库中.
* watch  相当于关注  如果你watch了一个人或者一个项目,那么当他有了新动态,新建文件或改了东西,就会提示你   star是收藏,不会随时提醒你
* issue 相当讨论,如果发现了一个项目有缺陷,要改代码,但是大家都还没有思路和代码,可以发起一个issue和项目负责人或者别的人一起讨论
* 第一次创建新文件在文件最下面有对该文件的创建描述,虽然并不一定需要,但是为每一个新创建的文件添加描述是一个好习惯.
* 常用的一些命令
``` 
git config --global user.name 'MagicalPotato'       //初始化用户名和邮箱
git config --global user.email '821691908@qq.com'

ls   //列出当前目录下的文件和文件夹

pwd   //显示当前目录

clear  //清空操作框内的东西

mkdir MagicHome  //在当前目录下创建MagicHome文件夹

cd MagicHome  //到MagicHome目录下去
git init   // 把MagicHome初始化成一个git仓库  在该文件夹下会生成一个.git的隐藏文件,该文件存储仓库的基本信息
touch hello.java //在该仓库下创建hello.java文件
rm hello.java  //把刚建的那个hello.java文件删了

add hello.java  //把hello.java添加成待提交文件,也就是把hello.java文件添加到暂存区
git status    //查看文件的状态
get commit -m 'xxxxx'描述信息   //从暂存区提交到git ,必须要加描述信息否则无法提交 

git status  //已经提交完后就没有再需要提交的文件了

vi hello.java   // 用vi编辑器来编辑hello,java文件,编辑完之后按esc,然后输入  :wq 然后回车就退出编辑了 
cat hello.java  //查看hello.java文件

git rm hello.java  //从Git上删除hello.java文件

git clone 仓库地址  //把仓库上的工程克隆到本地,地址不带引号
git pull   //从git上更新最新的代码到本地,类似于svn update
git push //网Git上合并你的本地仓库,有可能会出现权限问题,是因为没有设置用户名和密码,需要手动修改 .git/config 文件添加用户名和密码
```
