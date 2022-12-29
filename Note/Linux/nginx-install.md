# 安装依赖

##  `CentOS` 

 `yum install gcc pcre pcre-devel zlib zlib-devel openssl openssl-devel` 



# 下载源码

> 访问 `https://nginx.org/en/download.html` 找到最新的Nginx版本对应的下载链接。
>
> 此处以 `1.22.1` 为例

执行 `cd /usr/local` 进入目录，再执行 `https://nginx.org/download/nginx-1.22.1.tar.gz` 将源码下载到Linux中。



# 编译与安装

执行 `tar -zxvf nginx-1.22.1.tar.gz` 解压，再执行 `cd nginx-1.22.1` 进入解压后的目录内。

执行 `./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-http_stub_status_module` 进行配置

执行 `make && make install` 进行编译，并将其安装到 `/usr/local/nginx` 目录中



# 添加软链接

 `ln -s nginx的安装位置/sbin/nginx /usr/bin/nginx` 

-  `ln -s /usr/local/nginx/sbin/nginx /usr/bin/nginx` 

 `ln -s nginx的安装位置/conf/ /etc/nginx` 

-  `ln -s /usr/local/nginx/conf/ /etc/nginx` 



# 最后

执行 `nginx -help` 查看前面的所有操作是否成功

