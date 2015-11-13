###1 date命令格式
date命令的作用就是显示和设置系统日期和时间。该命令的一般格式为： 

```
date [选项] 显示时间格式（显示时间格式以+开头，后面接格式）
```

date命令中的各选项的含义分别为：

* -d datestr, --date datestr 显示由datestr描述的日期 
* -s datestr, --set datestr 设置datestr 描述的日期 
* -u, --universal 显示或设置通用时间 时间域 

时间格式中：

* % y 年的最后两个数字（ 1999则是99） 
* % Y 年（例如：1970，1996等）
* % m 月（01..12） 
* % d 一个月的第几天（01..31） 
* % H 小时（00..23） 
* % I 小时（01..12） 
* % k 小时（0..23） 
* % l 小时（1..12） 
* % M 分（00..59） 
* % S 秒（00..59） 
* % p 显示出AM或PM 
* % Z 时区 日期域 
* % b 月的简称（Jan..Dec） 
* % h 和%b选项相同 
* % B 月的全称（January..December） 
* % a 星期几的简称（ Sun..Sat） 
* % A 星期几的全称（ Sunday..Saturday） 
* % j 一年的第几天（001..366） 
* % w 一个星期的第几天（0代表星期天） 
* % W 一年的第几个星期（00..53，星期一为第一天） 
* % D 日期（mm／dd／yy） 
* % x 显示日期的格式（mm/dd/yy）
* % r 时间（hh：mm：ss AM或PM），12小时 
* % T 时间（24小时制）（hh:mm:ss） 
* % X 显示时间的格式（％H:％M:％S） 
* % c 日期和时间（ Mon Nov 8 14：12：46 CST 1999） 
* % s 从1970年1月1日00：00：00到目前经历的秒数 
 
注意：

> 需要特别说明的是，只有超级用户才能用date命令设置时间，一般用户只能用date命令显示时间。

###2 使用实例
####1) 使用示例一:
首先进行设置

```
#date //显示当前日期
#date -s //设置当前时间，只有root权限才能设置，其他只能查看。
#date -s 20151113 //设置成20151113，这样会把具体时间设置成空00:00:00
#date -s 12:23:23 //设置具体时间，不会对日期做更改
#date -s “12:12:23 2015-11-13″ //这样可以设置全部时间
```

设置完系统时间后,还需要同步到硬件时钟上
```
# clock --systohc
```
硬件时钟与系统时钟同步：
```
# hwclock --hctosys
```
或者
```
# clock --hctosys
```
上面命令中，--hctosys表示Hardware Clock to SYStem clock。

系统时钟和硬件时钟同步：
```
# hwclock --systohc
```
或者
```
# clock --systohc
```
####2) 使用示例二:
用指定的格式显示时间。 

```
$ date +'This\ date\ now\ is=>%x'
This\ date\ now\ is=>2015/11/13
```

用预定的格式显示当前的时间。 

```
$ date
五 11 13 16:07:41 CST 2015 
```

设置时间为下午14点36分。 

```
# date -s 14:36:00 
五 11 13 14:36:00 CST 2015
```

设置时间为2015年11月12号。 

```
# date -s 151112 
四 11 13 00:00:00 CST 2015 
```

设置一天前
```
date --date "1 days ago" +"%Y-%m-%d"
```

Date 命令参数小技巧
由于Linux对man date -d 参数说的比较模糊,故举例如下:

```
$ -d, --date=STRING  display time described by STRING, not `now'
```

For Linux
```
$ date -d next-day +%Y%m%d
20151114
$ date -d last-day +%Y%m%d
20151112
$ date -d yesterday +%Y%m%d
20151112
$ date -d tomorrow +%Y%m%d
20151114
$ date -d last-month +%Y%m
201510
$ date -d next-month +%Y%m
201512
$ date -d next-year +%Y
2016
```
而FreeBSD则不同;举例如下:
For FreeBSD
```
$ date -v -1d +%Y%m%d
20151112
$  date -v -1m +%Y%m%d 
20151013
$  date -v -1y +%Y%m%d 
20141113
```
####3)使用示例三:
在linux环境下要取得几天前的时期只要使用

```
date -d "x days ago" +%Y%m%d
```

x用数字代替，如果需要几天前的直接写正数，如果要几天后的日期直接写负数即可；

```
date -d "x weeks ago" +%Y%m%d
```

x用数字代替，如果需要几周前的直接写正数，如果要几周后的日期直接写负数即可；

```
date -d "x years ago" +%Y%m%d
```

x用数字代替，如果需要几年前的直接写正数，如果要几年后的日期直接写负数即可；
看下面例子：

```
$ date +%Y%m%d
20151113
```

上面是今天的日期20151113

```
$ date -d "2 days ago" +%Y%m%
20151111
``` 

上面是两天前的日期

```
$ date -d "4 days ago" +%Y%m%d 
20151109
```

上面是四天前的日期

```
$ date -d "-1 days ago" +%Y%m%d
20151114
```

上面是一天后的日期

```
$ date -d "-2 days ago" +%Y%m%d 
20151115
```

上面是两天后的日期

```
$ date -d "1 week ago" +%Y%m%d
20151106
```

上面是一周前的日期

```
$ date -d "1 year ago" +%Y%m%d
20141113
```

上面是一年前的日期

###3 几种时间
* CST：中国标准时间（China Standard Time），这个解释可能是针对RedHat Linux。
* UTC：协调世界时，又称世界标准时间，简称UTC，从英文国际时间/法文协调时间”Universal Time/Temps Cordonn&eacute;”而来。中国大陆、香港、澳门、台湾、蒙古国、新加坡、马来西亚、菲律宾、澳洲西部的时间与UTC的时差均为+8，也就是UTC+8。
* GMT：格林尼治标准时间（旧译格林威治平均时间或格林威治标准时间；英语：Greenwich Mean Time，GMT）是指位于英国伦敦郊区的皇家格林尼治天文台的标准时间，因为本初子午线被定义在通过那里的经线。
