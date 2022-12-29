# JavaWeb + Tomcat报错信息

### MONTH Query: SELECT * FROM userinfo WHERE ID = ? Parameters: [21016021901]

日期格式有误，此处是月份无法转换



### Tomcat 404报错，但控制台和日志中没有报错信息

 **解决办法** ：在项目的classpath（src文件夹）下新建 `loggin.properties` ，并粘贴以下内容

```properties
handlers = org.apache.juli.FileHandler, java.util.logging.ConsoleHandler

############################################################
# Handler specific properties.
# Describes specific configuration info for Handlers.
############################################################

org.apache.juli.FileHandler.level = FINE
org.apache.juli.FileHandler.directory = ../logs
org.apache.juli.FileHandler.prefix = error-debug.

java.util.logging.ConsoleHandler.level = FINE
java.util.logging.ConsoleHandler.formatter = java.util.logging.SimpleFormatter
```





### XXX cannot be cast to class jakarta.servlet.Filter

 **原因** 

1. Tomcat 启动后先将tomcat/lib目录下的jar包全部读入内存，如果webapps目录里的应用程序中WEB-INF/lib目录下有相同的包，将无法加载，不同版本的包之间也会造成类似问题
2. Servlet现已由Apache管理，较新版本的Servlet规范已经将包名从 `javax` 改为了 `jarkata` 
3. Tomcat 10以上版本
4.  `Exception starting filter [包名]` 

 **解决办法** ：

1. 删除项目路径下的所有 `servlet-api.jar` ，重启Tomcat
2. 确认Filter所在包是 `jarkata` 而不是 `javax` 
3. 使用Tomcat 9
4. 删除或修改对应包的 `@WebFilter` 注释