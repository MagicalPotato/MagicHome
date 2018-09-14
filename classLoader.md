#### java的根加载器,拓展类加载器和系统类加载器加载的路径
```
String a = System.getProperty("sun.boot.class.path");  //根加载器
System.out.println(a.replace(";", "\n"));
C:\Program Files\Java\jre1.8.0_141\lib\几个指定的.jar
C:\Program Files\Java\jre1.8.0_141\lib\classes   //这个文件夹下的所有class

String b = System.getProperty("java.ext.dirs");  //拓展类加载器
System.out.println(b.replace(";", "\n"));
C:\Program Files\Java\jre1.8.0_141\lib\ext  //这个文件夹下所有
C:\Windows\Sun\Java\lib\ext                 //这个文件夹下所有

String c = System.getProperty("java.class.path");  //系统类加载器
System.out.println(c.replace(";", "\n"));
系统类加载器加载你配置的环境变量classpath下的jar和.class--- .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar
注意到这个变量里面的那个点路径没,代表当前目录,所以系统类加载器加载的就是如下这些:
1. 首先当前这个类所在的上层目录直到classes目录下的所有,例如D:\某个路径\某个微服务下\build\classes  //这个路径下的全加载
2. 然后是你当前这个工程的lib文件夹下的所有.jar都加载
3. 如果你的这个服务部署在某个容器中比如tomcat,那么你那个容器的工程lib文件夹下的所有.jar都会加载
4. 最后是你这个工程的构件工具的lib下的所有.jar都会加载
总之一句话:系统类加载器会加载所有与你当前运行的这个java类有关的.class和jar,当然你的这个.class和.jar要放在正确的
路径下,不然你放在桌面人家也不可能给你去加载.

还有个问题:每个加载器都是有自己默认的加载路径的,你要想让某个加载器去加载一个你写的类,你得把编译好的.class放到人家
指定的路径下,不然人家是无法加载的.双亲委派机制虽说每次都让父类的加载器去尝试加载,但是你如果不把你写的类放到父类指定
的路径下,那么最终父类加载器加载的还是自己路径下已有的那些jar,最终你写的类还是得你自己来加载.所以,与其说双亲委派机制是
一种类加载机制,不如说它是一种类加载的校验机制.这种机制保证了加载过的类是不会被重复加载.如果你在父类的路径下已经放了一个你
写的类了,那么父类在加载的时候就已经加载了,你再想用自己写的加载器去加载肯定就不行了. 你只能把你写的类加载器的父类指定
为null,这样你就可以用你写的类加载器进行加载了.
```
