# 概念

## Maven的作用

依赖管理，项目构建



## POM

> Project Object Model，项目对象模型



## 仓库

> 用来存储资源，包含各种jar包

### 本地仓库

> 自己电脑上的仓库，通过连接远程仓库获取资源



### 远程仓库

> 本地仓库以外的仓库，为本地仓库提供资源

#### 中央仓库

> 由Maven团队维护，存储所有资源的仓库

- 这里只存储开源的jar包

#### 私服

> 团队（公司、部门....）的仓库，从中央仓库获取资源

- 作用
  - 保护具有版权的资源，包含购买或自主研发的jar包
  - 一定范围内共享资源，仅对内部开放，而不对外开放



## 坐标

> 描述仓库中资源的位置

使用唯一标识，唯一地定位资源位置，通过该标识可以将资源的识别和下载工作交给机器完成

### 组成

####  `groupId` 

> 组织ID，定义当前Maven项目隶属组织名称（通常是域名的反写）

####  `artifactId` 

> 项目ID，定义当前Maven项目的名称（通常是模块的名称）

####  `version` 

> 版本号，定义当前项目版本号

####  `packaging` 

> 定义该项目的打包方式

 **不属于坐标的组成** 



## 仓库配置

### 本地仓库

> 默认存放在 `${user.home}\.m2\repository` 下

在Maven安装目录下的 `conf` -> `settings.xml` 中找到 `<localRepository></localRepository>` 标签，将自定义的仓库位置粘贴到标签内即可

### 远程仓库

```xml
<repositories>
    <repository>
        <!-- 中央仓库 -->
        <id>central</id>
        <!-- 仓库名，可以不写 -->
        <name>Central Repository</name>
        <!-- 仓库地址，私服或者本地仓库的所有jar包都会从这里下载 -->
        <!-- 建议使用国内的镜像仓库 -->
        <url>https://repo.maven.apache.org/maven2</url>
        <layout>default</layout>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>
```

修改后的代码

###  `setting` 文件的区别

 `conf` 下的 `settings.xml` 是全局设置

我们可以在与仓库同一级文件夹下新建自己的个性化设置

```
// 假设本地仓库在 C:\Download\Maven\repository 
Maven
|---- repository
|---- settings.xml	// 这就是自定义的设置，如果与全局的不同，则会覆盖全局设置
```

## 常用命令

```shell
mvn compile		# 编译
mvn clean		# 清理
mvn test		# 测试
mvn package		# 打包
mvn install 	# 安装到本地仓库
```

##  `pom.xml` 

```xml
<!-- 标准的头，xml都有，不用管 -->
<?xml version="1.0" encoding="UTF-8"?>

<!-- 头信息 -->
	<!-- xmlns：命名空间，类似包名，因为xml的标签可自定义，需要命名空间来做区分 -->
		<!-- 例如，a.xml 和 b.xml 中都有<name></name>标签，有了命名空间，程序就可以将他们区分开来 -->
	<!-- xmlns:xsi：xml遵循的标签规范 -->
	<!-- xsi:schemaLocation：用来定义xmlschema的地址，也就是xml书写时需要遵循的语法 -->
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/maven-v4_0_0.xsd">
    
    <modelVersion>4.0.0</modelVersion>
    
<!-- 项目基础信息 -->
    <!-- 公司或组织的唯一标志，通常是域名的反写 -->
    <!-- 所有项目的生成路径 -->
    <!-- 命名规范：项目名称.模块.子模块 -->
    <groupId></groupId>
    <!-- 项目ID，或者称为模块名称 -->
    <!-- 例如，java-project就是项目ID，整个项目的代码都写在该文件夹下 -->
	<!-- userDao就是用户模块，用户相关的代码就写在这个文件夹下 -->
    <!-- 命名规范：唯一即可 -->
    <artifactId></artifactId>
	<!-- 项目的版本号，可以是任何字符，但一般像下面这样书写 -->
    <version>0.1</version>
	<!-- 打包类型，可取值：pom, jar, maven-plugin, ejb, war, ear, rar, par, .... -->
    <packaging>jar</packaging></packaging>
	<!-- 项目名称，这里就填写方便记忆的即可。 -->
	<!-- 此项可以不要 -->
	<name></name>

<dependencies>…</dependencies>
  <parent>…</parent>
  <dependencyManagement>…</dependencyManagement>
  <modules>…</modules>
  <properties>…</properties>
  <!– Build Settings –>
  <build>…</build>
  <reporting>…</reporting>
  <!– More Project Information –>
  <name>…</name>
  <description>…</description>
  <url>…</url>
  <inceptionYear>…</inceptionYear>
  <licenses>…</licenses>
  <organization>…</organization>
  <developers>…</developers>
  <contributors>…</contributors>
  <!– Environment Settings –>
  <issueManagement>…</issueManagement>
  <ciManagement>…</ciManagement>
  <mailingLists>…</mailingLists>
  <scm>…</scm>
  <prerequisites>…</prerequisites>
  <repositories>…</repositories>
  <pluginRepositories>…</pluginRepositories>
  <distributionManagement>…</distributionManagement>
  <profiles>…</profiles>
</project>
```





# 创建Maven项目

## 纯手工创建

```
// 假设项目位置在 C:\Java\Neko 下，该项目为Java项目，不是JavaWeb
Neko
|---- src
	  |---- main	// 程序存放位置
	  		|---- java			// 存放Java源代码
	  		|---- resources		// 存放配置文件
      |---- test	// 测试程序存放位置
      		|---- java			// 存放Java源代码
	  		|---- resources		// 存放配置文件，这些配置文件可能不适合在main中使用
|---- pom.xml	// 必须要有这个文件，才能成为Maven项目
```



## 使用模板创建

### Java工程

```shell
mvn archetype:generate
	-DgroupId=com.yaoyausagi	## 所有项目的存放路径
	-DartifactId=java-project	## 其中一个项目的存放路径，不能是一个已存在的Maven项目名
	-DarchetypeArtifactId=maven-archetype-quickstart
	-Dversion=0.1
	-DinteractiveMode=false
```

### JavaWeb工程

```shell
mvn archetype:generate
	-DgroupId=com.yaoyausagi	## 所有项目的存放路径
	-DartifactId=web-project	## 其中一个项目的存放路径，不能是一个已存在的Maven项目名
	-DarchetypeArtifactId=maven-archetype-webapp
	-Dversion=0.1
	-DinteractiveMode=false
```



## IDEA创建

