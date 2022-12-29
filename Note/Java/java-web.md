

# Java Web



## Tomcat

> 开源免费的轻量级服务器软件，其logo是一只公猫，寓意该软件像猫一样轻盈

### 下载与安装

1. 打开 [Apache官网](https://apache.org/) ，向下滑到底部，找到 `Tomcat` 



# 编程前准备

### 建库

```sql
CREATE DATABASE st;
```

### 建表

```sql
CREATE TABLE stuinfo (
	`ID` CHAR(3) NOT NULL PRIMARY KEY UNIQUE,
    `Name` VARCHAR(255) NOT NULL,
    `RoomNum` VARCHAR(8) NOT NULL
) DEFAULT CHARSET = utf8;
```

### 添加一条数据

```sql
INSERT INTO stuinfo VALUES ("001", "尧", "D11-5032");
```





## 系统架构

#### C/S架构

> Client/Server（客户端/服务器），需要安装特定的客户端软件

##### 优点

- 速度快
  - 大量的数据都在本地，加载更快
- 界面酷炫
- 体验好
  - 基于前两点
- 服务器压力小
  - 大部分任务都在本机完成
- 安全
  - 数据安全，本地有缓存



##### 缺点

- 升级维护麻烦
  - 频繁
  - 安装困难
- ......



#### B/S架构

> Browser/Server（浏览器/服务器），实际上还是C/S，只不过C固定为浏览器

##### 角色

- Browser开发团队（常见浏览器：Chrome、FireFox、Edge....） <span id = "browser"> </span> 
- WEB Server开发团队（Tomcat、Jetty、JBOSS....） <span id = "webserver"> </span> 
- DB Server开发团队（MySQL、Oracle....） <span id = "dbserver"> </span> 
- WEBapp开发团队（我们） <span id = "webapp"> </span> 



##### 优点

- 升级方便，成本低
- 不需要安装特定的客户端网站，只需输入网址即可



##### 缺点

- 速度慢
- 体验差
- 不安全
- ......



通常客户使用的应该用C/S，而公司或组织内用B/S即可



## 端口号

> 一台计算机上，一个端口号唯一对应一个服务（进程）



## Servlet

> Server Applet，服务器端的小程序，JavaWeb开发的最核心规范，其作用是 [WEB Server](#webserver) 与 [WEBapp](#webapp) 解耦合

对于Java程序员来说，我们只需要编写一个类实现Servlet接口，并在配置文件中指定请求路径与类名的关系

Servlet同时还规定了以下内容

- WEBapp的目录结构
- 配置文件的名字、存放路径
- Java程序的存放路径



## 规范与协议

### 规范

 [WEB Server开发团队](#webserver) 与 [WEBapp开发团队](#webapp) 之间：JavaEE规范之一的Servlet规范

 [WEBapp开发团队](#webapp) 与 [DB Server开发团队](#dbserver) 之间：JDBC



### 协议

 [Brower](#browser) 与  [WEB Server](#webserver) 之间：HTTP协议（超文本传输协议）



## 规范

### 新建项目

1. 项目文件夹下新建文件夹，必须叫 `WEB-INF` 

2.  `WEB-INF` 下新建 `classes` ，用来存放编译后的Java程序，即字节码文件

3.  `WEB-INF` 下新建 `lib` ，用来存放第三方jar包，例如jdbc的jar包

4.  `WEB-INF` 下新建 `web.xml` 文件，该文件为描述文件，以下是一个基础的样式，通常建议复制粘贴

    `*.xml` 文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
                         https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
     version="5.0"
     metadata-complete="true">
   </web-app>
   ```

   

### 目录结构 <span id="mulujiegou"> </span> 

```
webapp-root
	|----WEB-INF
		|----classes（字节码文件）
		|----lib（第三方jar包）
		|----web.xml（配置文件，注册Servlet）
	|----html
	|----css
	|----js
	|----images
	....
```

- 请求响应的过程
  - 用户在浏览器中输入URL或者点击URL
  - Tomcat接收到请求，截取路径
  - Tomcat在 `web.xml` 文件中查找路径对应的Servlet是X
  - Tomcat通过反射机制创建X对象
  - Tomcat调用X对象的方法



## 向浏览器响应一段html代码

```
public 
```



# IDEA中开发Servlet

> 以查询数据库信息为例，并将信息响应到网页上

### 创建空项目（Empty Project）

打开IDEA新建一个空项目 `Neko` 

### 新建模块（Module）

在项目名上右键，添加模块 `Li-Hua` 

### 添加框架支持（Add Framework Support）

在模块名上右键，添加框架支持，选择Web Application，此时IDEA会自动为我们生成一个符合Servlet规范的Web app目录

### 编写Servlet

#### 新建包（Packages）

在模块下的src上右键，新建包 `com.yaoyausagi.javaweb.servlet` 

#### 新建类（Class）

在包上右键，新建类 `demo01` 

- 这个类应该是用来实现Servlet接口的，即 `public class demo01 implements Servlet {}` 

#### 导入jar包

上一步完成后，会出现报错，此时我们需要导入两个jar包来解决报错

- 点击 `File` -> `Project Structure` -> `Modules` -> `Dependencies` -> `+` -> `JARs or Directories...` 
- 找到Tomcat下lib内的 `jsp-api.jar` 和 `servlet-api.jar` ，选中两者，点击 `OK` 
- 勾选两者，再点击 `OK` ，导包成功

我们将光标移动到Servlet上，按下 `Alt` + `Enter` ，选择第一个选项即可解决报错

> 此时出现了五个方法（Servlet下的方法）

### 编写业务代码

在Servlet的service方法中编写业务代码

```java
// 设置响应类型
servletResponse.setContentType("text/html;charset=utf-8");
PrintWriter out = servletResponse.getWriter();

// JDBC经典六步
Connection conn = null;
PreparedStatement ps = null;
ResultSet rs = null;
String url = "jdbc:mysql://localhost:3306/st?serverTimezone=UTC";
String user = "root";
String passwd = "534760";
try {
    Class.forName("com.mysql.cj.jdbc.Driver");
    conn = DriverManager.getConnection(url, user, passwd);
    String sql = "SELECT * FROM stuinfo WHERE ID = ?";
    ps = conn.prepareStatement(sql);
    ps.setString(1, "001");
    rs = ps.executeQuery();
    while (rs.next()) {
        String id = rs.getString("ID");
        String name = rs.getString("Name");
        String roomNumber = rs.getString("RoomNum");
        out.print(id + " " + name + " " + roomNumber);	// 向网页输出查询到的信息
    }
} catch (ClassNotFoundException | SQLException e) {
    throw new RuntimeException(e);
} finally {
    if (rs != null) {
        try {
            rs.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    if (ps != null) {
        try {
            ps.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    if (conn != null) {
        try {
            conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### 注册 `demo01` 类

向 `web.xml` 文件中添加以下内容

```xml
<servlet>
    <servlet-name>demo01</servlet-name>
    <servlet-class>com.yaoyausagi.javaweb.servlet.demo01</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>demo01</servlet-name>
    <url-pattern>/demo01</url-pattern>	// 指定访问路径，这个可以自行取一个有意义的路径，不一定要写项目名
</servlet-mapping>
```

### 给出超链接 <span id="geichuchaolianjie"> </span> 

在网页（ `.html` 或者 `.jsp` ）中给出超链接，此处以 `.html` 为例

```html
<!-- 此处以带反斜杠（/）的项目名开始，加上上一步中指定的访问路径 -->
<!-- 如果在 .jsp 中，项目名是可以动态获取的，而不用手动指定 -->
<a href="/Li-Hua/demo01"></a>
```

> 注意，网页文件应该与 `WEB-INF` 在同一文件夹下，这是 `Servlet` 的 [规范](#mulujiegou) 

### 关联Tomcat服务器

在IDEA中关联Tomcat服务器，同时将项目部署到Tomcat服务器上

- 点击绿色锤子 （ `Build Project` ）旁的 `Add Configuration` 
- 点击左上角 `+` -> `Tomcat Server` -> `local` 
- 弹出新页面，要求我们配置Tomcat
  - Name： `Tomcat 10.0.23` （自行取名）
  - Server选项卡
    - Application server：点击 `Configure...` ，确认Tomcat路径与 `CATALINA_HOME` 的路径一致即可点击 `OK` 
    - Other browser
      -  `After launch` 表示当项目开始运行时自动打开选定的浏览器（在右边选定），可以自行决定是否开启。
      -  `JRE` 指定JDK版本
  - Deployment选项卡中，点击 `+` -> `Artifact...` ，将项目部署到Tomcat服务器上
    - 注意，我们还需要将 `Application context` 中的内容更改为 [给出的超链接](#geichuchaolianjie) 中的项目名
      -  `Application context` 表示应用的根，其实就是设置项目名
- 点击 `OK` ，完成关联及部署

### 启动Tomcat服务器

以Debug方式启动，稍等片刻

> 至此，我们已经可以在浏览器中访问demo01了，URL为 `localhost:8080/Li-Hua/test.html` 



# Servlet对象

### Servlet对象的生命周期？

综合来讲，Servlet对象的生命周期就是：一个Servlet对象从被创建到被销毁的全过程

### 什么时候被创建？

 [通常](#qidongshichuangjian) ，在用户向服务器发送请求时，Servlet对象才会被创建。而Servelt对象被创建后，有无参构造方法的，先执行无参构造方法，没有就先执行init方法

### 创建了几个？

我们可以在 `demo01` 类中添加一个无参构造方法，在JavaSE的学习时，我们知道当一个类被实例化时，会自动执行无参构造方法，如果我们不写无参构造方法，那么Java就会自动给出无参构造方法（方法内没有任何语句）。既然如此，我们就可以利用这个特性来验证Servlet对象到底被创建了几个。当然，为了方便观察，我们需要让这个无参构造方法在控制台输出一些内容，来让我们知道它被执行了。

```java
public demo01 {
    Syetem.out.println("无参构造方法被执行了");
}
```

结果我们发现，无参构造方法只被执行了一次，说明Servlet对象只被创建了一个，且所有的用户请求都由这个对象处理（我们称这是一个伪单例模式，因为Servlet对象的方法不是私有的）

### 什么时候被销毁？

### 由谁来维护？

- Servlet对象的创建、对象方法的调用、对象的销毁，我们WEBapp程序员无权干预
- Servlet对象的生命周期由Tomcat服务器（WEB Server）全权管理
- 我们又称Tomcat服务器为 `WEB容器（WEB Container）` 
- 注意，我们实例化的Servlet对象不受WEB容器管理，因为该对象根本没有放到WEB容器中
  - WEB容器创建的Servlet对象都会被放在一个集合（HashMap）中，即只有在这个集合中的Servlet对象才能被WEB容器管理
  - WEB容器的底层有一个集合（HashMap），这个集合中存放了Servlet对象和请求路径的关系

- 默认情况下，服务器在启动时并不会将项目中的Servlet对象实例化

  - 启动时将Servlet对象实例化可能会造成资源的浪费

  - 服务器在启动时，会解析 `web.xml` 文件，并将请求路径与Servlet类名放到Map集合中

### 如何在服务器启动时实例化Servlet对象？ <span id="qidongshichuangjian"> </span> 

前面我们说到，Servlet对象在用户发送请求时才会被创建。如果我们想在服务器启动时就创建Servlet对象，应该怎么操作呢？

- 在 `<servlet></servlet>` 标签中添加 `load-on-startup` 子标签，并在该子标签中添加一个整数，整数越小，创建优先级越高

  ```xml
  <servlet>
      <servlet-name>demo01</servlet-name>
      <servlet-class>com.noisekiller.javaweb.servlet.demo01</servlet-class>
      <!-- 添加以下标签，并添加一个整数，该整数越小，则该Servlet对象越先被实例化 -->
      <!-- 我们可以在demo01中添加一个无参构造方法验证 -->
      <load-on-startup>0</load-on-startup>
  </servlet>
  <servlet-mapping>
      <servlet-name>demo01</servlet-name>
      <url-pattern>/servlet/search</url-pattern>
  </servlet-mapping>
      
  <servlet>
      <servlet-name>demo02</servlet-name>
      <servlet-class>com.noisekiller.javaweb.servlet.demo01</servlet-class>
      <!-- demo02会在demo01实例化后，再被实例化 -->
      <!-- 我们可以在demo01中添加一个无参构造方法验证 -->
      <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
      <servlet-name>demo01</servlet-name>
      <url-pattern>/servlet/search</url-pattern>
  </servlet-mapping>
  ```

# ServletConfig对象

> ServletConfig接口由WEB服务器（此处是Tomcat）实现

### ServletConfig对象是什么？有什么用？

被译为Servlet对象的配置信息对象，Tomcat在实例化一个Servlet对象时，会将该对象在 `web.xml` 文件中的配置信息包装成一个ServletConfig对象，传递给init方法，所以ServletConfig对象就是用来存储Servlet对象的配置信息的

- 对于 `demo01` 对象而言，这些就是它的配置信息（ `<servlet></servlet>` 标签内的内容）

  ```xml
  <servlet>
      <servlet-name>demo01</servlet-name>
      <servlet-class>com.noisekiller.javaweb.servlet.demo01</servlet-class>
      ....
  </servlet>
  ```

### 创建了几个？

一个Servlet对象对应一个ServletConfig对象

# ServletContext对象

> ServletContext接口由WEB服务器（此处是Tomcat）实现，又称应用域
>

### ServeltContext对象是什么？有什么用？

被译为Servelt对象的上下文对象（环境对象），其对应的是整个 `web.xml` 文件。如果所有的Servelt对象共享一份数据，且该数据数据量小、几乎不会修改，就可以将这些数据放到ServeltContext这个应用域中。

### 什么时候被创建？

在服务器启动时，由WEB服务器创建

### 创建了几个？

一个WEBapp只有一个ServletConfig对象，也就是说多个Servlet对象共享一个ServletContext对象

### 什么时候被销毁？

服务器关闭的时候被销毁

### 




# 认识接口

## Servlet接口

> 由程序员实现

### Servlet接口的方法

Servlet对象只会被创建一次，不管有多少个用户在发送请求，这些请求都会通过这一个对象完成。也就是说，这个对象是多线程共享的

####  `public void init(ServletConfig servletConfig);` 

- init被译为初始化，是用来完成初始化操作的，例如初始化数据库连接池、线程池....
  - init方法在demo01对象被创建（实例化）后立刻就执行了，且只执行一次
  - 注意，init方法执行时，Servlet对象已经被创建了

```java
@Override
public void init(ServletConfig servletConfig) throws ServletException {
}
```

#### getServletConfig方法

```java
@Override
public ServletConfig getServletConfig() {
	return null;
}
```

从名字来看，这个方法是用来获取 `ServletConfig` 类型对象的

#### service方法

- service方法是处理用户请求的核心方法，也就是说我们会把业务代码写在service方法里
  - 用户进行多少次请求，service方法就会被执行多少次

```java
@Override
public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
}
```

#### getServletInfo方法

```java
@Override
public String getServletInfo() {
    return null;
}	
```

获取Servlet的作者、版权信息等

#### destory方法

- destory方法在Tomcat销毁demo01对象前（即将被销毁时）执行，且只执行一次
  - destory方法执行后，demo01对象就被销毁了
  - 通常，我们可以将关闭资源和流，保存数据等的代码写在destory方法中

```java
@Override
public void destroy() {
}
```

#### 为什么要有init方法

> 你可能会好奇，init方法和无参构造方法那么像，init方法有什么存在的意义呢？

程序员在编写Servlet对象时，可能会将初始化的代码写在一个带参构造方法里，而当我们提供了带参构造方法时，Java将不会再为我们提供无参构造方法，这就会导致Servelt对象实例化出错。

因此，SUN公司在制定Servelt规范时就提供了初始化的方法（init方法），避免程序员自己去写构造方法，从而避免出现报错。

### 适配器模式改造Servlet

#### 新建 `GenericServlet` 类

之前的代码中，我们编写的类都是直接实现Servlet接口，整个页面会显得很凌乱，因为Servlet提供的五个方法里我们常用的只有service，其他的方法显得很碍眼。所以我们可以考虑用适配器模式改造一下Servlet接口

```java
public abstract class GenericServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    // 将service方法抽象
    @Override
    public abstract void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException ;

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

当我们在写Servlet时选择继承 `GenericServlet` 类，此时Servlet代码中只需要重写service方法，代码整体简洁不少

```java
public class demoAdapter extends GenericServlet {
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {

    }
}
```

#### 完善 `GenericServlet` 类

> 实际上也是在完善Servelt

##### 保存 `servletConfig` 变量

由于init方法只执行一次，而init方法接收到的 `servletConfig` 变量又是由Tomcat服务器创建的，保不齐service方法也会用到。所以我们要把init方法接收到的 `servletConfig` 变量保存下来，方便后续使用

```java
public abstract class GenericServlet implements Servlet {
    private ServletConfig servletConfig;	// 创建一个私有的servletConfig成员变量

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        this.servletConfig = servletConfig;	// 将init方法接收到的servletConfig变量
    }

    @Override
    public ServletConfig getServletConfig() {	// Servlet接口提供的get方法
        return servletConfig;	// 原本返回的是null，现在返回servletConfig成员变量
    }

    @Override
    public abstract void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException ;

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

##### 保护init方法

> 上一步中我们成功将 `ServletConfig` 变量保存了下来，并且能够在Servlet对象的service方法中通过 `getServletConfig()` 调用。但现在仍有一个问题，如果我们在Servlet对象中重写了init方法（毕竟有时还是会用到init方法的），那么Tomcat服务器就不会调用 `GenericServlet` 类中的init方法了（子类中有时，不调用父类的；子类中没有时，到父类中找），那么很可能会出现报错，还会导致 `ServletConfig` 变量很可能就没了。

我们可以在 `GenericServlet` 类中用 `final` 修饰init方法，并通过方法重载的方式提供一个无参init方法给 `demoAdapter` 类重写

```java
public abstract class GenericServlet implements Servlet {
    private ServletConfig servletConfig;

    @Override
    public final void init(ServletConfig servletConfig) throws ServletException {
        this.servletConfig = servletConfig;
        // 调用无参init方法
        this.init();
    }
    
    public void init() {
    	
    }

    @Override
    public ServletConfig getServletConfig() {
        return servletConfig;
    }

    @Override
    public abstract void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException ;

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

- 起因：在保证代码简洁的的前提下，我们希望在 `demoAdapter` 类中用init方法初始化一些东西，同时又希望抽象类中 `ServletConfig` 变量得以保留
- 思路：在 `GenericServlet` 类中用 `final` 修饰init方法，并重载一个无参init方法，在 `final` 修饰的init方法中调用了无参init方法。又因为 `GenericServlet` 类中的无参init方法被 `demoAdapter` 类重写了，所以其执行的是重写后的代码

### 注意

- 我们不建议在编写Servlet对象时手动添加其他方法，这可能会导致程序出错（不符合Servlet规范）
- 无参构造方法、init方法（很少用）、destory方法（很少用）都只执行一次，service方法有多少个请求就执行多少次
-  **如果我们写的类继承 `GenericServlet` ，那么 `Servlet` 接口的方法可以直接通过 `this` 调用** 

-  **`GenericServlet` 类是Servlet提供了的，上面是在讲解原理，实际上我们不需要自己写** 

  - 在SUN公司编写的 `GenericServlet` 类并没有用 `final` 修饰init方法，这个没有关系。只要我们在开发的Servlet不要重写 `public void init(ServletConfig servletConfig) throws ServletException {}` 方法就行。


## ServletConfig接口

> 由WEB服务器实现，不同的服务器可能存在包名和类名的差异

### ServletConfig接口的方法

>  `<init-param></init-param>` 标签用来设置初始化参数
>
>  `<param-name></param-name>` 标签用来设置参数名
>
>  `<param-value></param-value>` 标签用来设置属性值

####  `getInitParameterNames()` 

- 获取 `web.xml` 文件中所有 `<param-name></param-name>` 标签内的值（ `Enumeration<String>` 集合）

`getInitParameter(String name)` 

- 返回 `<param-value></param-value>` 标签内的值，该方法需要传递属性名

```xml
<servlet>
        <servlet-name>demo00</servlet-name>
        <servlet-class>com.yaoyausagi.javaweb.demo.servlet.demo00</servlet-class>
	    <!-- 返回所有param-name的值，标签在前的，排得越靠后 -->
        <init-param>
            <param-name>url</param-name>	<!-- 属性名 -->
            <param-value>jdbc:mysql://localhost:3306/st?serverTimezone=UTF8</param-value>	<!-- 属性值 -->
        </init-param>
        <init-param>
            <param-name>user</param-name>
            <param-value>root</param-value>
        </init-param>
        <init-param>
            <param-name>password</param-name>
            <param-value>534760</param-value>
        </init-param>
</servlet>
```

-   `<init-param></init-param>` 用来设置单个Servlet对象的属性，仅对单个Servlet对象生效

####  `getServletName()` 

-  返回值为当前Servlet对象的名字（字符串类型）

####  `getServletContext()` 

-  返回值为ServletContext对象

向网页输出属性名和属性值

```java
PrintWriter out = servletResponse.getWriter();
ServletConfig config = this.getServletConfig();	// 获取ServletConfig对象
Enumeration<String> initParameterNames = config.getInitParameterNames();	// 接收返回的属性名的集合
while (initParameterNames.hasNext()) {
    String name = initParameterNames.nextElement();	// 取出字符串类型的属性名
    out.print(name+"="+config.getInitParameter(name));	// 通过属性名获取属性值
    out.print("<br>");
}
```

.........

### 注意

 **如果我们写的类继承 `GenericServlet` ，那么 `ServletConfig` 接口的方法不一定要通过 `ServletConfig` 对象调用，因为 `GenericServlet` 也实现了 `ServletConfig` 接口的方法，可以直接通过 `this` 调用** 

## ServletContext接口

### ServletContext接口的方法

-  `getServletContext()` 

  -  返回 `ServletContext` 对象

-  `getInitParameterNames()` 

  - 返回 `web.xml` 文件中 `<param-name></param-name>` 内的值（ `Enumeration<String>` 集合）

-   `getInitParameter(String name)` 

  -  返回 `web.xml` 文件中 `<param-value></param-value>` 内的值，该方法需要传递属性名

  ```xml
  <context-param>
      <param-name>pageSize</param-name>	<!-- 属性名 -->
      <param-value>10</param-value>	<!-- 属性值 -->
  </context-param>
  <context-param>
  	<param-name>startIndex</param-name>
      <param-value>0</param-value>
  </context-param>
  ```

  -  `<context-param></context-param>` 用来配置全局配置信息，对WEBapp中的每一个Servlet对象都起作用
     -  如果仅仅想对某一个Servlet对象起作用，请配置到 `<init-param></init-param>` 标签内

-  `getContextPath()` 
  - 通过ServletContext对象调用，动态获取应用的根目录
    - 在java源码中，不要将应用的根目录写死，因为项目最终起什么名字大多数时候是未知的
-  `getRealPath(String path)` 
  - 获取文件的绝对路径，必须写到该文件所在的文件夹
    - 例如，获取WEB-INF下images中的a.png，应该写作 `"/images/a.png"` （ `"/a.png"` 是错误的）
-  `log(String msg)` 
  -  写入日志，在CATALINA_HOME（Tomcat）的logs目录下
    - 在IDEA中开发，则应该到IDEA的目录下查找
-  `setAttribute(String name, Object value)` 
  - 向ServletContext对象中存数据
-  `getAttribute(String name)` 
  - 获取ServeltContext对象中的数据，返回值为 `Object` 类型
-  `removeAttribute(String name)` 
  - 删除ServletContext对象中的数据

# 问题

## 中文乱码没解决

## 什么是单例模式
