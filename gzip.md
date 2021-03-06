1 下载了一个源码文件abc.tar.gz或abc.tgz（后缀tar.gz和tgz基本没啥区别，相同的还有.taz和.tar.Z）
```
# tar -xzvf abc.tar.gz 
```
或
```
# tar -xzvf abc.tgz
```
注释：
> tar.gz和tgz是经过归档并由gzip工具压缩之后所得到的压缩包。
> * x选项表示解压缩
> * z表示用gzip工具进行解压缩
> * v表示在解压缩时显示详细信息
> * f表示指定文件（请注意，这个选项一定要放在各个选项的最后，也就是要和所指定的文件名挨得最近）

2 遇到一个文件xyz.gz，想解压缩：
```
# gzip -d xyz.gz
```
注释：d选项表示解压缩
3 遇到一个文件edf.tar，想解压缩：（严格的讲tar文件是归档文件，并未被压缩，这里提到的“解压缩”只是将tar文件拆开而已）：
```
# tar -xvf edf.tar
```
注释：
> 其实这个命令不是压缩命令范畴，只是在这里提一下：）

4 想将一个文件夹dirabc压缩成.tar.gz的压缩文件：
```
# tar -czvf dirabc.tar.gz dirabc
```
5 想查看一下下载的abc.tar.gz压缩文件里包含哪些文件
```
# tar -ztvf abc.tar.gz
```
6 用5的方法查看到abc.tar.gz压缩包，其中包括def/xyz.txt文件等很多文件，但只想提取出xyz.txt这一个文件
```
＃ tar -xzvf abc.tar.gz def/xyz.txt
```
7 解压abc.tar.gz时我想保留原来被压缩文件的权限（常用于备份）
```
＃ tar -xzvpf abc.tar.gz
```
8 我想压缩得最快，代价是压缩比最高
```
# gzip -1 abc.tar
```
注释：
> -1也可以换成–fast；-9表示压缩比高，但速度最慢，-9也可以用–best代替。默认的是-6（数字不好记的话，可以这样记：1表示一步到位，往往一步到位的东西追求的是快，而不是精致程度）

