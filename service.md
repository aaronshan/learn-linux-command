service, 顾名思义，就是用于管理linux操作系统中服务的命令。

###注意事项
* 声明：该命令不是所有linux发行版都有，主要在redhat、fedora、centos中。
* 该命令位于/sbin目录下，用file命令查看此命令会发现它是一个脚本命令。
* 分析脚本可知此命令的作用是去/etc/init.d目录下寻找相应的服务，进行开启和关闭等操作。
* 开启|关闭|重启|重新载入配置 httpd服务器：service httpd start|stop|restart|reload
* 关闭mysql服务器：service mysqld stop
* 强烈建议将service命令替换为/etc/init.d/mysqld stop （因为不是所有发行版都有service命令，为了通用）
