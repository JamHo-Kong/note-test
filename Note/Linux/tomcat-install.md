# JDK

## 安装

> 使用超级用户权限执行以下命令，已安装则跳过此步骤

执行 `rpm -qa | grep jdk
` 、 `rpm -qa | grep java` 检查系统中有没有已经安装好了的java环境。如果没有输出，则表示没有安装

- 卸载请使用命令 `rpm -e --nodeps 要卸载的包名` 

###  `CentOS` 

1. 执行 `yum search java` 查看有哪些jdk可以安装
2. 执行 `yum install java-1.8.0-openjdk` 安装
   -  `java-1.8.0-openjdk` 可以替换成第一步中列出的包名。
3. 执行 `yum install java-1.8.0-openjdk-devel` ，安装 `javac` 需要的包



###  `arch Linux` 





### 解压包

> 自己找JDK的包，并解压到自己指定的位置。以Oracle官方为例

首先在 `/usr/local` 目录下新建 `jdk` 目录，进入 `jdk` 目录

访问 `https://www.oracle.com/java/technologies/java-se-glance.html` ，选择需要的JDK版本，并下载对应系统版本的 `tar.gz` 包。

- 这里选择JDK1.8进行安装，访问 `https://www.oracle.com/java/technologies/downloads/#java8` ，选择下载Linux下的 `x64 Compressed Archive` 。
  - 注意，你可以选择在本地下载好了之后，传到Linux中。也可以在 `jdk` 目录下，执行命令 `wget 下载链接` 直接将 `tar.gz` 包下载到Linux中
    - 本地下载：打开cmd或者powershell，执行命令 `scp 你下载的tar.gz包的本地路径 Linux用户名@Linux主机的IP地址:目标目录` 。例如， `scp C:\Users\sample\Downloads\jdk-8u351-linux-x64.tar.gz root@1.11.111.2:/usr/local/jdk` ，就是将我下载的 `tar.gz` 包传到 `/usr/local/jdk` 中，如果信息正确，则会向你索要root用户的密码。这个文件的属主和属组都是root。
    - wget下载：进入 `/usr/local/jdk` ，执行 `wget 下载链接` 即可。

执行命令 `ll` 查看下载的包的名字，此处我的包名是 `jdk-8u351-linux-x64.tar.gz` ，再执行 `tar -zvxf jdk-8u351-linux-x64.tar.gz` 解压包。

- 执行 `mv jdk1.8.0_351 jdk8` 将解压后的文件夹更名为 `jdk8` 



## 配置环境变量

> 全局的环境变量修改 `/etc/profile` ，单个用户的环境变量修改 `~/.bash_profile` 

执行 `vim /etc/profile` ，在文件最后面添加以下内容

```sh
export JAVA_HOME=/usr/local/jdk/jdk8	## 上一步中JDK的解压目录
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

执行 `source /etc/profile` 立即刷新。

此时使用任何用户在任何目录下都可以执行 `java` 和 `javac` 命令



# Tomcat

## 安装

> 与JDK的安装基本一致，绝大多数通过解压包来安装的软件

首先在 `/usr/local` 目录下新建 `tomcat` 目录。

访问 `https://tomcat.apache.org/download-10.cgi` ，下载Core下的 `tar.gz` 包。

- 注意，你可以选择在本地下载好了之后，传到Linux中。也可以在 `tomcat` 目录下，执行命令 `wget 下载链接` 直接将 `tar.gz` 包下载到Linux中
  - 本地下载：打开cmd或者powershell，执行命令 `scp 你下载的tar.gz包的本地路径 Linux用户名@Linux主机的IP地址:目标目录` 。例如， `scp C:\Users\sample\Downloads\.tar.gz root@1.11.111.2:/usr/local/jdk1.8` ，就是将我下载的 `tar.gz` 包传到 `/usr/local/tomcat10` 中，如果信息正确，则会向你索要root用户的密码。这个文件的属主和属组都是root。
  - wget下载：进入 `/usr/local/tomcat` ，执行 `wget 下载链接` 即可。

执行命令 `ll` 查看下载的包的名字，此处我的包名是 `apache-tomcat-10.0.27.tar.gz` ，再执行 `tar -zvxf apache-tomcat-10.0.27.tar.gz` 解压包

- 执行 `mv apache-tomcat-10.0.27 tomcat10` 将解压后的文件夹更名为 `tomcat10` 

## 配置环境变量

执行 `vim /etc/profile` ，在文件最后面添加以下内容

```sh
export CATALINA_HOME=/usr/local/tomcat/tomcat10
```



## 设定Tomcat开机自启

执行 `cd /etc/init.d` 进入 `init.d` 目录，再执行 `touch tomcat && vim tomcat` ，粘贴一下内容

```sh
#!/bin/sh
# chkconfig: 345 99 10  
# description: Auto-starts tomcat  
# /etc/init.d/tomcatd  
# Tomcat auto-start  
# Source function library.  
 . /etc/init.d/functions  
# source networking configuration.  
 . /etc/sysconfig/network  
RETVAL=0  
  
export JAVA_HOME=/usr/local/jdk/jdk18		# 你安装jdk的目录
export CATALINA_HOME=/usr/local/tomcat/tomcat10		#这两行都写安装tomcat的目录
export CATALINA_BASE=/usr/local/tomcat/tomcat10
  
start()  
{  
        if [ -f $CATALINA_HOME/bin/startup.sh ];  
          then  
            echo $"Starting Tomcat"  
                $CATALINA_HOME/bin/startup.sh  
            RETVAL=$?  
            echo " OK"  
            return $RETVAL  
        fi  
}  
stop()  
{  
        if [ -f $CATALINA_HOME/bin/shutdown.sh ];  
          then  
            echo $"Stopping Tomcat"  
                $CATALINA_HOME/bin/shutdown.sh  
            RETVAL=$?  
            sleep 1  
            ps -fwwu tomcat | grep apache-tomcat|grep -v grep | grep -v PID | awk '{print $2}'|xargs kill -9  
            echo " OK"  
            # [ $RETVAL -eq 0 ] && rm -f /var/lock/...  
            return $RETVAL  
        fi  
}  
case "$1" in  
 start)  
        start  
        ;;  
 stop)  
        stop  
        ;;  
 restart)  
         echo $"Restaring Tomcat"  
         $0 stop  
         sleep 1  
         $0 start  
         ;;  
 *)  
        echo $"Usage: $0 {start|stop|restart}"  
        exit 1  
        ;;  
esac  
exit $RETVAL
```

执行 `chkconfig --add tomcat` ，将tomcat添加到自动启动中。

- 执行 `chkconfig --list` 检查是否添加成功

之后可以通过 `chkconfig tomcat on/off` 来控制tomcatshi'fou