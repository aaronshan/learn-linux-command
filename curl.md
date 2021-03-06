```
curl www.csdn.net
```
请求csdn，返回csdn主页的内容。如果想要将返回的内容写入文件，使用如下语句即可。
```
curl www.csdn.net > file.html
```
也可以使用如下语句：
```
curl -o file.html www.csdn.net
```
这个会显示下载进度条，如果你不想让它显示下载进度条，则只需要再加上-s参数，即curl -s -o file.html www.csdn.net 即可。
如果想使用断点续传功能，则只需要加上-C参数。即：curl -C -o file.html www.csdn.net 即可。
curl也支持代理访问网络。如下所示：
```
curl -x 192.168.1.1:9090 -o file.html www.csdn.net
```
如果想保存页面的cookie信息，加上-D参数即可。如下：
```
curl -D cookie.txt -o file.html www.csdn.net
```
如何在下一次访问的时候使用上一次的cookie信息呢？加上-b参数即可。
```
curl -b cookie.txt -0 file.html www.csdn.net
```
如何在请求中加上浏览器信息呢？使用-A参数即可。如下：
```
curl -A "Mozilla/5.0 (Windows NT 6.1; WOW64)" -b cookie.txt -0 file.html www.csdn.net
```
如何欺骗服务器，告诉服务器自己是从某一个网址过来的呢？使用-e：
```
curl -e www.a.com -o file.html www.b.com
```
就可以欺骗www.b.com，你是从www.a.com过来的了。
使用-o可以下载文件。使用-O可以按照服务器上的文件名下载文件。
```
curl -O www.a.com/test.jpg
```
此外，curl下载文件还支持正则表达式批量下载。
curl也支持分块下载可以使用-r参数。比如下载一首歌a.mp3。可以使用如下命令：
```
curl -r 0-10240 "a.part1" www.a.com/a.mp3
curl -r 10241-20480 "a.part2" www.a.com/a.mp3
```
在linux下再使用cat a.part* > a.mp3就可以将文件合并为一个整体。
curl还支持FTP下载。用法如下：
```
curl -u username:password ftp://ip:port/path/file
```
或者
```
curl ftp://username:password@ip:port/path/file
```
讲了下载，再说说上传。上传使用的参数是-T。比如向ftp上传一个文件：
```
curl -T file.txt -u username:password ftp://ip:port/path
```
或者
```
curl -T file.txt http://www.a.com/a
```
此时使用的是HTTP的put方法。
如何制定http协议的各种方法呢？get方法不用指定，因为默认就是它。比如：
```
curl http://www.a.com/login?username=shan&password=1234
```
使用post方法时需要使用-d参数。比如：
```
curl -d "username=shan&password=1234" http://www.a.com/login
```
如果想要上传json数据，则可以使用如下语句：
```
curl -H "Accept: application/json" -H "Content-type: application/json" -X POST -d '{"title":"a new test","content":"hahahah","author":"shan","status":0,"created":"2013-08-10 12:03:04"}' http://a.test.com/api/post
```
使用put方法上传json数据，则可以使用如下命令：
```
curl -H "Accept: application/json" -H "Content-type: application/json" -X PUT -d '{"title":"a new test1"}'http://a.test.com/api/post
```
使用curl测试接口响应时间：
```
curl -o /dev/null -s -w '%{time_connect}:%{time_starttransfer}:%{time_total}\n' 'http://test.com/api/a'    
0.081:0.272:0.779
```
通过 -o 参数发送到 /dev/null，-s 参数去掉所有状态信息， -w 参数让 curl 写出列出的计时器的状态信息。  
curl 使用的计时器：

| 计时器 | 描述 |
|---------| -------|
|time_connect |建立到服务器的 TCP 连接所用的时间   |
|time_starttransfer|在发出请求之后,Web 服务器返回数据的第一个字节所用的时间   |
|time_total|完成请求所用的时间    |
|time_namelookup|DNS解析时间,从请求开始到DNS解析完毕所用时间(记得关掉 Linux 的 nscd 的服务测试)    |
|speed_download|下载速度，单位-字节每秒。  |  

这些计时器都相对于事务的起始时间,甚至要先于 Domain Name Service（DNS）查询.因此,在发出请求之后,Web 服务器处理请求并开始发回数据所用的时间是 0.272 – 0.081 = 0.191 秒.客户机从服务器下载数据所用的时间是 0.779 – 0.272 = 0.507 秒.

通过观察curl数据及其随时间变化的趋势,可以很好地了解站点对用户的响应性.以上变量会按curl认为合适的格式输出，输出变量需要按照%{variable_name}的格式，如果需要输出%，double一下即可，即%%，同时，\n是换行，\r是回车，\t是TAB。

curl设置超时时间：
使用curl时，有两个超时时间：一个是连接超时时间，另一个是数据传输的最大允许时间。
连接超时时间用 –connect-timeout 参数来指定(参数值的单位为秒)，数据传输的最大允许时间用 -m 参数来指定（参数值的单位为秒）。
例如：
```
curl –connect-timeout 10 -m 20 “http://test.com/api/a”
```

连接超时的话，出错提示形如：
curl: (28) connect() timed out!
数据传输的最大允许时间超时的话，出错提示形如：
curl: (28) Operation timed out after 2000 milliseconds with 0 bytes received
