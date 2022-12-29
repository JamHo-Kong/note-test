# 安装snap

##  `CentOS` 

 `sudo yum install snapd` 

### 启用管理systemd单元

 `sudo systemctl enable --now snapd.socket` 

### 添加软连接

 `sudo ln -s /var/lib/snapd/snap /snap` 



# 安装Certbot

## 更新snap

 `sudo snap install core; sudo snap refresh core` 

## 安装

 `sudo snap install --classic certbot` 

## 添加软连接

 `sudo ln -s /snap/bin/certbot /usr/bin/certbot` 



# ssl证书

## 仅获取

 `sudo certbot certonly --nginx --email sample@email.com --agree-tos -d 域名1 -d 域名2` 

## 获取并安装

 `sudo certbot --nginx --email sample@email.com --agree-tos -d 域名1 -d 域名2` 

- 报错 `The nginx plugin is not working; there may be problems with your existing configuration.
  The error was: NoInstallationError("Could not find a usable 'nginx' binary. Ensure nginx exists, the binary is executable, and your PATH is set correctly.")` ，即找不到设备上的nginx
  - 添加软链接即可 `ln -s nginx的安装位置/sbin/nginx /usr/bin/nginx` 、 `ln -s nginx的安装位置/conf/ /etc/nginx` 



## 自动更新

> certbot提供自动更新ssl的功能，当证书有效期小于30天时会自动执行更新

执行 `sudo systemctl status certbot.timer` 检查该功能是否正常运行



# 日常使用中可能用到的命令

## 手动更新

 `sudo certbot renew --dry-run` 



## 查看服务器安装了哪些网站的证书

 `sudo certbot certificates` 



## 运行测试环境

在命令后添加 `--dry-run` （注意要使用空格隔开）即可
