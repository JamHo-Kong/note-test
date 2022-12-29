# 删除已存在的包

```bash
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

# 安装 `yum-utils` 

```bash
sudo yum install -y yum-utils
```

# 配置 `yum` 

```bash
sudo yum-config-manager \
--add-repo \
http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

# 安装Docker

```bash
sudo yum install -y docker-ce docker-ce-cli containerd.io
```



# 加入守护进程

> 开机自启

```bash
systemctl enable docker --now
```

# 配置镜像加速

1. 注册阿里云
2. 导航栏点击 `产品` -> `容器与中间件` -> `容器镜像服务 ACR` -> `管理控制台` -> `镜像工具` -> `镜像加速器` 





# 常用命令

## 拉取镜像

```bash
## 未指定版本的，将拉取最新版本
docker pull package-name[:version]
```

## 删除镜像

```bash
## IMAGE ID
docker rmi IMAGE ID
```







# 问题

## 情况

报 `Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/images/json": dial unix /var/run/docker.sock: connect: permission denied` 错误

## 解决

添加名为docker的用户组，并将当前用户添加到该用户组中

```bash
sudo gpasswd -a ${USER} docker	## 将当前用户加入到docker用户组
sudo newgrp docker	## 更新docker用户组
```

