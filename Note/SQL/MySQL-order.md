# 注意

- 右上角带 `*` 的条目为未确定条目
- 为了编辑方便，本文中的 `database_name` 表示 “数据库名”， `table_name` 表示 “表名” 



# 常用命令

## 启停MySQL服务

### 启动

Windows： `net start mysql80` 

### 关闭

Windows： `net stop mysql80` 



## 登录root账户

### 本地

 `mysql -u用户名 -p` 

 `mysql -u用户名 -p密码` 

> 第二种方法后面直接跟密码，不需要加引号

### 云端

 `mysql -u 用户名 -p` 

 `mysql -hIP地址 -P端口号 -u用户名 -p密码` 

> 同上，第二种方法后面直接跟密码，不需要加引号



## 查看

> 此部分的语句后可以选择添加 `\G` 或者 `\g` 
>
> 加了 `\G` 后，输出将会按列打印，将每一个字段打印到单独的一行
>
> 加上 `\g` 后，与什么都不加没有任何区别，其作用等同于 `;`  。也就是说加了 `\g` 就不用加 `;` 了

### 查询数据库

#### 查看所有数据库

 `SHOW DATABASES;` 

- 以此为例讲解 `\g` ： `SHOW DATABASES\g` 



#### 查看当前数据库

 `SELECT DATABASE();` 

> 输出当前使用的数据库的名称



### 查询表

#### 查看所有表（当前数据库中）

 `SHOW TABLES;` 



#### 查询建表语句

 `SHOW CREATE TABLE table_name;` 



#### 查看表的详细信息

> 包含字段名、属性

 `SHOW FULL COLUMNS FROM table_name;` 



### 单表查询

> DQL语句的书写顺序：
>
>  `SELECT 字段1, 字段2, .... FROM table_name WHERE 条件 GROUP BY 分组字段名1 HAVING 分组后的过滤条件 ORDER BY 排列顺序条件 LIMIT 起始索引, 查询记录数;` 
>
> DQL语句的执行顺序：
>
>  `FROM` -> `WHERE` -> `GROUP BY .... [HAVING ....]` -> `SELECT` -> `ORDER BY` -> `LIMIT` 

#### 查询所有字段

 `SELECT * FROM table_name;` 

- 开发中用的比较少
  - 不够直观，不能直接看出在查询哪些字段



#### 查询指定字段

 `SELECT 字段名1, 字段名2, .... FROM table_name;` 



#### 以别名的形式输出指定字段

 `SELECT 字段名1 AS '别名1', 字段名2 AS '别名2', .... FROM table_name;` 

 `SELECT 字段名1 '别名1', 字段名2 '别名2', .... FROM table_name;` 

 `SELECT '别名1' = 字段名1, '别名2' = 字段名2, .... FROM table_name;` 

- 别名的使用，很多时候是为了方便其他人在看数据库时能够更容易理解每个字段的含义
- 但你在语句中为某个字段起了别名，在该条语句中的其他位置，可以用这个别名指代这个字段

- 三条语句的输出结果一致，但更推荐第一种写法



#### 按条件查询

> 可以用 `()` 将某些条件括起来

 `SELECT 字段名1, 字段名2, .... FROM table_name WHERE 条件;` 

| 比较运算符                | 作用                                               | 逻辑运算符 | 作用 |
| ------------------------- | -------------------------------------------------- | ---------- | ---- |
| >                         | 大于                                               | AND 或 &&  | 与   |
| >=                        | 大于等于                                           | OR 或 \|\| | 或   |
| <                         | 小于                                               | NOT 或 !   | 非   |
| <=                        | 小于等于                                           |            |      |
| =                         | 等于                                               |            |      |
| <> 或 !=                  | 不等于                                             |            |      |
| BETWEEN 最小值 AND 最大值 | 在指定范围内，包含最小值和最大值                   |            |      |
| IN(....)                  | 在IN之后的列表中的值，多选一                       |            |      |
| LIKE 占位符               | 模糊匹配（ `_` 匹配单个字符， `%` 匹配任意个字符） |            |      |
| IS NULL                   | 为NULL                                             |            |      |

#####  `WHERE` 

`SELECT StuID FROM StuInfo WHERE Apoint > 85;` 

 `SELECT StuID FROM StuInfo WHERE NOT Apoint <= 85;` 

- 按是否为空进行查询时，条件写为 `IS NULL` 或 `IS NOT NULL` 

##### 多重条件查询（运算符）

- 查询全班平均分为60、80的同学的名字

    `SELECT StuName FROM StuInfo WHERE Apoint = 60 OR Apoint = 80;` 

    `SELECT StuName FROM StuInfo WHERE Apoint IN (60, 80);` 

- 查询全班平均分为80的男同学的名字

    `SELECT StuName FROM StuInfo WHERE Apoint = 60 AND Gender = '男';` 

##### 模糊查询 `LIKE` 

- 查询学生表中所有姓 ’李‘ 的同学

   `SELECT StuID FROM StuInfo WHERE StuName LIKE '李%';` 

  - 名字可能是两个字、三个字，甚至更多
    - 如果要查询姓 ‘李’ 名字为2个字的同学，写成 `'李_'` 
    - 如果要查询姓 ‘李’ 名字为3个字的同学，写成 `'李__'` 

  - 如果要查询第二个字为 ‘嘉’ 的同学，就写成 `'_嘉%'` ，其他位置以此类推

##### 分组查询 `GROUP BY .... [HAVING ....]` 

>  `SELECT 字段列表 FROM table_name [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件];` 
>

###### 执行顺序

在 `table_name` 表中，根据分组字段名查询字段列表

1.  **没有**  `HAVING` 关键字，则返回查到的第一个分组字段名的字段列表
2.  **有**  `HAVING` 关键字，则根据 `HAVING` 关键字后的条件筛选符合的字段列表，并将其返回

- 查询全班男同学和女同学各有多少人

   `SELECT Gender, COUNT(*) FROM StuInfo GROUP BY Gender;` 

- 根据城市分组，查询选了《乐理视唱与音乐欣赏》课的所有同学，并且按家乡输出其中平均分高于80的同学

   `SELECT City, COUNT(*) FROM StuInfo WHERE EC = '乐理视唱与音乐欣赏' GROUP BY City HAVING Apoint > 80;` 

  -  `WHERE` 和 `HAVING` 的区别

    - 执行时机
      -  `WHERE` 在分组前过滤，不满足 `WHERE` 的不参与分组
      -  `HAVING` 在分组后对结果进行过滤

    - 判断条件
      -  `WHERE` 不对聚合函数进行判断
      -  `HAVING` 可以对聚合函数进行判断
    - 通常， `HAVING` 只对聚合函数和分组字段进行判断，判断其他字段是没有意义的

  -  `GROUP BY` 后面跟有多个字段名时，按照排列组合来理解：把这些字段的所有值按所有可能的组合输出（**穷举法**）

    - 字段1为A，且字段2为AA时，聚合函数的结果是什么
    - 字段1为B，且字段2为BB时，聚合函数的结果是什么
    - ....

##### 去重查询 `DISTINCT` 

- 查询全班同学选了哪些选修课

   `SELECT DISTINCT EC FROM StuInfo;` 

  -   `DISTINCT` 只能在 `SELECT` 语句中使用

##### 降（升）序查询 `ORDER BY` 

> 默认时ASC（升序）

- 查询全班选《乐理视唱与音乐欣赏》选修课的同学有哪些，并将他们的学号按平均成绩降（升）序排列

   `SELECT StuID FROM StuInfo WHERE EC = '乐理视唱与音乐欣赏' ORDER BY Apoint DESC;` 

   `SELECT StuID FROM StuInfo WHERE EC = '乐理视唱与音乐欣赏' ORDER BY Apoint ASC;` 

  -  `DESC` 降序； `ASC` 升序，可以省略

- 对学生的学号按平均成绩降序排序，当平均成绩相同时，按学号降序排序

   `SELECT StuID FROM StuInfo ORDER BY Apoint, StuID DESC;` 

##### 分页查询 `LIMIT` 

> 分页查询在不同的数据库（DBMS）中有不同的实现，MySQL中的关键字是 `LIMIT` 

 `SELECT 字段名1, 字段名2, .... FROM table_name LIMIT 起始索引, 查询记录数量` 

- 起始索引从0开始。起始索引 = (页码-1) * 每页显示记录数
  - 例如，以n条记录为一页，查询第m页的数据，则起始索引 = (m-1) * n
  - 如果查询的是第1页的数据，可以不写起始索引，直接简写为 `LIMIT 要查询的记录数量` 



### 多表查询

在多表查询中，必须要将两张表中相同且唯一的字段作为连接条件，这等同于古代的虎符，当给出了连接条件后，可视两张表为一张表





### 查询用户

见 [用户权限控制](#quanxiankongzhi) 





## 使用

### 使用数据库

 `USE database_name;` 



### 使用表

 `USE table_name;` 



## 增添

### 创建数据库*

 `CREATE DATABASE database_name;` 

 `CREATE DATABASE [IF NOT EXISTS] database_name [DEFAULT CHARSET 字符集] [COLLATE 排序规则];` 

```sql
CREATE DATABASE IF NOT EXISTS st DEFAULT CHARSET utf8mb4 COLLATE ;
# 设置默认字符集时，不建议设置utf8，某些特殊字符占4个字节，占3个字节的utf8无法保存
```



### 创建表（以学生信息表为例）

```sql
CREATE StuInfo (
    `StuID` CAHR(11) NOT NULL PRIMARY KEY UNIQUE COMMENT'学号',
    `StuName` VARCHAR(6) NOT NULL COMMENT'姓名',
    `Gender` CHAR(1) NOT NULL COMMENT'性别',
    `Age` INT NOT NULL COMMENT'年龄',
    `City` VARCHAR(6) NOT NULL COMMENT'家乡',
    `Tpoint` FLOAT(1) NOT NULL COMMENT'总成绩',
    `Apoint` FLOAT(1) NOT NULL COMMENT'平均成绩',
    `EC` VARCHAR(12) NOT NULL COMMENT'选修课'
)DEFAULT CHARSET=utf8 COMMENT'学生信息';
```

- 定义完一个字段后，如果还要定义其他字段，用 `,` 隔开
  - 定义最后一个字段时，不需要再加 `,` 



### 添加外键

> 添加外键前，被参照表（父表）必须已存在

- 建表时添加

  方法一： `字段名 属性 REFERENCES 被参照表名(外键字段名)` 

  方法二： `FOREIGN KEY(表内字段名) REFERENCES 被参照表名(外键字段名);` 

- 建表后添加

   `ALTER TABLE table_name ADD [CONSTRAINT 别名] FOREIGN KEY(表内字段名) REFERENCES 被参照表名(外键字段名);` 



### 增加字段

 `ALTER TABLE table_name ADD 字段名 数据类型 [COMMENT 注释] [约束]; ` 



### 增加约束

 `ALTER TABLE table_name ADD [CONSTRAINT 别名] 约束(字段名);` 

- 如果有别名，则以后可以用别名删除该约束
- 不能添加NOT NULL约束，只能删除该字段后重写



### 增加用户

见 [用户权限控制](#quanxiankongzhi) 





## 赋值

### 为所有字段赋值

 `INSERT INTO table_name VALUES (值1, 值2, 值3), (值4, 值5, 值6);` 



### 为指定字段赋值

 `INSERT INTO table_name(字段名1, 字段名2, 字段名3) VALUES (值1, 值2, 值3), (值4, 值5, 值6);` 



## 删除

### 删除数据库

 `DROP DATABASE database_name;` 

 `DROP DATABASE IF EXISTS database_name;` 



### 删除表 <span id="shanchut"> </span> 

 `DROP TABLE [IF EXISTS] table_name;` 

- 必须先删除参照表（子表），再删除被参照表（父表）
-  **跑路必备技能** 



### 删除数据

#### 删除所有数据 <span id="shanchusuoyoushuju"> </span> 

 `DELETE FROM table_name;` 

- 表还在，和 [删除表](#shanchut) 的第一种方法不一样

 `TRUNCATE TABLE table_name;` 

- 旧表不在了，创建了一张新表。此方法会先删除表，在重新创建一张结构、属性、字段等与当前删除的表相同的空表
  - 从结果来看，这条语句的作用跟 `DELETE FROM table_name;` 相同，它仅仅删除了表中的数据

#### 按条件删除

 `DELETE FROM table_name WHERE 条件;` 



### 删除字段

 `ALTER TABLE table_name DROP [COLUMN] 字段名;` 

-  `COLUMN` 关键字可以不加，加了也没错



### 删除约束

 `ALTER TABLE table_name DROP CONSTRAINT 别名;	##要删除的字段约束有别名时` 



### 删除外键

 `ALTER TABLE table_name DROP FOREIGN KEY 外键字段名;` 



### 删除用户

见 [用户权限控制](#quanxiankongzhi) 



## 修改

### 修改表的名字

 `RENAME TABLE 旧表名 TO 新表名` 



### 修改字段的属性

#### 修改列的数据类型

 `ALTER TABLE table_name MODIFY [COLUMN] 字段名 新数据类型 [新约束];` 

eg. `ALTER TABLE table_name MODIFY COLUMN 字段名 数据类型 新约束` 

-  `COLUMN` 关键字可以不加，加了也没错

#### 同时修改字段的名字和数据类型

 `ALTER TABLE table_name CHANGE [COLUMN] 旧字段名 新字段名 新数据类型 [COMMENT 注释] [新约束];` 

-  `COLUMN` 关键字可以不加，加了也没错

#### 修改字段的注释*

 `ALTER TABLE table_name MODIFY 字段名 数据类型 COMMENT'XXX';` 



### 修改记录的值

 `UPDATE table_name SET 字段名1 = "值", 字段名2 = "值", .... [WHERE 条件];` 

- 不加 `WHERE` 关键字则表示修改表内所有字段的值
  - 例如， `UPDATE StuInfo SET Age = 18;` 会将表内所有记录的 `Age` 修改为18



### 修改用户密码

见 [用户权限控制](#quanxiankongzhi) 



## 用户权限控制 <span id="quanxiankongzhi"> </span> 

| 权限关键字          | 权限                 |
| ------------------- | -------------------- |
| ALL, ALL PRIVILEGES | 所有权限             |
| SELECT              | 查询数据             |
| INSERT              | 插入数据             |
| UPDATE              | 修改数据             |
| DELETE              | 删除数据             |
| ALTER               | 修改表               |
| DROP                | 删除数据库、表、视图 |
| CREATE              | 创建数据库、表       |

- 权限间用 `,` 隔开

### 查询用户

```sql
# 所有用户的信息都存在mysql的user表中
USE mysql;
SELECT * FROM user;
```



### 查询用户权限

 `SHOW GRANTS FOR '用户名'@'主机名';` 



### 授予权限

 `GRANT 权限关键字 ON 数据库名.表名 TO '用户名'@'主机名';` 

- 数据库名和表名都可以用 `*` 表示，即 `*.*` ，表示授予所有数据库中所有表的权限



### 撤销权限

 `REVOKE 权限关键字 ON 数据库名.表名 FROM '用户名'@'主机名';` 

- 数据库名和表名都可以用 `*` 表示，即 `*.*` ，表示撤销所有数据库中所有表的权限



### 增加用户

 `CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';` 



### 修改用户密码

 `ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';` 



### 删除用户

 `DROP USER '用户名'@'主机名';` 



## 导入 `.sql` 文件

>  `.sql` 文件内就是一些SQL语句

 `SOURCE 绝对路径` 





## 约束







## 函数

### 常用内置函数

#### 字符串函数

> 索引从1开始

| 函数                       | 功能                                            |
| -------------------------- | ----------------------------------------------- |
| CONCAT(str1, str2, ....)   | 字符串拼接，将str1, str2, ....拼接成一个字符串  |
| LOWER(s)                   | 将字符串s全部转成小写                           |
| UPPER(s)                   | 将字符串s全部转成大写                           |
| LPAD(str, n, pad)          | 左填充，将pad拼接到str的左边，结果的长度为n     |
| RPAD(str, n, pad)          | 右填充，将pad拼接到str的右边，结果的长度为n     |
| TRIM(str)                  | 去掉字符串头部和尾部的空格                      |
| SUBSTRING(str, start, len) | 返回从字符串str从start位置起，长度为len的字符串 |

##### LPAD

 `SELECT LPAD('.', 2, '*');` 

输出 `*.` 



##### RPAD

 `SELECT RPAD('.', 2, '*');` 

输出 `.*` 

- 组合上面两种 `SELECT RPAD(LPAD('.', 2, '*'), 3, '*');` ，输出 `*.*` 



#### 数值函数

| 函数        | 功能                             |
| ----------- | -------------------------------- |
| CEIL(x)     | 向上取整                         |
| FLOOR(x)    | 向下取整                         |
| MOD(x, y)   | 返回x/y的模                      |
| RAND()      | 返回 [0, 1) 的随机数             |
| ROUND(x, y) | 求参数x的四舍五入值，保留y位小数 |



#### 日期函数

| 函数                               | 功能                                                |
| ---------------------------------- | --------------------------------------------------- |
| CURDATE()                          | 返回当前日期， `YYYY-MM-DD`                         |
| CURTIME()                          | 返回当前时间， `HH: MM: SS`                         |
| NOW()                              | 返回当前日期和时间， `YYYY-MM-DD HH: MM: SS`        |
| YEAR(date)                         | 获取指定date的年份                                  |
| MONTH(date)                        | 返回指定date的月份                                  |
| DAY(date)                          | 返回指定date的日期                                  |
| DATE_ADD(date, INTERVAL expr type) | 返回一个日期 / 时间值加上一个时间间隔expr后的时间值 |
| DATEDIFF(date1, date2)             | 返回起始时间date1和结束时间date2之间的天数          |

##### YEAR(date)

把date中的年份分离并打印出来，MONTH(date)、DAY(date)同理



##### DATE_ADD(date, INTERVAL expr type)

在date上添加expr type

- 例如， `SELECT DATE_ADD('1997-07-01', INTERVAL 10 DAY);` ，表示在1997-07-01上添加（推后）10天，输出就是1997-07-11



##### DATEDIFF(date1, date2)

> 前-后

求date1和date2之间相隔的天数，可以交换位置，如果过去在前，未来在后，则输出的值为 **负** 

- 例如， `SELECT DATEDIFF('1997-07-10', '1997-07-01');` ，输出为10，交换后，输出为-10

#### 流程函数

| 函数                                                        | 功能                                                        |
| ----------------------------------------------------------- | ----------------------------------------------------------- |
| IF(value, t, f)                                             | 如果value为true，则返回t，否则返回f                         |
| IFNULL(value1, value2)                                      | 如果value1不为空，则返回value1，否则返回value2              |
| CASE WHEN [val1] THEN [res1] .... ELSE [default] END        | 如果val1为true，返回res1，....，否则返回default默认值       |
| CASE [expr] WHEN [val1] THEN [res1] .... ELSE [default] END | 如果expr等于val1的值，返回res1，....，否则返回default默认值 |



### 常用聚合函数

- 只能用于SELECT子句或GROUP BY的HAVING子句中
- 所有聚合函数均忽略空值



#### COUNT

计算表中有多少字段

 `SELECT COUNT(*) FROM StuInfo;` 



#### SUM

计算全班平均成绩之和

 `SELECT SUM(Apoint) FROM StuInfo;` 



#### AVG

求全班的平均成绩

 `SELECT AVG(Apoint) FROM StuInfo;` 



#### MIN

求全班最低平均成绩

 `SELECT MIN(Apoint) FROM StuInfo;` 



#### MAX

求全班最高平均成绩

 `SELECT MAX(Apoint) FROM StuInfo;` 
