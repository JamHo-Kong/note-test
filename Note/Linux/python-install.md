# 安装依赖

##  `CentOS` 

 `yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make libffi-devel` 



# 下载源码

> 访问 `https://www.python.org/downloads/` 查看最新的Python版本，再访问 `https://www.python.org/downloads/source/` 找到该版本 `XZ compressed source tarball` 的下载链接
>
> 此处以 `3.11.1` 为例

执行 `cd /usr/local` 进入目录，再执行 `wget https://www.python.org/ftp/python/3.11.1/Python-3.11.1.tar.xz` 将源码下载到Linux中。



# 编译与安装

执行 `tar -Jxvf Python-3.11.1.tar.xz` 解压，再执行 `cd Python-3.11.1` 进入解压后的目录内。

执行 `./configure --prefix=/usr/local/Python/Python-3.11.1 && make` 进行编译

执行 `make install` 将Python安装到 `/usr/local/Python/Python-3.11.1` 目录中



# 配置环境变量

执行 `vim /etc/profile` ，在文件最后添加以下内容

```sh
export PYTHON_HOME=/usr/local/Python/Python-3.11.1
export PATH=$PATH:$PYTHON_HOME/bin
```

执行 `source /etc/profile` 让配置立即生效



# 最后

- 执行 `cd /usr/local/python3.11.1/bin && mv python3 python` 将 `python3` 更名为 `python` ，那么在终端输入 `python` 即可呼出Python，而不是 `python3` 

- 添加软链接（个人感觉添加环境变量后，这一步不是必要的）

  ```bash
  ln -s /usr/local/Python/Python3.11.1/bin/python3.11 /usr/bin/python
  ln -s /usr/local/Python/Python3.11.1/bin/pip3.11 /usr/bin/pip3
  ```

 **执行 `python --version` 查看前面的所有操作是否成功** 



```
# Java
JDK_8=/usr/local/Java/JDK/jdk-1.8.0-351
JDK_18=/usr/local/Java/JDK/jdk-18.0.2.1
export JAVA_HOME=$JDK_18
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

