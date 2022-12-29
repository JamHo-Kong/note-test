# 安装MariaDB数据库

##### 安装MariaDB

`sudo pacman -S mariadb`



##### 初始化MariaDB

`sudo mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql`



##### 启动

`sudo systemctl start mysqld`



##### 为root用户设置密码

`sudo mysqladmin -u root password "你的密码"`



##### 登录mysql

`mysql -u root -p`



##### 切换到mysql数据库

`USE mysql;`



##### 设置权限

`GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '你的密码';`



##### 退出

`EXIT;`



##### 加入守护进程

`sudo systemctl enable mysqld`