文件a内容如下：
```
$ cat a.txt 
1
2
4
6
8
```
文件b内容如下：

```
$ cat b.txt 
2
3
4
8
```

方法1）使用comm命令
comm命令比较两个排好序且内容唯一的文件a和b。不加可选参数时，会输出3列，第一列是a-b，第二列是b-a，第三列是a交b：
```
$ comm a.txt b.txt 
1
                2
       3
                4
6
                8
```
如果仅仅只需要交集，则执行如下命令即可：

```
$ comm -12 a.txt b.txt
2
4
8
```
其中，-12代表去掉第1、2列。如果a.txt、b.txt文件没有排好序且唯一，则可使用如下命令：
```
comm -12 &lt;(sort a.txt | uniq) &lt;(sort b.txt | uniq)
2
4
8
```
如果想要求a-b，则只需要使用如下命令：
```
comm -23 &lt;(sort a.txt | uniq) &lt;(sort b.txt | uniq)
1
6
```

方法2）使用grep命令
```
$ grep -F -f a.txt b.txt 
2
4
8
```
想要求a-b，使用如下命令：

```
$ grep -F -v -f b.txt a.txt 
1
6
```
而b-a，使用如下命令：

```
$ grep -F -v -f a.txt b.txt 
3

```
