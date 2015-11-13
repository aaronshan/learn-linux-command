tr指令从标准输入设备读取数据，经过字符串转译后，输出到标准输出设备。

1. cat filename | tr u n ：用于在屏幕上将filename文件中的u替换为n，而实际文件中未作替换
2. cat filename | tr -d abc 在屏幕上将filename内容中的所有出现的a或b或c字符删去，并显示出来
3. cat filename | tr [:lower:] [:upper:] 将文件内容中的小写全部变为大写

类似于[:lower:]的代替符号还有：
* [:alnum:] 表示所有的字母和数字
* [:alpha:] 表示所有的字母
* [:blank:] 表示所有空格
* [:digit:] 表示所有数字
* [:graph:] 表示所有可打印字符，但不包括空格
* [:print:] 表示所有可打印字符，包括空格
