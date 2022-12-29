# [arch Linux & IDEA]JDBC连接数据库

#### 安装 mariadb-jdbc

> 写博客时的版本为3.0.5

`yay -S mariadb-jdbc`



#### 添加 mariadb-jdbc 的jar包

> `mariadb-jdbc` 默认安装在 `/usr/share/java` 路径下

新建或者打开一个项目，点击 `File` -> `Project Structure`

![](/home/kong/Pictures/Screenshot 2022-08-03 031845.png)



点击 `Module` -> `Dependencies` -> `➕` -> `JARs or Directories`

![](/home/kong/Pictures/2022-08-03_10-34.png)



找到 `mariadb-jdbc` 后，选择 `mariadb-java-client-3.0.5.jar` ，再点击 `OK`

![](/home/kong/Pictures/2022-08-03_10-36.png)



勾选上这个jar包，再点击 `OK`

![](/home/kong/Pictures/2022-08-03_10-37.png)



创建一个 `demo1` 类，用以连接数据库，并输入以下代码

```java
import java.sql.Driver;
import java.sql.DriverManager;

public class demo1 {
    public void connect() {
        Driver driver = new org.mariadb.jdbc.Driver();
        DriverManager.registerDriver(driver);
    }
}
```

> 此时我们会发现 `Driver` 和 `registerDriver` 报错了



#### 解决报错

点击一下 `Driver` ，并按下 `alt` + `Enter` 键，点击 `Add library 'mariadb-java-client-3.0.5.jar' to classpath` 

![](/home/kong/Pictures/2022-08-03_10-53.png)

> 此时， `Driver` 报错消失



同样的，我们点击一下 `registerDriver` ，并按下 `alt` + `Enter` 键，点击 `Add exception to method signature` ，

![](/home/kong/Pictures/2022-08-03_11-04.png)

> 此时， `registerDriver` 报错消失
