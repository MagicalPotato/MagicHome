```
sudo -i

安装内核并开启加速:
yum install -y wget
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
上面这条是合在一起的命令,下面是分开的,两种都可以.
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
chmod +x bbr.sh
./bbr.sh
安装完成后，脚本会提示需要重启 VPS，输入 y 并回车后重启。然后验证内核是否成功安装:
uname -r     #用来查看内核版本,如果是最新的就表示安装成功了,最新的大概都4.19


安装shadowsocks-r,这个脚本是四合一脚本,所以是-all,但是安装完成之后环境上的程序是-r:
wget --no-check-certificate -O shadowsocks-all.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-
all.sh
chmod +x shadowsocks-all.sh
./shadowsocks-all.sh 2>&1 | tee shadowsocks-all.log

卸载:
./shadowsocks-all.sh uninstall  #原本卸载命令是这个,但是没找到这个,只找到了-r.我怀疑是他把-all这个脚本添加到系统PATH中去了.后续看一下这个脚本到底
写了什么东西,面前也暂时用不到卸载就先这样放着吧.
./shadowsocks-r.sh uninstall   #卸载的时候只能是在root的目录下卸载,因为卸载文件shadowsock-r.sh放在root用户目录下


启动：/etc/init.d/shadowsocks-r start   #查看状态等这些东西是在etc目录下
停止：/etc/init.d/shadowsocks-r stop
重启：/etc/init.d/shadowsocks-r restart
状态：/etc/init.d/shadowsocks-r status

配置文件路径：/etc/shadowsocks-r.json
日志文件路径：/var/log/shadowsocks-r.log
代码安装目录：/usr/local/shadowsocks-r


多用户配置文件示例
{
"server":"0.0.0.0",
"server_ipv6": "[::]",
"local_address":"127.0.0.1",
"local_port":1080,
"port_password":{
    "8989":"password1",
    "8990":"password2",
    "8991":"password3"
},
"timeout":300,
"method":"aes-256-cfb",
"protocol": "origin",
"protocol_param": "",
"obfs": "plain",
"obfs_param": "",
"redirect": "",
"dns_ipv6": false,
"fast_open": false,
"workers": 1
}

配置完多用户之后如果新的端口不能使用,可能是防火墙的问题(如果开了防火墙的话),到/etc/firewalld/zones/,用vi编辑public.xml
把新增的端口按照格式加上去,每个端口要加两遍. 加好之后重启shadowsocks和防火墙。
/etc/init.d/shadowsocks-r restart
firewall-cmd –reload
```
