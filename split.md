当面临将一个大文件进行切分时，linux的split命令是很好的选择。它包含多种参数，支持按行、大小进行切分。
split命令的语法如下：
```
split [--help][--version][-a ][-b][-C ][-l ][要切割的文件][输出文件名前缀]  
```
对应的参数描述如下：
* -a, --suffix-length=N      使用的后缀长度 (默认为 2)  
* -b, --bytes=SIZE      每个输出文件的字节大小  
* -C, --line-bytes=SIZE      每个输出文件每行的最大字节大小  
* -d, --numeric-suffixes      使用数字后缀代替字母后缀  
* -l, --lines=NUMBER      设定每个输出文件的行数  
* --help 显示帮助信息  
* --version      显示版本信息  

下面举几个例子：

1）将文件splitTest.txt分割成多个文件，分割后的每个文件大小为10M。命令：
```
$ split -b 20m splitTest.txt  
$ ls  
splitTest.txt  xaa  xab  xac  
```

2）将文件splitTest.txt分割成多个文件，分割后的每个文件大小为10M。指定分割后的文件前缀位split，命令：
```
$ split -b 20m splitTest.txt  split  
$ ls  
splitaa  splitab  splitac  splitTest.txt  
```

3）将文件splitTest.txt分割成多个文件，每个文件50万行。命令：
```
$ wc -l splitTest.txt   
1502216 splitTest.txt  
$ split -l 500000 splitTest.txt  split  
$ ls  
splitaa  splitab  splitac  splitad  splitTest.txt  
```

4）将文件splitTest.txt分割成多个文件，每个文件50万行。指定分割后的文件后缀为数字，数字位数为3位，命令：
```
$ wc -l splitTest.txt   1502216 splitTest.txt  
$ split -l 500000 -d -a 3 splitTest.txt  split  
$ ls  
split000  split001  split002  split003  splitTest.txt  
```

可以使用cat命令将切分后的文件合并成新的文件：
```
$ cat split0* > original.txt  
```

