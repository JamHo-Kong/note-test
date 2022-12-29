# 连接到服务器

> 使用openssh

 `ssh 用户名@IP地址` 



# 基础安装

## 安装依赖环境

 `sudo yum install -y ca-certificates` 

 `sudo yum -y install epel-release` 

 `sudo yum -y install git` 

 `sudo yum install -y vim python3-pip` 

## 更新 `pip` 

 `pip3 install --upgrade pip` 

## 安装 `docker-compose` 

 `pip install docker-compose` 

## 安装 Docker

 `curl -sSL https://get.daocloud.io/docker | sh` 

## 授权 `docker-compose` 

 `chmod +x /usr/local/bin/docker-compose` 

### 可能的问题 <span id=kenengdewenti> </span> 

报错 `/usr/local/bin/docker-compose 找不到或不存在` 

> 原因是我们没有将 `docker-compose` 安装到 `/usr/local/bin` ，只要找到 `docker-compose` 并把它移到 `/usr/local/bin` 目录下即可解决此报错

1. 执行命令 `whereis docker-compose` ，将输出的路径复制下来
2. 执行命令 `mv 上一步输出的路径 /usr/local/bin` ，即可将 `docker-compose` 移动到 `/usr/local/bin` 目录下，此时再执行 `chmod +x /usr/local/bin/docker-compose` 就不会报错了



# 部署OJ

## 拉取项目

 `git clone https://gitee.com/himitzh0730/hoj-deploy.git && cd hoj-deploy` 

## 配置OJ

1. 进入 `standAlone` 文件夹

    `cd standAlone` 

2. 编辑安装前配置文件

   > 我推荐使用nano，即 `nano .env` 
   >
   > nano操作起来更简单

    `vim .env` 

3. 修改以下内容

   > 各项配置请看文件内说明，一般请修改默认为hoj123456的三个密码配置项即可，此处的修改如下：

   缓存Redis的密码配置项： `REDIS_PASSWORD=CloudCode0417.` 

   数据库MySQL的密码配置项： `MYSQL_ROOT_PASSWORD=CloudCode0417.` 

   服务注册中心Nacos的密码配置项： `NACOS_PASSWORD=CloudCode0417.` 

   端口号要进行修改，防止冲突： `Mysql_Public_Port=3307` 

![img](https://cdn.nlark.com/yuque/0/2022/png/21581623/1666250363645-e24f1bc8-95fb-4375-aca5-06284d6c1848.png)

## 开始安装部署

 `docker-compose up -d` 

- 该命令执行完成后就会出现如下内容

![img](https://cdn.nlark.com/yuque/0/2022/png/21581623/1666252548968-86424d08-5c26-4d2a-b4b9-3570ad5c5443.png)

即代表安装和启动均成功

## 访问项目

由于我们使用了特殊的端口，后续还需要在 `docker-compose.yml` 中进行二次修改

>  `docker-compose.yml` 文件在 `hoj-deploy/standAlone` 路径下

1. 特殊端口的修改方式，在140-141行，把原有端口更改为特殊端口， **请勿修改错误，修改的是冒号前面的内容** 

![img](https://cdn.nlark.com/yuque/0/2022/png/21581623/1666252726323-6aec0911-ce77-4335-8794-f54da3127f18.png)

保存，然后执行 `docker-compose.yml` 

### 本地访问

1. 执行 `docker ps` 查看端口是否修改成功
2. 如无意外，会看到 `0.0.0.0:你刚刚修改的端口号` ，访问这个端口即可

### 外部访问

与上面相似， `服务器IP:你刚刚修改的端口号` 

## 关闭容器

进入到 `docker-compose.yml` 所在文件夹,执行 `docker-compose down -v` 



# 另一些问题

## docker报错

### 重新安装docker-compose

```bash
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.5/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

### 重新授权

 `chmod +x /usr/local/bin/docker-compose` 

### 启动Docker

`service docker start`    启动服务

`systemctl enable docker.service`    生成自启动服务

 **或者** 

 `systemctl enable --now docker.service` 

### 查看状态

> 按Ctrl+C退出

 `systemctl status docker.service` 

### 再次部署

> 预计等待5分钟

 `docker-compose up -d` 



# 最后

如果需要进行新部署的机器的转移，请参考下面链接的文档

> Git项目文档

https://docs.hdoi.cn/deploy/how-to-backup/

那么，如果需要备份，只需将该hoj文件夹复制一份即可，在新的机器上重新部署新的hoj的时候，将该文件夹放置与 `docker-compose.yml` 一个目录下，使用 `docker-compose up -d` 即可启动恢复原来的数据。

**Tips**

注意：在新机器上启用备份的数据的操作顺序如下：

1. 先将hoj文件夹先复制到 `~/hoj-deploy/standAlone` 目录里面（保证该目录无hoj文件夹，干净！）
2. 然后修改 `.env` 文件的配置，主要是Redis，Nacos，MySQL等的密码配置，与原先备份hoj文件夹时的老机器的配置一致！
3. 最后再使用 `docker-compose up -d` 启动即可