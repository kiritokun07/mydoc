## rocketmq



## Win10环境下配置RocketMQ

https://zhuanlan.zhihu.com/p/99981354

RocketMQ官网

http://rocketmq.apache.org/

下载二进制
https://www.apache.org/dyn/closer.cgi

清华镜像
https://mirrors.tuna.tsinghua.edu.cn/apache/

环境变量配置，变量名ROCKETMQ_HOME 变量值，二进制文件解压后地址

注意：RocketMQ NAMESERVER默认分配的jvm参数需要占用较大内存，但是一般来说我们自己用不需要占用这么大内存

更改bin目录下的这两个文件的相应部分 runbroker.cmd runserver.cmd

注意给第一行的&&加上双引号，不然启动broker会报错

p26 `set CLASSPATH=.;%BASE_DIR%conf;"%CLASSPATH%"`

p31 `set "JAVA_OPT=%JAVA_OPT% -server -Xms256m -Xmx256m -Xmn128m"`

## rocketmq插件安装

git clone https://github.com/apache/rocketmq-externals

解压之后进入rocketmq-externals\rocketmq-console\src\main\resources文件夹，打开application.properties进行配置



如果broker不是第一次启动却失败了，可以把Administrator下的store删了
应该是程序不正常中断又无法恢复之前运行状态

