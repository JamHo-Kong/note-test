# 安装依赖

##  `CentOS` 

 `yum -y install curl-devel expat-devel gettext-devel openssl-devel zlib-devel` 



# 下载源码

> 访问 `https://git-scm.com/` 查看最新的Git版本，再访问 `https://mirrors.edge.kernel.org/pub/software/scm/git/` 找到对应的下载链接。
>
> 此处以 `2.39.0` 为例

执行 `cd /usr/local` 进入目录，再执行 `wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.39.0.tar.gz` 将源码下载到Linux中。



# 编译与安装

执行 `tar -zxvf git-2.39.0.tar.gz` 解压，再执行 `cd git-2.39.0` 进入解压后的目录内。执行 `make` 进行编译

执行 `make install prefix=/usr/local/git` 将Git安装到 `/usr/local/git` 目录中



# 配置环境变量

执行 `vim /etc/profile` ，在文件最后添加以下内容

```sh
export PATH=$PATH:/usr/local/git/bin
```

执行 `source /etc/profile` 让配置立即生效



# 最后

执行 `git -v` 查看前面的所有操作是否成功

