# Spring系统架构

系统中，上层依赖下层

![系统架构4.0](E:\Files\github\jamho-kong.github.io\docs\images\Java\spring-grammar\2022-09-29.png)

接下来的内容会根据该图进行逐一讲解



## Core Container

### 核心概念

#### IoC (Inversion of Control)

 **控制反转** 。指将对象创建控制权转移到外部，在书写代码时不再主动使用 `new` 实例化对象，而是改为由外部提供对象。其目的在于降低代码的耦合度（高内聚，低耦合）。

- Spring将该思想实现了，所以Core Container实际上也可称为 **IoC Container（IoC容器）** 
- IoC容器中负责对象的创建、初始化等一系列工作。这些对象在IoC容器中统称为 **Bean** 

#### DI (Dependency Injection)

依赖注入。在容器中创建Bean之间依赖关系的过程。

```java
public calss BookServiceImpl implements BookService {
    
    // 这段代码要想成功运行，那么IoC容器里的BookService对象和bookDao对象就必须要存在依赖关系
    // 而DI就是实现这两个对象依赖关系绑定的思想
    private BookDao bookDao;
    
    public void save() {
        bookDao.save();
    }
}
```

````
IoC容器
|---- BookSrvice
|---- bookDao
````

 **基于这两个思想，Spring实现了代码的解耦合。** 



### Bean

> Spring也是使用构造方法（无参构造方法）创建的对象

#### Bean实例化的三种方法

##### 构造方法

> 此方法如果不提供无参构造方法，将抛出异常 `BeanCreationException` 

1. 提供可访问的构造方法（无参构造方法）

2. 在 `.xml` 中配置

   ```xml
   <bean></bean>
<!-- 或者 -->
   <bean/>
   ```


##### 静态工厂

> 使用这种方法，需要指定工厂类的路径以及创建目标bean的方法名（少了这一步，创建出来的是工厂类对象）
>
> 这种方法用作兼容早期的一些项目，了解即可

1. 提供一个静态工厂类

2. 在 `.xml` 中配置

   ```xml
   <bean id="" class="静态工厂类路径" factory-method="工厂类中创建目标对象的方法名"/>
   ```

##### 实例工厂

1. 提供一个实例工厂类

2. 在 `.xml` 中配置

   ```xml
   <!-- 先配置实例工厂 -->
   <bean id="" class="实例工厂类路径" factory-method="工厂类中创建Bean的方法名"/>
   
   <!-- 删除class属性，新增factory-bean，指定实例工厂的id -->
   <bean id="" factory-bean="" factory-method="工厂类中创建Bean的方法名"/>
   ```

- 实例工厂bean显得多余，这个bean的存在仅仅是为了在写目标类的factory-bean时用
- factory-method不固定，每次都要重写，很麻烦

###### 实例工厂的优化（重要）

> Spring提供

1. 创建工厂Bean类

   ```java
   // 此处假设需要一个创建UserDao的工厂
   // 这个类要实现FactoryBean<T>这个接口
   // 而这个接口需要的泛型参数填Bean的类型
   public class UserDaoFactoryBean implements FactoryBean<UserDao> {
       // 代替原始实例对象工厂中创建Bean的方法、
       // 原本的代码写在这里，解决了factory-method中名字不固定的问题
   	public UserDao getObject() throws Exception {
           return null;
       }
   
       // 要创建的Bean的类型
       public Class<?> getObjectType() {
           return UserDao;
       }
       
       // 返回的Bean是否为单例
       // true是单例，false不是
       // 通常都是单例的，所以这个方法一般不写（如 配置文件 - Bean 部分说的那样，交给Spring管理的对象一般都是单例的）
       public boolean isSingleton() {
           return true;
       }
   }
   ```

2. 在 `.xml` 中配置

   ```xml
   <!-- 此处指定的class虽然是工厂Bean，但实际创建的是Bean类型 -->
   <!-- 即工厂Bean中 getObject() 的返回类型 -->
   <bean id="" class="工厂Bean的路径"/>
   ```

#### Bean的生命周期********



# 入门案例

> 创建一个Maven项目

## 目录结构

```
main
|---- dao
		|---- BookDao（Interface）
		|---- impl
				|---- BookDaoImpl
|---- service
		|---- BookService
		|---- impl
				|---- BookServiceImpl
|---- App（程序入口）

resources
|---- applicationContext.xml
```





## 导入Spring坐标

> pom.xml

```xml
<!-- 导入Spring的依赖 -->
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.3.23</version>
</dependency>
```



## IoC

### 将被管理的对象告诉IoC容器

> 通过配置文件（ `applicationContext.xml` ）告知IoC容器

```xml
<!-- bean标签用来配置bean -->
	<!-- id属性是bean的名字 -->
	<!-- class属性是bean的类型 -->
<bean id="bookService" class="com.yaoyausagi.service.impl.BookServiceImpl"/>
<bean id="bookDao" class="com.yaoyausagi.dao.impl.BookDaoImpl"/>
```



### 获取IoC容器？

> 在实现类中使用 `ApplicationContext` 接口获取IoC容器

```java
// ClassPathXmlApplicationContext()需要的参数是.xml文件
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
```



### 从IoC容器中获取Bean

> 在实现类中调用 `ApplicationContext` 接口的方法 `getBean()` 获取Bean

```java
// getBean()需要的参数是applicationContext.xml里bean的id
BookService bookService = (BookService) applicationContext.getBean("bookService");
```



## DI

> 通过依赖注入，解决在代码中new对象的耦合问题

### 配置service和dao的关系

> 在配置文件（ `applicationContext.xml` ）中配置

```xml
<bean id="bookService" class="com.yaoyausagi.service.impl.BookServiceImpl">
    <!-- property用来配置bookService的属性 -->
    	<!-- name属性表示属性的名称，要和调用类中的对象名相同 -->
    	<!-- ref属性表示property标签的参照了xml中的哪个bean，此处就是bookDao -->
	<property name="bookDao" ref="bookDao"/>
</bean>

<!-- 此处的id对应property标签里的ref -->
<bean id="bookDao" class="com.yaoyausagi.dao.impl.BookDaoImpl"/>
```



### 在调用 `BookServiceImpl` 的类中设置 `set` 方法

```java
// 注入BookDao类型的Bean
private BookDao bookDao;

// 提供一个set方法给IoC容器，让其将创建好的bookDao对象传进来，赋给注入的Bean
// 这样才能在当前的类里调用BookDao的方法
public void setBookDao(BookDao bookDao) {
    this.bookDao = bookDao;
}
```



# 配置文件

##  `<bean>` 

> 可以是单标签

### 属性

-  `id` ：配置文件中 `id` 属性应当唯一，可以用于指定bean之间的关系（ `ref` 属性）和代码中获取bean
-  `name` ：为bean起多个名字，名字间用 `;` 、 `,` 或 `空格` 隔开，其作用与id几乎一样
-  `scope` ：指定作用范围，默认值为 `singleton` 



### 作用范围

> 默认情况下，Spring创建的对象是单例的

实际应用中，IoC中的bean多数为可复用的，这类bean没必要每次都new，所以Spring默认创建单例的bean。（节约了资源）

#### 适合交给容器管理的bean

- 表现层对象
- 业务层对象
- 数据层对象
- 工具类对象

#### 不适合的

- 封装实体的域对象（实体类）