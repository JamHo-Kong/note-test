# Java后端学习路线

​		根据网上各类教程总结得出Java后端学习路线，从上往下学习，由浅及深，由入门到高级， ~~由脱发到植发~~ 。本文也可用于面试前复习。

​		其中标注 `*` 的内容是后端攻城狮应该了解但不需要深入学习的。例如，了解前端的框架，可以让我们在实际工作中更好地和前端对接。

​		文中的内容可能有但不限于名词错误、拼写错误、解释错误，如果你在学习过程中发现了，请自行修正或者留言。





## Java基础

### 变量



### 控制结构

- 顺序结构

- 分支

- 循环



### OOP（面向对象）

- 封装

- 继承

- 多态



### 数组



### Java API

> 学会查阅API文档



### 异常和处理



### 集合



### 泛型



### IO



### 反射



### 网络通信





## MySQL基础

### SQL



### JDBC

- PreparedStatement

  > 解决SQL注入

- JDBCUtils

  > 即JDBC工具类，把常用的，必要的步骤、方法写到 `JDBCUtils` 中，减少代码书写量



### 事务



### 连接池

- c3p0

- DBCP

- Druid



## Java高级

### Java多线程/高并发

- 并发基础

  - 互斥同步

  - 非阻塞同步

  - 指令重排

  - synchronized

  - volatile

- 线程

- 锁

  - 自旋锁

  - 偏向锁

  - 可重入锁

- 线程池

- 并发容器

- JUC

  - executor

  - collections

  - locks

  - atomic（原子类）

  - tools
    - CountDownLatch
    - Exchanger
    - ThredLocal
    - CyclicBarrier



### 数据结构与算法

- 数据结构

  - 数组（稀疏数组）

  - 队列

  - 栈

  - 链表

  - 树

  - 散列

  - 堆

  - 图

- 算法

  - 排序算法
    - 冒泡
    - 选择
    - 插入
    - 快排
    - 计数
    - 归并
    - 堆

  - 查找算法

  - 分治算法

  - 动态规划
    - 背包问题

  - 回溯算法
    - 骑士周游问题

  - 贪心算法

  - KMP算法*

  - Prim算法*

  - kruskal算法*

  - floyd*
    - 最短路径

  - 迪杰斯特拉算法*
    - 最短路径



### 设计模式（23种）

> 学会常用的8种即可

- 单例模式

- 观察者模式

- 工厂模式

- 适配器模式

- 装饰者模式

- 代理模式

- 模板模式

- 职责链模式

- 其它*

  - 组合模式

  - 桥接模式

  - 原型模式



### JVM

- JVM体系
- 类加载过程/机制
- 双亲委派模式
- JMM（Java内存模式）
- 字节码执行的过程/机制
- GC（垃圾回收算法）
- JVM性能监控和故障定位
- JVM调优





## JavaWeb

### 前端基础

- HTML
- CSS
- JavaScript
- Ajax
- jQuery



### 前端框架*

- VUE
- React
- Angular
- bootstrap
- Node.js



### JavaWeb后端

#### Tomcat

#### Servlet

#### JSP





## 主流的框架和项目管理

### Linux操作系统



### Nginx

> 反向代理的Web服务器



### SSM

- Spring

  > 轻量级的容器框架

- SpringMVC

  > 分层的Web开发框架

- MyBatis

  > 持久化框架



### 项目管理

- Maven
- Git&Github
-  ~~SVM~~ 



### 数据库

- Redis
- MySQL
- Oracle



### 其它框架

- WebService

  > 即SOA

- Activiti

  > 工作流框架

- Shiro

  > 安全框架

- Spring Security

  > 安全框架

- JPA

  > 持久化

- SpringData

  > 持久层的通用解决方案



## 分布式 微服务 并行架构

### Netty



### Dubbo



### FastDFS

> 分布式的文件系统



### Docker

> 应用容器引擎



### Spring家族

- SpringBoot

- SpringCloud

  > 内部包含的组件很多

- Nacos

  > 阿里巴巴，能够做服务发现、配置、管理

- Seata

  > 阿里巴巴，能够做分布式事务的中间件

- Sentinel

  > 阿里巴巴，能够做流量控制、熔断、系统负载保护

- GateWay

  > 网关、日志、监控、限流、鉴权

- OpenFeign

  > 服务间调用



### 搜索引擎

- ElasticSearch

- Solr



### 中间件

- 数据库中间件

  - MyCat

    > 用来分库分表

- 消息中间件
  - ActiveMQ
  - RabbitMQ
  - KafKa



### 日志分析与监控（ELK）

- ElasticSearch

  > 搜集、存储数据

- LogStash

  > 分析日志

- Kibana

  > 日志的可视化



### Zookeepr

> 一致性服务：配置维护、域名维护、分布式同步等





## 开发运维一体化技术（DevOps）

> 自动化部分管理项目或者说是自动化部署

> 解决CI/CD

### k8s

> 让部署容器化的应用简单高效



### 普罗米修斯（prometheus）

> 系统监控和报警



### Jenkins

> 监控持续的工作，比如部署、集成、交付



### Harbor

> 容器的镜像仓库



### GitLab



### SonarQube

> 项目工程代码质量检测





## 大数据技术*

> 了解即可

### Hadoop



### Hive



### Impals



### spark



### flink





## 项目

> 至少动手做三个项目，才能掌握前面的知识，以下是热门的项目方向

### 电商



### 金融



### 教育



### 直播



### CRM、ERP





## 大厂高频面试题

> 公司面试喜欢从以下方面提问

**Java高级（面试的重点）**、操作系统、**SSM**、**数据库**、**Spring家族**、**Netty**、**项目**、Redis优化、中间件、SpringCloud





## 底层源码与内核





## 本科目标

计算机网络、操作系统、编译原理、离散数学、数值分析、计算机组成原理、汇编语言