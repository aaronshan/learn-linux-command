ln是linux中又一个非常重要命令，它的功能是为某一个文件在另外一个位置建立一个同步的链接。当我们需要在不同的目录，用到相同的文件时，我们不需要在每一个需要的目录下都放一个必须相同的文件，我们只要在某个固定的目录，放上该文件，然后在 其它的目录下用ln命令链接（link）它就可以，不必重复的占用磁盘空间。

###1 命令格式：
```
ln [参数][源文件或目录][目标文件或目录]
```
###2 命令功能：
Linux文件系统中，有所谓的链接(link)，我们可以将其视为档案的别名，而链接又可分为两种 : 硬链接(hard link)与软链接(symbolic link)。
硬链接的意思是一个档案可以有多个名称，而软链接的方式则是产生一个特殊的档案，该档案的内容是指向另一个档案的位置。硬链接是存在同一个文件系统中，而软链接却可以跨越不同的文件系统。

####1) 软链接：
1. 软链接，以路径的形式存在。类似于Windows操作系统中的快捷方式
2. 软链接可以跨文件系统 ，硬链接不可以
3. 软链接可以对一个不存在的文件名进行链接
4. 软链接可以对目录进行链接

####2)硬链接:
1. 硬链接，以文件副本的形式存在。但不占用实际空间。
2. 不允许给目录创建硬链接
3. 硬链接只有在同一个文件系统中才能创建

这里有两点要注意：
> 第一，ln命令会保持每一处链接文件的同步性，也就是说，不论你改动了哪一处，其它的文件都会发生相同的变化；
> 第二，ln的链接又分软链接和硬链接两种，软链接就是
```
ln –s 源文件 目标文件)
```
它只会在你选定的位置上生成一个文件的镜像，不会占用磁盘空间，硬链接
```
ln 源文件 目标文件
```
没有参数-s， 它会在你选定的位置上生成一个和源文件大小相同的文件，无论是软链接还是硬链接，文件都保持同步变化。
ln指令用在链接文件或目录，如同时指定两个以上的文件或目录，且最后的目的地是一个已经存在的目录，则会把前面指定的所有文件或目录复制到该目录中。若同时指定多个文件或目录，且最后的目的地并非是一个已存在的目录，则会出现错误信息。

###3 命令参数：
必要参数:

* -b 删除，覆盖以前建立的链接
* -d 允许超级用户制作目录的硬链接
* -f 强制执行
* -i 交互模式，文件存在则提示用户是否覆盖
* -n 把符号链接视为一般目录
* -s 软链接(符号链接)
* -v 显示详细的处理过程

选择参数:

* -S “-S<字尾备份字符串>”或 “--suffix=<字尾备份字符串>”
* -V “-V<备份方式>”或“--version-control=<备份方式>”
* --help 显示帮助信息
* --version 显示版本信息

###4 使用实例：
####实例1：给文件创建软链接
命令：
```
ln -s log2013.log link2013
```
输出：
```
[root@localhost test]# ll
-rw-r--r-- 1 root bin      61 11-13 06:03 log2013.log
[root@localhost test]# ln -s log2013.log link2013
[root@localhost test]# ll
lrwxrwxrwx 1 root root     11 12-07 16:01 link2013 -> log2013.log
-rw-r--r-- 1 root bin      61 11-13 06:03 log2013.log
```
说明：
为log2013.log文件创建软链接link2013，如果log2013.log丢失，link2013将失效

####实例2：给文件创建硬链接
命令：
```
ln log2013.log ln2013
```
输出：
```
[root@localhost test]# ll
lrwxrwxrwx 1 root root     11 12-07 16:01 link2013 -> log2013.log
-rw-r--r-- 1 root bin      61 11-13 06:03 log2013.log
[root@localhost test]# ln log2013.log ln2013
[root@localhost test]# ll
lrwxrwxrwx 1 root root     11 12-07 16:01 link2013 -> log2013.log
-rw-r--r-- 2 root bin      61 11-13 06:03 ln2013
-rw-r--r-- 2 root bin      61 11-13 06:03 log2013.log
```
说明：
为log2013.log创建硬链接ln2013，log2013.log与ln2013的各项属性相同

####实例3：接上面两实例，链接完毕后，删除和重建链接原文件
命令：
输出：
```shell
[root@localhost test]# ll
lrwxrwxrwx 1 root root     11 12-07 16:01 link2013 -> log2013.log
-rw-r--r-- 2 root bin      61 11-13 06:03 ln2013
-rw-r--r-- 2 root bin      61 11-13 06:03 log2013.log
[root@localhost test]# rm -rf log2013.log 
[root@localhost test]# ll
lrwxrwxrwx 1 root root     11 12-07 16:01 link2013 -> log2013.log
-rw-r--r-- 1 root bin      61 11-13 06:03 ln2013
[root@localhost test]# touch log2013.log
[root@localhost test]# ll
lrwxrwxrwx 1 root root     11 12-07 16:01 link2013 -> log2013.log
-rw-r--r-- 1 root bin      61 11-13 06:03 ln2013
---xrw-r-- 1 root bin  302108 11-13 06:03 log2012.log
-rw-r--r-- 1 root root      0 12-07 16:19 log2013.log
[root@localhost test]# vi log2013.log 
2013-01
2013-02
2013-03
2013-04
2013-05
2013-06
2013-07
2013-08
2013-09
2013-10
2013-11
2013-12[root@localhost test]# ll
lrwxrwxrwx 1 root root     11 12-07 16:01 link2013 -> log2013.log
-rw-r--r-- 1 root bin      61 11-13 06:03 ln2013
-rw-r--r-- 1 root root     96 12-07 16:21 log2013.log
[root@localhost test]# cat link2013 
2013-01
2013-02
2013-03
2013-04
2013-05
2013-06
2013-07
2013-08
2013-09
2013-10
2013-11
2013-12
[root@localhost test]# cat ln2013 
hostnamebaidu=baidu.com
hostnamesina=sina.com
hostnames=true
```
说明：

1. 源文件被删除后，并没有影响硬链接文件；软链接文件在centos系统下不断的闪烁，提示源文件已经不存在
2. 重建源文件后，软链接不在闪烁提示，说明已经链接成功，找到了链接文件系统；重建后，硬链接文件并没有受到源文件影响，硬链接文件的内容还是保留了删除前源文件的内容，说明硬链接已经失效

####实例4：将文件链接为另一个目录中的相同名字
命令：
```
ln log2013.log test3
```
输出：
```
[root@localhost test]# ln log2013.log test3
[root@localhost test]# ll
lrwxrwxrwx 1 root root     11 12-07 16:01 link2013 -> log2013.log
-rw-r--r-- 1 root bin      61 11-13 06:03 ln2013
-rw-r--r-- 2 root root     96 12-07 16:21 log2013.log
[root@localhost test]# cd test3
[root@localhost test3]# ll
-rw-r--r-- 2 root root 96 12-07 16:21 log2013.log
[root@localhost test3]# vi log2013.log 
2013-01
2013-02
2013-03
2013-04
2013-05
2013-06
2013-07
2013-08
2013-09
2013-10[root@localhost test3]# ll
-rw-r--r-- 2 root root 80 12-07 16:36 log2013.log
[root@localhost test3]# cd ..
[root@localhost test]# ll
lrwxrwxrwx 1 root root     11 12-07 16:01 link2013 -> log2013.log
-rw-r--r-- 1 root bin      61 11-13 06:03 ln2013
-rw-r--r-- 2 root root     80 12-07 16:36 log2013.log
[root@localhost test]#
```
说明：
在test3目录中创建了log2013.log的硬链接，修改test3目录中的log2013.log文件，同时也会同步到源文件

####实例5：给目录创建软链接
命令：
```
ln -sv /opt/soft/test/test3 /opt/soft/test/test5
```
输出：
```
[root@localhost test]# ll
drwxr-xr-x 2 root root   4096 12-07 16:36 test3
drwxr-xr-x 2 root root   4096 12-07 16:57 test5
[root@localhost test]# cd test5
[root@localhost test5]# ll
lrwxrwxrwx 1 root root 5 12-07 16:57 test3 -> test3
[root@localhost test5]# cd test3
-bash: cd: test3: 符号连接的层数过多
[root@localhost test5]# 
[root@localhost test5]# 
[root@localhost test5]# ll
lrwxrwxrwx 1 root root 5 12-07 16:57 test3 -> test3
[root@localhost test5]# rm -rf test3
[root@localhost test5]# ll
[root@localhost test5]# ln -sv /opt/soft/test/test3 /opt/soft/test/test5
创建指向“/opt/soft/test/test3”的符号链接“/opt/soft/test/test5/test3”
[root@localhost test5]# ll
lrwxrwxrwx 1 root root 20 12-07 16:59 test3 -> /opt/soft/test/test3
[root@localhost test5]# 
[root@localhost test5]# cd test3
[root@localhost test3]# ll
总计 4
-rw-r--r-- 2 root root 80 12-07 16:36 log2013.log
[root@localhost test3]# touch log2014.log
[root@localhost test3]# ll
总计 4
-rw-r--r-- 2 root root 80 12-07 16:36 log2013.log
-rw-r--r-- 1 root root  0 12-07 17:05 log2014.log
[root@localhost test3]# cd ..
[root@localhost test5]# cd ..
```
说明：

1. 目录只能创建软链接
2. 目录创建链接必须用绝对路径，相对路径创建会不成功，会提示：符号连接的层数过多 这样的错误
3. 在链接目标目录中修改文件都会在源文件目录中同步变化
