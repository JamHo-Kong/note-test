# 概念

## 面向接口编程



## 代理模式

目标对象不可以直接访问，通过代理对象增强功能访问。



### 功能

1. 控制目标对象的访问
2. 增强功能



### 分类

>  **场景设定** 
>
> - 业务功能											   ===>请明星进行表演
>
>   - 明星刘德华									===>目标对象（无法直接访问）
>     - 唱歌
>     - 讲话
>     - ......
>       - 业务功能必须且只能由目标对象来实现
>
>   - 刘德华助理									===>代理对象（我们可以访问，他还可以和明星对接）
>     - 预定时间
>     - 预定场地
>     - 商议费用
>     - ......
>       - 这些功能是目标对象需要的，但目标对象不会去实现这些功能
>         - 明星不会花时间来谈价格什么的
>       - 同样的，代理对象不会去实现目标对象的功能
>
>   - 我们												===>客户端对象
>     - 和助理（代理对象）打交道

#### 静态代理

##### 特点

- 目标对象和代理对象实现同一个业务接口
- 目标对象必须实现业务接口
- 代理对象在程序运行前就已经存在了
- 能够灵活切换目标对象，却无法进行功能的灵活处理（因此我们要使用动态代理来解决）

##### 实现





#### 动态代理

> 又称JDK动态代理，CGLib代理（子类代理）

代理对象在程序运行过程中动态地在内存中构建，可以灵活地进行业务功能的切换

##### 特点

1. 目标对象必须实现业务接口
2. 代理对象不需要实现业务接口，且它只能代理业务接口中的方法（不同于静态代理）
3. 动态代理的对象在程序运行前不存在，在程序运行时动态地在内存中构建
4. 动态代理能灵活地实现业务功能的切换（不同于静态代理）
5. 目标对象中特有的方法（非接口中的方法）不能被代理



##### 实现

###### 几个类

> 使用现成的工具类实现JDK动态代理

1. Proxy类

   > java.lang.reflect.Proxy

   主要使用 `Proxy.newProxyInstance` 方法，这个方法专门用来生成动态代理对象

   ```java
   public static Object newProxyInstance(ClassLoader loader,	// 类加载器，把目标对象加载到JVM中
                                         Class<?>[] interfaces,	// 目标对象实现的所有接口
                                         InvocationHandler h	// 类似代理对象的功能。代理的功能和目标对象的业务功能调用在这做
                                        ) throws IllegalArgumentException 
   {....}
   ```

2. Method类

   > 反射用的类，用来实现目标对象的反射调用

   主要使用 `Method.invoke()` 方法，手动调用目标方法。

3. InvocationHandler接口

   实现代理和业务功能，在调用时使用匿名内部类实现

   - 匿名内部类就是在实例化接口的同时，实现接口的功能

     ```java
     // 假设存在接口Service，该接口内有一个无返回值的sing功能
     public class MyTest() {
         public void Test01() {
             Service service = new Service() {
                 @Override
                 public void sing() {
                     System.out.println("运行了匿名内部实现类。");
                 }
             }
         }
     }
     ```


###### 基本实现

 **业务接口** 

```java
public interface Service {
    void sing();
    void speak();
}
```

 **目标对象1** 

```java
public class SuperStarLiu implements Service {
    @Override
    public void sing() {
        System.out.println("刘德华：正在唱歌");
    }
    
	@Override
    public void speak() {
        System.out.println("刘德华：正在讲话");
    }
}
```

 **目标对象2** 

```java
public class SuperStarZhou implements Service {
    @Override
    public void sing() {
        System.out.println("周润发：正在唱歌");
    }

    @Override
    public void speak() {
        System.out.println("周润发：正在讲话");
    }
}
```

 **动态代理** 

```java
public class ProxyFactory {
    // 类中的成员变量设计为接口
    // 这个变量就是这个类用来存储目标对象的变量
    Service target;

    // 接受目标对象
    public ProxyFactory(Service target) {
        this.target = target;   // 将调用方法中传过来的目标对象存到成员变量中去，
    }

    // 获取动态代理对象
    public Object getAgent() {
        return Proxy.newProxyInstance(
                // 类加载器，完成目标对象的加载
                target.getClass().getClassLoader(),
                // 目标对象实现的所有接口
                target.getClass().getInterfaces(),
                // 实现代理功能的接口，此处传入的是匿名内部实现类
                new InvocationHandler() {
                    // invoke方法中写静态代理方法中的代码，也就是说它类似静态代理中的代理对象
                    // 它们的区别在于这里使用method调用目标对象的方法，从而实现了业务功能的灵活切换
                    // 返回值为目标对象方法的返回值
                    public Object invoke(
                            Object proxy,   // 创建代理对象
                            Method method,  // method就是目标方法
                            Object[] args   // 目标方法需要的参数
                    ) throws Throwable{
                        System.out.println("场地");
                        Object object = method.invoke(target, args);    // 执行目标对象中的方法
                        System.out.println("费用");
                        return object;
                    }
                }
        );
    }
}
```

######  **测试类** 

```java
public class Demo {
    @Test
    public void Demo01() {
        ProxyFactory proxyFactory = new ProxyFactory(new SuperStarLiu());
        // 注意：即便此处进行了强转，但service是JDK动态代理类型，而非Service类型。
        Service service = (Service) proxyFactory.getAgent();
        service.sing();
        service.speak();
        // 如果我们要添加新的功能（增强功能），只需要业务接口中定义新方法，并在目标对象中实现新方法即可
        // 而无需修改代理类
    }
}
```

##### CGLib动态代理

> 又称子类代理，通过动态地在内存中构建子类对象，重写父类方法进行代理功能的增强
>
> 如果目标对象没有实现接口，则只能通过CGLib子类代理来进行功能增强
>
> 子类代理是通过字节码框架ASM来实现的

