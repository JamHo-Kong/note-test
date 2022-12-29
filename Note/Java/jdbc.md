# Java连接数据库（JDBC编程六步）

## 注意 <span id = "zhuyi"> </span> 

- **本文以 `MariaDB` 为例，如果中途看到不懂的，又没有注明 “自行CSDN“ ，请继续往下看，后面会有解释**
- **在食用本文前，建议先看我的另一篇 [文章](https://blog.csdn.net/weixin_43801670/article/details/126137077?spm=1001.2014.3001.5501) ，学习如何导入 `mariadb-jdbc` 的 jar 包**
  -  `MySQL` 也可参考上文，两者仅仅是获取jar包的方法不同，请自行前往 [官网](https://dev.mysql.com/downloads/connector/j/) 下载官方 `jdbc` 的 jar 包![](/home/kong/Desktop/2022-08-09.png)
- **请根据自己安装的数据库版本下载对应的 jar 包，否则将无法连接到数据库**
  - 如果你的数据库是 `MySQL` ，且版本高于6.0，那么连接数据库时，请将本文中的 `org.mariadb.jdbc.Driver` 改为 `com.mysql.cj.jdbc.Driver` 
  - 如果你的数据库是 `MySQL` ，且版本低于6.0，那么连接数据库时，请将本文中的 `org.mariadb.jdbc.Driver` 改为 `com.mysql.jdbc.Driver` 

- 文中可能有但不限于术语错误、代码错误，请在下方留言，确认后我会尽快修改




## 准备工作

### 新建名为 JDBC 的项目



### 导包

> 见 [注意](#zhuyi) 第二点



### 新建名为 mariadb 的 Module 



### 在 mariadb 的 src 下新建两个类（class）

- connection01：用来写方法一
- connection02：用来写方法二





## 概念

> 该部分详细讲解JDBC编程的六步，并带你了解一些概念

Java程序员：调用者，面向JDBC接口编程

SUN公司：制定JDBC接口，具体实现类由各大厂商完成

数据库厂家：编写JDBC接口实现类，称为驱动

下标：JDBC中的下标都是从 `1` 开始



### **注册驱动**

> 通知Java程序，要连接的是哪一个品牌的数据库

#### 方法一

```java
// 导包
import java.sql.Connection;
import java.sql.Statement;
import java.sql.ResultSet;
import java.sql.Driver;
import java.sql.DriverManager;
import java.sql.SQLException;

public static void main(String[] args){
    // 一些下文要用到的东西，最重要的是为释放资源部分做铺垫
    Connection conn = null;	// 连接对象
    Statement stmt = null;	// 数据库操作对象
    ResultSet rs = null;	// 结果集
    
    try {
    	Driver driver = new org.mariadb.jdbc.Driver();
		DriverManager.registerDriver(driver);	// 注册驱动
	} catch (SQLException e) {
    	e.printStackTrace();
	}
}
```

- 第16行报错，将光标移到Driver()上，点击 `Alt` + `Enter` ，点击第一个选项 `Add library 'mariadb-java-client-3.0.5.jar' to classpath` 即可

- 因为第17行有异常，所以使用 `try{...}catch{...}` 捕捉异常，之后步骤的代码都是写在 `try{...}` 里面的

  - 我们通常把可能存在异常的代码写在 `try{...}` 里， `catch{...}` 捕捉这些异常，并且根据代码块里的代码处理异常，在这里，我们选择把异常输出到控制台 `e.printStackTrace();` 

  - 如果 `try{...}` 里的代码有错误，就会直接跳出 `try{...}` ，执行 `try{...}` 后面的代码。就像 `swith` 语句中遇到了 `break` 一样

- 写出导包的代码，目的是方便大家了解这段代码中用到了哪些包，之后的代码中除非是没有导过的包，不然不会再写导包的代码了。实际上，IDEA会自动帮我们导包，如果你的IDEA不会自动导包，请自行百度如何开启该功能。

  - 热芝士：你还可以通过键入下面一行代码，一次性导入 `sql` 下的所有包

     `import java.sql.*;` 




#### 方法二（推荐）

> 此处使用到反射机制，故推荐使用此方法注册驱动

```java
import java.sql.Connection;
import java.sql.Statement;
import java.sql.ResultSet;
import java.sql.SQLException;

public static void main(String[] args) {
    Connection conn = null;
    Statement stmt = null;
    ResultSet rs = null;
    
    try {
        // 这里注册驱动用到了反射，下文还会提到类加载，学完Java SE就知道啥是反射和类加载了，你也可以马上CSDN
        Class.forName("org.mariadb.jdbc.Driver");
    } catch (ClassNotFoundException e) {	// 处理Class.forName()的异常
        e.printStackTrace();
    } catch (SQLException e) {
    	e.printStackTrace();	// 解决之后代码中基于conn、stmt、rs的操作出现的异常
	}
}
```

- 因为第12行有异常，所以使用 `try{...}catch{...}` 捕捉异常

- 我们可以把第14行到第18行改写成

  ```java
  } catch (Exception e) {
  	e.printStackTrace();
  }
  ```

  



### 获取连接

> 开启JVM进程与MySQL进程之间的通道

以连接本地数据库为例

```java
String url = "jdbc:mariadb://localhost:3306/stu?serverTimezone=UTC";
String user = "root";
String passwd = "你的MariaDB密码";

// 注意， DriverManager 提供了两种 getConnection() 方法，此处使用的是有三个参数的方法，两个变量的方法请自行了解
// 为什么 DriverManager 能提供两个同名的方法呢？这个就是方法重载，也请自行了解
conn = DriverManager.getConnection(url, user, passwd);
```

-  `url` ：统一资源定位符，包含通信协议、IP地址、端口号、资源名
  -  `jdbc:mariadb` ：通信协议
  -  `localhost` ：IP地址
  -  `3306` ：端口号
  -  `stu` ：资源名，要连接的数据库的名字
  -  `serverTimezone=UTC` ：Windows下使用高版本mysql数据库时，建议加上时区，避免执行SQL语句时出现问题



### 获取数据库操作对象

> 创建Statement对象，它是专门用来执行SQL语句

```java
stmt = conn.createStatement();
```



### 执行SQL语句

#### DML

```java
Stirng sql = "INSERT INTO VALUES ('000','Name','1997-07-01','M')";

statement.executeUpdate(sql);
```

-  `executeUpdate()` 
  - 执行的 `DML` 语句，即 `INSERT` 、 `DELETE` 、 `UPDATE` 
  - 返回值为 `int` 类型，表示当前语句执行后受影响的行数
- JDBC中的SQL语句不用加分号



#### DQL

```java
Stirng sql = "INSERT INTO VALUES ('000','Name','1997-07-01','M')";

// 用rs接收返回的结果集
rs = statement.executeQuery(sql);
```

-  `executeQuery()` 
  - 执行 `DQL` 语句，即 `SELECT` 
  - 返回值为 `ResultSet` 类型，内容为查询结果



### 处理查询结果集

> 仅当执行的SQL语句是SELECT语句时才有这一步

```java
// 上一步我们已经接收到了结果集，现在遍历结果集 rs ，查看查询到的结果
while (rs.next()) {
    System.out.println(rs.getString(1));
}
```

-  `ResultSet` 
  -  `next()` 
    -  将光标向下移动一行，返回值为 `Boolean` 类型，下一行有数据返回 `true` ，没有返回 `false` 
  -  `getString(String 字段名)` 
    - 根据 `字段名` 取出，无论该数据在表中是何种类型（ `VARCHAR` 、 `CHAR` 、 `INT` 等等），返回值都为 `String` 类型，正因如此，我们推荐使用该方法，以提高程序健壮性
  -  `getString(int number)` 
    - 返回结果集的第 `number` 列，无论该数据在表中是何种类型（ `VARCHAR` 、 `CHAR` 、 `INT` 等等），返回值都为 `String` 类型
  -  以指定类型取出
    - 指定类型应该与要取出的数据在表中的类型相同。例如， `CHAR` 不能用 `getDouble()` 取出，而 `DOUBLE()` 类型就可以
  
-  `executeQuery()` 
  - 执行 `DQL` 语句，即 `SELETE` 
  - 返回值为 `ResultSet` 类型，是一个结果集



### 释放资源

> 最后创建的进程最先释放

```java
finally {
    try {
		if (rs != null) {	// 如果rs不为空，才执行第4行
            rs.close();
        }
	} catch (SQLException e) {
        e.printStackTrace();
    }
	try {
		if (stmt != null) {	// 如果stmt不为空，才执行第11行
            stmt.close();
        }
	} catch (SQLException e) {
        e.printStackTrace();
    }
    try {
        if (conn != null) {	// 如果conn不为空，才执行第18行
            conn.close();
        }
    } catch (SQLException e) {
        e.printStackTrace();
    }
}
```

- 释放资源时，最先创建的最后关闭
- 此处将 `stmt.close()` 和 `conn.close()` 分开写，是因为如果 `stmt` 关闭异常，程序会自动跳出当前的 `try{...}catch{...}`，导致无法关闭 `conn` 





## 示例代码 <span id = "shilidaima"> </span> 

> 以执行 DML 语句为例，你也可以尝试执行 DQL 语句
>
> 方法一和方法二的不同之处在注册驱动的方法，方法二使用类加载方法注册驱动，该方法较为常用

### 创建数据库

```sql
CREATE DATABASE stu;
```



### 创建表

```sql
CREATE TABLE stuinfo (
    `ID` CHAR(3) NOT NULL UNIQUE PRIMARY KEY,
    `Point` VARCHAR(10) NOT NULL,
    `Name` VARCHAR(5) NOT NULL,
    `Birth` DATE NOT NULL,
    `Gender` CHAR(1) NOT NULL
)DEFAULT CHARSET=utf8;
```



### JDBC

#### 方法一

```java
import java.sql.*;

public class connection01 {
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        String url = "jdbc:mariadb://localhost:3306/stu?serverTimezone=UTC";
        String user = "root";
        String passwd = "你的MariaDB密码";	// 切记改成你的密码
        String sql = "INSERT INTO stuinfo VALUES ('001', '100', 'Yao', '1997-07-01', 'M')";
        
        // 因为第17、20、23、26行有异常，所以使用 try{...}catch{...} 抛出异常
        try {
            // 注册驱动
            Driver driver = new org.mariadb.jdbc.Driver();
            DriverManager.registerDriver(driver);
            
            // 获取连接
            conn = DriverManager.getConnection(url, user, passwd);
            
            // 获取数据库操作对象
            stmt = conn.createStatement();
            
            // 执行SQL语句
            stmt.executeUpdate(sql);
        } catch (SQLException e) {	// 解决异常
            e.printStackTrace();
        } finally {	// 释放资源
            try {
                if (rs != null) {
                    rs.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if (stmt != null) {
                    stmt.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

- 运行程序后，如果出现报错，且报错在第20行，那么请看看你有没有把第10行改成你的密码



#### 方法二（推荐）

```java
import java.sql.*;

public class connection02 {
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        String url = "jdbc:mariadb://localhost:3306/stu?serverTimezone=UTC";
        String user = "root";
        String passwd = "你的MariaDB密码";	// 切记改成你的密码
        String sql = "INSERT INTO stuinfo VALUES ('002', '100', 'Yiu', '1997-07-01', 'M')";
        
        // 因为第16、19、22、25行有异常，所以使用 try{...}catch{...} 抛出异常
        try {
            // 注册驱动
            Class.forName("org.mariadb.jdbc.Driver");
            
            // 获取连接
            conn = DriverManager.getConnection(url, user, passwd);
            
            // 获取数据库操作对象
            stmt = conn.createStatement();
            
            // 执行SQL语句
            stmt.executeUpdate(sql);
        } catch (ClassNotFoundException e) {	// 解决第16行的异常
            e.printStackTrace();
        } catch (SQLException e) {	// 解决第19、22、25行的异常
            e.printStackTrace();
        } finally {	// 释放资源
            try {
                if (rs != null) {
                    rs.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if (stmt != null) {
                    stmt.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
            try {
                if (conn != null) {
                    conn.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

- 运行程序后，如果出现报错，且报错在第19行，那么请看看你有没有把第10行改成你的密码





## 进阶

> 学完上文的代码，相信你已经能够连接到数据库并对其中的数据进行修改了，接下来的部分我们继续完善代码，解决一些问题



### Properties文件

> 配置文件，文件类型为 `*.properties` ，进阶中会用到

#### 为什么要用？

在实际开发中，不建议将数据库的连接信息写死在代码中，因为我们面对的客户可能不懂计算机编程，万一客户想更改数据库连接信息，总不能叫人家改代码吧，那用户体验可就差到家了。所以我们通常会将连接信息写在 `*.properties` 配置文件中，方便用户客户修改。

- 最好是能提供图形化界面，让客户通过该界面间接地修改 `*.properties` 配置文件，避免客户修改源文件，出现误删等问题



#### 怎么用？

> 以JDBC为例

在 mariadb 的 src 下新建 `File` ，并命名为 `jdbc.properties` ，粘贴以下内容：

```
url = jdbc:mariadb://localhost:3306/stu?serverTimezone=UTC
user = root
passwd = 你的MariaDB密码	// 记得改成你的密码
```

用下面的代码替换掉原先的url、user、passwd

```java
import java.util.ResourceBundle;

ResourceBundle bundle = ResourceBundle.getProperties("jdbc");
String url = bundle.getString("url");
String user = bundle.getString("user");
String passwd = bundle.getString("passwd");
conn.DriverManager.getConnection(url, user, passwd);
```





### SQL注入

> 用户输入的信息中含有SQL语句的关键字，并且这些关键字参与了SQL语句的编译过程，导致SQL语句的原意被扭曲，称为SQL注入

#### eg.查询学生信息

我们在 [示例代码](#shilidaima) 的基础上查询班上学生的信息

```java
// 接收学生信息
Scanner s = new Scanner(System.in);
String stuID = s.nextLine();

// 拼接SQL语句
String sql = "SELECT * FROM stuinfo WHERE ID = '"+stuID+"'";

// 获取预编译数据库操作对象
ps = conn.preparement(sql);

// 执行
rs = ps.executeQuery();

// 判断班上有没有这个同学，有输出 yes ，没有输出 no 
if (rs.next()) {
    System.out.println("yes");
} else {
    System.out.println("no");
}
```

运行程序，输入以下示例信息

```bash
asdf' or '1' = '1'
```

这些信息在示例代码中没有被录入数据库，应该是查不到的，但程序运行的结果如下

 `yes` 

**得到错误结果的原因是示例信息中的 `or` 关键字参与了SQL语句的拼接**，得到以下SQL语句

 `SELECT * FROM stuinfo WHERE ID = "asdf" or '1' = '1'` 

 `or` 表示或， `'1' = '1'` 恒成立，所以无论账户信息存在与否，都会输出 `yes` 



#### 解决办法

1. 使用预编译的数据库操作对象

```java
// 接收学生信息
Scanner s = new Scanner(System.in);
String stuID = s.nextLine();

PreparedStatement ps = null;
String sql = "SELECT * FROM stuinfo WHERE ID = ?";

// 将sql传给DBMS进行预编译
ps = conn.prepareStatement(sql);

// 给占位符传值，第一个参数为占位符下标，第二个参数为值
// 所以，我们还可以传递int类型的值，使用 setInt() 方法
ps.setString(1, stuID);

rs = ps.executeQuery();
```

-  `PreparedStatement` 与 `Statement` 的联系与区别
  -  `PreparedStatement` 继承自 `Statement` 
  -  `PreparedStatement` 的执行效率高于 `Statement` 
    -  `PreparedStatement` 的SQL语句是固定写死的，它只需要编译一次，尽管后面有给占位符传值的步骤，但本质上SQL语句不会变化，也就不需要再编译
    -  `Statement` 的SQL语句每次都会变化，所以每次都需要进行编译，执行效率更低
  -  `PreparedStatement` 会在编译阶段做类型的安全检查
- SQL语句中的 `?` 叫做占位符，接收一个值



#### 总结

除非业务要求必须支持SQL注入，否则安全起见都使用 `PreparedStatement` 





### JDBC事务机制

> 只要执行任意一条DML语句，就会自动提交一次

实际编程中，JDBC的默认机制往往是我们不希望看到的

#### eg.银行转账

创建账户余额表

```sql
CREATE TABLE a_b (
    `Account` CHAR(1) NOT NULL UNIQUE PRIMARY KEY,
    `Balance` DOUBLE(7,2) NOT NULL
)DEFAULT CHARSET=utf8;
```

A账户有200元，B账户有0元

```sql
INSERT INTO a_b VALUES (001, 200), (002, 0);
```

正常情况下，以下代码就可以实现A账户转100元给B账户

```java
PreparedStatement ps = null;
String sql = "UPDATE a_b SET Balance = ? where Account = ?";

// 修改A账户的余额，我们叫它操作1好了
ps.setDouble(1, 100);
ps.setString(2, "A");
int count = ps.executeUpdate();

// 修改B账户的余额，我们叫它操作2好了
ps.setDouble(1, 100);
ps.setString(2, "B");
count += ps.executeUpdate();

System.ou.println(count);	// 如果程序执行正常，输出的count应该等于2
```



但如果第4行到第10行间出现错误，就会导致修改余额异常

```sql
## 在Konsole中把A、B账户的余额修改为初始值
UPDATE a_b SET Balance = 200 WHERE Account = "A";
UPDATE a_b SET Balance = 0 WHERE Account = "B";
```

在第8行加入以下代码（制造异常，来演示JDBC自动提交机制

```java
// 此处会产生空指针错误，程序跳出当前 try{...} 
String s = null;
s.toString();
```

再次执行程序，我们就会发现，A账户的钱少了，B账户的钱却没有增加。

**上面的这些代码中，操作1和操作2是两个事务，根据事务隔离性可知，操作1和操作2各自独立，它们的执行结果不会影响到彼此**



#### 解决办法

> 我们可以让操作1和操作2互相影响，即把操作1和操作2放到同一个事务中，利用事务的一致性，就可以让操作1和操作2要么同时成功，要么同时失败。

关闭JDBC的自动提交（第7行），改为手动提交（第24行），如果程序能执行到24行，那么就说明前面没有出现错误，提交事务，转账成功。并在最后判断是否提交成功，没成功就进行回滚（回滚应该理解为把事务里的待提交的SQL语句删掉，然后关闭事务）。

```java
try {
    Class.forName("org.mariadb.jdbc.Driver");
    conn = DriverManager.getConnection(url, user, passwd);
    String sql = "UPDATE a_b SET Balance = ? where Account = ?";
    ps = conn.prepareStatement(sql);

    // 关闭自动提交，开启事务
    conn.setAutoCommit(false);

    // 修改A账户的余额
    ps.setDouble(1, 100);
    ps.setInt(2, 001);
    int count = ps.executeUpdate();

    // 此处会产生空指针错误，程序跳出当前 try{...} ，即程序将从第16行直接跳到第25行
    String s = null;
    s.toString();
    
    // 修改B账户的余额
    ps.setDouble(1, 100);
    ps.setInt(2, 002);
    count += ps.executeUpdate();
    System.out.println(count == 2 ? "转账成功" : "转账失败");

    // 手动提交，关闭事务
    conn.commit();
} catch (Exception e) {
    // 判断提交是否成功，失败就回滚
    if (conn != null) {
        try {
            conn.rollback();	// 清空操作1和操作2，关闭事务
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    e.printStackTrace();
}
```

> 我想起以前看的一部喜剧，在外漂泊的主人公阿蔡读了同乡从老家带来书信后伤心极了，放声大哭。同乡问他怎么哭的那么伤心，阿蔡说信里写他的妈妈病得很严重，请了好多郎中来看病，都说他的妈妈的病要很多钱才能医好，否则命不久矣，可自己却拿不出钱来，想着自己不能尽孝道，所以才哭得那么伤心。同乡拿过信封说，这里面不是还有一张嘛，我出门的时候你妈妈还好好的，怎么可能命不久矣嘞。阿蔡拿过信封里的第二张信看后果然破涕为笑：好在后来遇到神医，阿蔡的妈妈才得治病好了。
>
> 如果字写小点，都写在一张信纸上，就不会闹出这样的笑话了。这和我们上面讲的情况还挺像的。

在JDBC中我们需要先创建连接，等程序执行完后，再将连接关闭。以一个网站为例，多个用户同时请求的情况很常见，如果每个用户再查询数据库信息的时候，后端都是用传统JDBC方式查询的话，由于创建的连接过多的缘故，就会导致数据库内存溢出的问题。我们可以使用数据库连接池技术来解决这些问题。

- 这就类似共享充电宝的运行方式，有需要的人可以借，等用完后，再还回去，而不是每个人都带一个充电宝。共享的概念可以大大减少资源的浪费

### 数据库连接池

#### 什么是数据库连接池？

#### 作用

数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用现有的数据库连接，而不是每次都先新建再关闭。

- 当应用程序向连接池请求的连接数量超过最大连接数量（连接池中的连接数）时，这些请求将会进入等待队列

#### 常见的数据库连接池

##### c3p0

第一种：程序内手动设置连接信息

```java
// 从 .properties 文件中获取数据库连接信息
ResourceBundle bundle = ResourceBundle.getBundle("sql.properties");
String driver = bundle.getString("driver");
String url = bundle.getString("url");
String user = bundle.getString("user");
String password = bundle.getString("passwd");

// 获取连接池对象
ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource("");

// 配置连接池
comboPooledDataSource.setDriverClass(driver);	// 注册驱动
comboPooledDataSource.setJdbcUrl(url);	// 设置url
comboPooledDataSource.setUser(user);	// 设置用户名
comboPooledDataSource.setPassword(password);	// 设置密码
comboPooledDataSource.setMinPoolSize(10);	// 设置最小连接数
comboPooledDataSource.setMaxPoolSize(50);	// 设置最大连接数

Connection connection = comboPooledDataSource.getConnection();	// 取一个连接
connection.close();	// 将连接放回连接池
```

第二种：从配置文件获取连接信息

- 创建 `c3p0-config.xml` 配置文件

  - 完整设置项

    ```xml
    <c3p0-config>
    <default-config>
        <!-- 数据库驱动名 --> 
        <property name="driverClass" ></properties>
        
        <!-- 数据库的url --> 
        <property name="jdbcUrl"  ></properties>
        
        <!-- 当连接池中的连接耗尽的时候，c3p0每次新增的连接数。Default: 3 -->
        <property name="acquireIncrement">3</property>
            
        <!-- 定义在从数据库获取新连接失败后重复尝试的次数。Default: 30 -->
    	<property name="acquireRetryAttempts">30</property>
        
        <!-- 两次连接的间隔时间，单位毫秒。Default: 1000 -->
        <property name="acquireRetryDelay">1000</property>
    	
        <!-- 连接关闭时将所有未提交的操作回滚。Default: false -->
        <property name="autoCommitOnClose">false</property>
    	
        <!-- c3p0将建一张名为Test的空表，并使用其自带的查询语句进行测试。如果定义了这个参数那么属性preferredTestQuery将被忽略。你不能在这张Test表上进行任何操作，它将只供c3p0测试使用。Default: null -->
        <property name="automaticTestTable"></property>
        
     	<!-- 获取连接失败将会引起所有等待连接池来获取连接的线程抛出异常。但是数据源仍有效 保留，并在下次调用getConnection()的时候继续尝试获取连接。如果设为true，那么在尝试获取连接失败后该数据源将申明已断开并永久关闭。Default: false -->
        <property name="breakAfterAcquireFailure">false</property>
        
    	<!--当连接池用完时客户端调用getConnection()后等待获取新连接的时间，超时后将抛出SQLException,如设为0则无限期等待。单位毫秒。Default:  0 -->
        <property name="checkoutTimeout">0</property>
        
    	<!--通过实现ConnectionTester或QueryConnectionTester的类来测试连接。类名需制定全路径。Default:com.mchange.v2.c3p0.impl.DefaultConnectionTester -->
        <property name="connectionTesterClassName"></property>
        
    	<!--指定c3p0 libraries的路径，如果（通常都是这样）在本地即可获得那么无需设置，默认null即可 Default: null -->
        <property name="factoryClassLocation">null</property>
        
    	<!--Strongly disrecommended. Setting this to true may lead to subtle and bizarre bugs. （文档原文）作者强烈建议不使用的一个属性 -->
        <property name="forceIgnoreUnresolvedTransactions">false</property>
        
    	<!--每60秒检查所有连接池中的空闲连接。Default: 0 -->
        <property name="idleConnectionTestPeriod">0</property>
        
    	<!--初始化时获取三个连接，取值应在minPoolSize与maxPoolSize之间。Default: 3 -->
        <property name="initialPoolSize">3</property>
        
    	<!--最大空闲时间,指定的时间内未使用则连接被丢弃。若为0则永不丢弃。Default: 0 -->
        <property name="maxIdleTime">0</property>
        
    	<!--连接池中保留的最大连接数。Default: 15 -->
        <property name="maxPoolSize">15</property>
        
    	<!--JDBC的标准参数，用以控制数据源内加载的PreparedStatements数量。但由于预缓存的statements 属于单个connection而不是整个连接池。所以设置这个参数需要考虑到多方面的因素。 
        如果maxStatements与maxStatementsPerConnection均为0，则缓存被关闭。Default: 0 -->
        <property name="maxStatements">0</property>
        
    	<!--maxStatementsPerConnection定义了连接池内单个连接所拥有的最大缓存statements数。Default: 0 -->
        <property name="maxStatementsPerConnection">0</property>
        
    	<!--c3p0是异步操作的，缓慢的JDBC操作通过帮助进程完成。扩展这些操作可以有效的提升性能 通过多线程实现多个操作同时被执行。Default: 3 -->
        <property name="numHelperThreads">3</property>
        
    	<!-- 当用户调用getConnection()时使root用户成为去获取连接的用户。主要用于连接池连接非c3p0的数据源时。Default: null -->
        <property name="overrideDefaultUser"></property>
        
    	<!-- 与overrideDefaultUser参数对应使用的一个参数。Default: null -->
    	<!-- 密码。Default: null -->
        <property name="password"></property>
        
    	<!-- 定义所有连接测试都执行的测试语句。在使用连接测试的情况下这个一显著提高测试速度。注意：测试的表必须在初始数据源的时候就存在。Default:  null -->
        <property name="preferredTestQuery"></property>
        
    	<!-- 用户修改系统配置参数执行前最多等待300秒。Default: 300 -->
        <property name="propertyCycle">300</property>
        
    	<!-- 因性能消耗大请只在需要的时候使用它。如果设为true那么在每个connection提交的 时候都将校验其有效性。建议使用idleConnectionTestPeriod或automaticTestTable等方法来提升连接测试的性能。Default: false -->
        <property name="testConnectionOnCheckout">false</property>
        
        <!-- 如果设为true那么在取得连接的同时将校验连接的有效性。Default: false -->
        <property name="testConnectionOnCheckin">false</property>
        
        <!-- 用户名。Default: null -->
        <property name="user"></property>
        
        <!-- 连接超时时间, default: 0。如果是0，表示无限等待 --> 
        <property name="checkoutTimeout">0</property>
        
        <!-- 测试空闲连接的间隔时间 default: 0 --> 
        <property name="idleConnectionTestPeriod ">0</property>
        
        <!-- 初始化时连接数，default: 3 -->    
        <property name="initialPoolSize">3</property>
        
        <!-- 连接的最大空闲时间，default: 0 -->    
        <property name="maxIdleTime">0</property>
        
        <!-- 连接池中最大连接数，default: 15 -->  
        <property name="maxPoolSize">15</property>
        
        <!-- 连接池中最小连接数，default: 3 -->    
        <property name="minPoolSize">3</property>
        
        <!-- 连接池为数据源缓存的PreparedStatement的总数 -->    
        <property name="maxStatements">0</property>  
    </default-config>
    </c3p0-config>
    ```

  - 常用设置

    ```xml
    <?xml version="1.0" encoding="UTF-8"?> 
    <c3p0-config>
      <default-config>   
        <property name="driverClass">com.mysql.cj.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/st</property>
        <property name="user">root</property>
        <property name="password">java</property>
     
        <property name="initialPoolSize">10</property>
        <property name="maxIdleTime">30</property>
        <property name="maxPoolSize">100</property>
        <property name="minPoolSize">10</property>
      </default-config>
     
      <named-config name="mySource">
        <property name="driverClass">com.mysql.cj.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/st</property>
        <property name="user">root</property>
        <property name="password">534760</property>
     
        <property name="initialPoolSize">10</property>
        <property name="maxIdleTime">30</property>
        <property name="maxPoolSize">100</property>
        <property name="minPoolSize">10</property>
      </named-config>
    </c3p0-config>
    ```

  - 连接代码

    ```java
    ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource("mySource");	// 获取连接池
    Connection connection = comboPooledDataSource.getConnection();	// 取一个连接
    System.out.println("Connection success");	// 输出信息提示连接成功
    connection.close();	// 将连接放回连接池
    ```

##### Druid

- 配置文件

  ```properties
  driverClassName=com.mysql.cj.jdbc.Driver
  url=jdbc:mysql://localhost:3306/st?serverTimezone=UTC
  characterEncoding=utf-8
  username=root
  password=534760
  initialSize=5
  maxActive=50
  maxWait=3000
  validationQuery=SELECT 1
  testWhileIdle=true
  ```

- 代码

  ```java
  Properties properties = new Properties();
  properties.load(new FileInputStream("druid_config.properties"));
  DataSource dataSource = null;
  dataSource = DruidDataSourceFactory.createDataSource(properties);
  ```



### Apache-DBUtils

我们编写的JDBC目前存在的几个问题

- 针对 `ResultSet` 的处理，必须要在 `connection` 关闭之前进行。既然我们已经学习到了连接池，那么这显然是个致命问题。
- 类似于 `ps = conn.prepareStatement(sql)` 、 `rs = ps.executeQuery()` 这样的语句，在每一个JDBC程序中几乎都会出现。显所以最好是能够有一个简单的方法，能帮助我们省略掉这些重复的代码书写

现在，我们可以使用Apache-DBUtils这个工具来帮助我们

#### DQL语句

> 新建对象类，采用一般方法将 `ResultSet` 的内容封装到一个对象类对象中

```java
import java.util.*;

public class UserInfo {
    private String id = null;
    private String point = null;
    private String name = null;
    private Date birth = null;
    private String gender = null;
    
    // 提供set方法
    public void setId(String id) {
        this.id = id;
    }
    
    public void setPoint(String point) {
        this.point = point;
    }
    
    public void set (String name) {
        this.name = name;
    }
    
    public void setBirth(Date birth) {
        this.birth = birth;
    }
    
    public void setGender() {
        this.gender = gender;
    }
    
    // 提供get方法
    public String getId() {
        return id;
    }
    
    public String getPoint() {
        return point;
    }
    
    public String getName() {
        return name;
    }
    
    public Date getBirth() {
        return birth;
    }
    
    public String getGender() {
        return gender;
    }
}
```

- 返回结果集合（不是 `ResultSet` 对象）

  ```java
  QueryRunner queryRunner = new QueryRunner();
  String sql = "SELECT * FROM stu";
  
  // 执行Apache-DBUtils的查询方法
  List<UserInfo> list = queryRunner.query(conn, sql, new BeanListHandler<>(UserInfo.class), values);
  ```

-  返回单个对象

  - 返回一条完整记录

    ```java
    QueryRunner queryRunner = new QueryRunner();
    String sql = "SELECT * FROM stu";
    
    // 执行Apache-DBUtils的查询方法
    UserInfo userInfo = queryRunner.query(conn, sql, new BeanHandler<>(UserInfo.class), values);
    ```

  - 返回指定字段

    ```java
    QueryRunner queryRunner = new QueryRunner();
    String sql = "SELECT * FROM stu WHERE ID = ?";
    
    // 执行Apache-DBUtils的查询方法
    List<UserInfo> list = queryRunner.query(conn, sql, new ScalarHandler(), values);
    ```

-   `QueryRunner queryRunner = new QueryRunner();` 

   -  实例化Apache-DBUtils查询对象

-   `query()` 

   -  查询记录若不存在，则返回null
   -   `new BeanListHandler<>(UserInfo.class)` ：将 `ResultSet` 对象的数据放到 `UserInfo` 对象中，在封装成 `ArrayList` 集合（用到反射机制）
   -   `new BeanHandler<>(UserInfo.class)` ：返回一个 `UserInfo` 对象
   -  values：可变参数，用来给SQL语句中的?赋值，有多少个问号，就填多少个值
   -  Apach-DBUtils中也会用到 `ResultSet` 和 `PreparedStatement` ，它们的创建和关闭都在Apach-DBUtils内部完成了，不会造成资源泄露

#### DML语句

```java
QueryRunner queryRunner = new QueryRunner();
String sql = "UPDATE stu SET Point = ? WHERE ID = ?";

int count = List<UserInfo> list = queryRunner.query(conn, sql, values);
```

- 返回值是受影响的行数
