

# 数据库（DB）

> DataBase



# 数据库管理系统（DBMS）

> DataBase Management System



# 语法

- 当SQL语句较长时，可以使用空格和缩进来增强可读性

  - 空格和缩进的个数可以是任意的

- 一条SQL语句可以写在同一行或多行，以分号 `;` 结尾

- MySQL中不区分大小写，但关键字仍然建议使用大写

- 使用 `--` 或 `#` 书写单行注释；使用 `/**/` 书写多行注释

  ```sql
  -- 一种单行注释
  # 另一种单行注释
  /* 
  这
  是
  多
  行
  注
  释
  */
  ```




# 概念

## 字段

又称记录、列，某个事物的一个特征，或者说是属性



## 记录

又称元组、数据，事物特征的组合，可以描述一个具体的事物



## 表

记录的组合，表示同一类事物的组合



## 约束

> 六大约束都可用作列级约束，而表级约束不可用 `NOT NULL` 、 `NULL` 、 `DEFAULT` 

### 主键约束（ `PRIMARY KEY` ）

> 能唯一标识事物的特征（如身份证号码）。

- 一张表里只能有一个主键，它是这张表的唯一索引
  - 当有两个字段被设置为主键时，这两个字段合称为 [复合主键](#fuhezhujian) ，由它们两个来组成这张表的唯一索引
- 设置为主键的字段，就是非空（ `NOT NULL` ）唯一（ `UNIQUE` ）的，无需重复设置

- 复合主键: <span id="fuhezhujian"> </span> 即一张表里存在多个主键，它们共同组成这张表的唯一索引。这种情况下我们应该在每个主键后添加主键约束，又或者添加表级主键约束

  - 不推荐使用

  ```sql
  ## 方法一：在每个主键后添加主键约束
  zhujian1 INT PRIMARY KEY,
  zhujian2 INT PRIMARY KEY
  
  ## 方法二：添加表级主键约束
  PRIMARY KEY(zhujian1, zhujian2)
  ```

- 联合主键：

### 外键约束（ `FOREIGN KEY` ）

> 外部参照。

- 外键不能是它所在表的主键

- 当前表的字段应当和外键的类型、长度完全一致

  ```sql
  ## 外键
  a CHAR(3)
  
  ## 当前表中字段
  ## 正确写法
  b CHAR(3) REFERENCES student(a)
  
  ## 错误写法
  b CHAR(2) REFERENCES student(a)		## 类型相同，长度不同
  b VARCHAR(3) REFERENCES student(a)	## 长度相同，类型不同
  ```

### 唯一约束（ `UNIQUE` ）



### 默认值约束（ `DEFAULT` ）



### 检查约束（ `CHECK` ）



### 非空约束（ `NOT NULL` ）



# 结构化查询语言（SQL，Structured Query Language）

> 结构化查询语言用于访问和处理数据库的标准的计算机语言
>

## 数据定义语言（DDL）

> Data Definition Language，定义数据结构和数据库对象

###  `CREATE` 



###  `ALTER` 



###  `DROP` 



## 数据操纵语言（DML）

> Data Manipulation Language，对数据库中的数据进行修改

###  `INSERT` 

> 插入



###  `UPDATE` 

> 更新



###  `DELETE` 

> 删除



## 数据查询语言（DQL）

> Data Query Language，进行数据查询，不会对数据进行修改

###  `SELECT` 

> 用于查询数据

#### 书写顺序

 `SELECT 字段1, 字段2, .... FROM table_name WHERE 条件 GROUP BY 分组字段名1 HAVING 分组后的过滤条件 ORDER BY 排列顺序条件 LIMIT 起始索引, 每页的记录数;` 

#### 执行顺序

 `FROM` -> `WHERE` -> `GROUP BY .... [HAVING ....]` -> `SELECT` -> `ORDER BY` -> `LIMIT` 

语言表达就是：查哪张表？展示的字段应该满足什么条件？按什么分组？[分组后的数据需要再筛选吗？]展示哪些字段？怎么排序？怎么分页？




## 事务处理语言（TPL）

> 确保被DML语句影响的表的所有行及时得以更新

###  `COMMIT` 

###  `ROLLBACK` 

###  `BEGIN TRANSACTION` 



## 数据控制语言（DCL）

> Data Control Language，创建用户和控制用户对数据访问权的指令，它可以控制特定用户对数据表、查看表、预存程序、用户自定义函数等数据库对象的控制

###  `GRANT` 



###  `REVOKE` 



## 指针控制语言（CCL）

> 对一个或多个表单独行的操作

###  `DECLARE CURSOR` 

###  `FETCH INTO` 

###  `UPDATE WHERE CURRENT` 



# 关系型数据库（RDBMS)

> 建立在关系模型的基础上，由多张相互连接的二维表组成的数据库

## 特点

- 使用表存储数据，格式统一，便于维护
  - 即不使用表结构存储数据的数据库就是 **非关系型数据库** 
- 使用SQL语言操作，标准统一，使用方便



## 属性

### 自增（ `IDENTITY (seed, increment)` ）

> seed 表示从几开始，increment表示每次增长多少

即便某条数据被删除，这条数据的编号也不会被用在其他数据上。



## 事务

### 特性（ACID）

#### 原子性（Atomicity）

> 指事务是一个不可分割的最小工作单位，事务中的操作只有都发生和都不发生两种情况

一个事务由许多DML语句组成，如果其中的某条DML语句执行失败了，那么就会触发回滚，而未执行的DML语句也就不会执行。因此这些DML语句的执行结果只可能是同时成功，或者同时失败



#### 一致性（Consistency）

> 事务必须使数据库从一个一致状态变换到另外一个一致状态

假如，李二给王五转账50元，其事务就是让李二账户上减去50元，王五账户上加上50元；一致性是指其他事务看到的情况是要么李二还没有给王五转账的状态，要么王五已经成功接收到李二的50元转账。而对于李二少了50元，王五还没加上50元这个中间状态是不可见的



#### 隔离性（Isolation）

> 一个事务的执行不能被其他事务干扰，即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰



#### 持久性（Durability）

> 一个事务一旦提交成功，它对数据库中数据的改变将是永久性的，接下来的其他操作或故障不应对其有任何影响

过去的事情已经发生了，那么未来无论发生什么都不可能改变已经发生的事情



### 分类

#### 隐式事务

> 事务没有明显的开启和结束标记，它们都具有自动提交事务的功能

DML语句就是隐式事务



#### 显式事务

> 事务具有明显的开启和结束标记

##### 查询当前显隐状态

 `SELECT @@autocommit;` 

- 默认为开启状态（1：开启	0：关闭）

##### 修改显隐状态

 `SET autocommit = 0	##修改为隐式状态` 



## 回滚



## 锁

### 悲观锁

> 又称行级锁，事务必须排队进行，当前事务结束前，元组处于被锁住的状态，不可被其他事务修改，即不可并发

在 `SELECT` 语句后写 `FOR UPDATE` 即可



### 乐观锁

> 支持并发，事务不需要排队，但需要提供版本号

事务1和事务2都读取到版本号1.1，事务1先修改数据，并把版本号改为1.2，之后事务2来修改数据，发现版本号与上次读取的不同，于是进行回滚，即放弃本次修改



## 数据类型

> 在插入记录时，除了 `INT` （整数类型）和 `SMALLINT` （小数类型）外，都要添加 `''` 
>
> 例如，为 `DATE` 类型赋值应写作 `'1997-07-01'` 

| 整数              | 大小 / byte | 范围（有符号 / 无符号）                     | 含义                                       |
| ----------------- | ----------- | ------------------------------------------- | :----------------------------------------- |
| TINYINT           | 1           | (-128, 127) / (0, 255)                      |                                            |
| SMALLINT          | 2           | (-32768, 32767) / (0, 65535)                |                                            |
| MEDIUMINT         | 3           | (-8388608, 8388607) / (0, 16777215)         |                                            |
| INT 或 INTEGER    | 4           | (-2147483648, 2147483647) / (0, 4294967295) | 长整数                                     |
| BIGINT            | 8           | (-2^63, 2^63-1) / (0, 2^64-1)               |                                            |
| NUMERIC(m, n)     |             |                                             | 定点数，总共含有m个数字，小数点后有n个数字 |
| REAL              |             |                                             | 取决于机器精度的浮点数                     |
| DOUBLE  PRECISION |             |                                             | 取决于机器精度的双精度浮点数               |
|                   |             |                                             |                                            |
|                   |             |                                             |                                            |
|                   |             |                                             |                                            |
|                   |             |                                             |                                            |

- 当数值不包含符号时，可在类型后添加 `UNSIGN` 关键字，例如 `INT(10) UNSIGNED` 



| 小数     | 大小 / byte | 范围（有符号 / 无符号）                                      | 含义                                    |
| -------- | ----------- | ------------------------------------------------------------ | --------------------------------------- |
| FLOAT(n) | 4           | (-3.402823466 E+38, -3.402823466351 E+38) / 0, (1.175494351 E-38, 3.402823466 E+38) | 单精度浮点数，小数点后有n个数字（n<24） |
| DOUBLE   | 8           | (-1.7976931348623157 E+308, 1.7976931348623157 E+308) / 0, (2.2250738585072014 E-308, 1.7976931348623157 E+308) | 双精度浮点数，                          |
|          |             |                                                              |                                         |

- 当数值不包含符号时，可在类型后添加 `UNSIGN` 关键字，例如 `FLOAT(10) UNSIGNED` 



| 字符（串）      | 大小 / byte    | 含义                         |
| --------------- | -------------- | ---------------------------- |
| CHAR(length)    | 0 - 255        | 定长字符串，长度为length     |
| VARCAHR(length) | 0 - 65535      | 变长字符串，最大长度为length |
| TINYBLOB        | 0 - 255        | 不超过255个字符的二进制数据  |
| TINYTEXT        | 0 - 255        | 短文本字符串                 |
| BLOB            | 0 - 65535      | 二进制形式的长文本数据       |
| TEXT            | 0 - 65536      | 长文本数据                   |
| MEDIUMBLOB      | 0 - 16777215   | 二进制形式的中等长度文本数据 |
| MEDIUMTEXT      | 0 - 16777215   | 中等长度文本数据             |
| LONGBLOB        | 0 - 4294967295 | 二进制形式的极大文本数据     |
| LONGTEXT        | 0 - 4294967295 | 极大文本数据                 |

- BLOB这种类型，可以将视频、音频等文件以二进制的形式存储在数据库中
  - 开发中很少这样用，因为效率太低。通常会有专门的服务器对文件进行存储，数据库中可以只保存文件对应的统一资源定位符（URL）



| 日期&时间 | 大小 / byte | 范围                                           | 格式                  | 含义                            |
| --------- | ----------- | ---------------------------------------------- | --------------------- | ------------------------------- |
| TIME      | 3           | -838: 59: 59 至 838: 59: 59                    | HH: MM: SS            | 时间，包含时、分、秒，H不大于24 |
| DATE      | 3           | 1000-01-01 至 9999-12-31                       | YYYY-MM-DD            | 日期，包含年、月、日            |
| YEAR      | 1           | 1901 至 2155                                   | YYYY                  | 年份                            |
| DATETIME  | 8           | 1000-01-01 00: 00: 00 至 9999-12-31 23: 59: 59 | YYYY-MM-DD HH: MM: SS | 混合日期和时间值                |
| TIMESTAMP | 4           | 1000-01-01 00: 00: 01 至 2038-01-19 03: 14: 07 | YYYY-MM-DD HH: MM: SS | 混合日期和时间值，时间戳        |

- 开发中，常用 `TIME` 、 `DATE` 、 `DATETIME` 



## 区分

###   `CHAR` 与 `VARCHAR` 

- 定长和变长
  - char 表示定长，长度固定。字符长度小于定义长度时，则用空格填充；
  - varchar表示变长，即长度可变。字符长度小于定义长度时，按实际长度存储。
  
- 存储的容量不同

  - 对 char 来说，最多能存放的字符个数 255，和编码无关。
  - 对 varchar 来说，最多能存放 65532 个字符。varchar的最大有效长度由最大行大小和使用的字符集确定。整体最大长度是 65,532字节。

- 性能
  - 时间上，因为 char 长度固定，所以它比 varchar 要快得多，方便程序的存储与查找
  - 空间上，由于 char 长度固定，所以会占据多余的空间
  - 总结起来， char 是以空间换取时间效率， varchar 是以时间换空间效率



## 小贴士

### 单引号

除INT，SMALLINT外，其余所有数据类型都需要加单引号。

### DESC

降序排列。

### ASC

升序排列，默认排序方式。

### 编译

当我们执行一条SQL语句时，DBMS会对这条语句进行编译。但不是每次执行SQL语句都有编译这一过程，例如，我们第一次执行 `SHOW DATABASES;` 时，DBMS会进行编译，如果我们紧接着再执行一次，那么这一次就不会进行编译了
