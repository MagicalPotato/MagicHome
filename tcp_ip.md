##### 在浏览器地址栏键入URL，按下回车之后会经历以下流程：
1. 浏览器向 DNS 服务器请求解析该 URL 中的域名所对应的 IP 地址;
2. 解析出 IP 地址后，根据该 IP 地址和默认端口 80，和服务器建立TCP连接;
3. 浏览器发出读取文件(URL 中域名后面部分对应的文件)的HTTP 请求，该请求报文作为 TCP 三次握手的第三个报文的数据发送给服务器;
4. 服务器对浏览器请求作出响应，并把对应的 html 文本发送给浏览器;
5. 释放 TCP连接;
6. 浏览器将该 html 文本并显示内容; 

##### GET和POST的区别
- 区别1---GET提交的数据会在地址栏中显示出来,POST提交地址栏不会改变
```
GET请求: 请求的数据会附在URL之后(就是把数据放置在HTTP协议头中),以?分割URL和传输数据:login.action?name=xx&psword=xx&verify=%E4%BD%A0 %E5%A5%BD。
如果数据是英文字母/数字,原样发送,如果是空格,转换为+号,如果是中文/其他字符,则直接把字符串用BASE64加密：%E4%BD%A0%E5%A5%BD,其中％XX中的XX为该符号以16
进制表示的ASCII。
GET /books/?sex=man&name=Professional HTTP/1.1
Host: www.wrox.com
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6) Gecko/20050225 Firefox/1.0.1
Connection: Keep-Alive
                        #注意这里有一个必须的空行
-----------------------------------------------------------
POST请求:把提交的数据放置在是HTTP包的包体中.同时post的请求参数是在http标题的一个不同部分（名为entity body）传输的，这一部分用来传输表单信息，
因此必须将Content-type设置为:application/x-www-form-urlencoded
POST / HTTP/1.1
Host: www.wrox.com
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6) Gecko/20050225 Firefox/1.0.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 40
Connection: Keep-Alive
                              #注意这里也是一个必须的空行
name=Professional%20Ajax&publisher=Wiley
```
- 区别2---对数据大小的限制
```
HTTP协议没有对传输的数据大小进行限制，HTTP协议规范也没有对URL长度进行限制。而在实际开发中存在的限制主要有：
GET:特定浏览器和服务器对URL长度有限制，例如IE对URL长度的限制是2083字节(2K+35)。对于其他浏览器，如Netscape、FireFox等，理论上没有长度限制，
其限制取决于操作系统的支持。因此对于GET提交时，传输数据就会受到URL长度的限制。
POST:由于不是通过URL传值，理论上数据不受限。但实际各个WEB服务器会规定对post提交数据大小进行限制，具体由配置决定,Apache、IIS6都有各自的配置。
```
- 区别3---安全性
  - POST的安全性肯定要比GET的高。一个明文一个密文.