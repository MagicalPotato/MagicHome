```
sudo -i

安装:
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh 2>&1 | tee shadowsocksR.log

卸载:
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
