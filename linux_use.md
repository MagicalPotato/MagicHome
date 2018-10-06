### 在使用linux中遇到的一些问题和解决方式
1. 无论是下载最小的安装包还是完整的包,最终安装的时候都是以最小标准来安装的.你需要在安装完成之后再手动安装别的软件包.
2. 安装好的系统输入命令 yum --help ,如果发现yum已经正常安装了但是使用yum安装别的软件的时候却报错,说yum无法使用,这种情况很有可能是子net模式下你的网卡
没有随系统一起启动.这时候需要修改网卡配置文件(如果是桥接模式需要修改ip地址,后续查查):
   - 以root用户登录
   - cd /ect/sysconfig/networ-scripts  (到网卡配置文件所在的目录)
   - vi ifcfg-eno33  (用vi编辑网卡配置文件,网卡配置文件的格式是 ifcfg-eno+几个数字,把最后一行的onboot的值no改成yes, 然后 :wq (保存退出))
   - 重启系统(reboot), 或者重启网络服务 (service network restart)
3. yum能正常工作之后安装如下一些软件包:
   - yum -y install wget
   - yum install -y net-tools (装了这个之后ifconfig(interface config)命令就可以正常使用了)
   - yum install -y vim-enhanced (centos自带vi编辑器,我们可以装一个更强大的vim)
