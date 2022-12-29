# 目前看到 P35



## 概念

### 引号

Java中严格区分 `""` 和 `''` ，字符串型字面值必须用 `""` 括起来，字符型字面值用 `''` 括起来



### 类

> 一个源文件中可以有若干个类，每一个类中都可以编写 `main` 函数，最多有一个与源文件同名的 `public` 类



### 封装<span id="fengzhuang"> </span> 



### 继承<span id="jicheng"> </span> 

> 子类（或称派生类）继承父类（或称基类、超类）的特征和行为，使得子类对象（实例）具有父类的实例域和方法，或子类从父类继承方法，使得子类具有父类相同的行为。通过继承可以快速创建新的类，提高代码的复用性，提高程序的可维护性，节省大量创建新类的时间，提高开发效率和开发质量。



### 解耦合



## 运算符





## 标识符

> 所有自行命名的单词都称作标识符

### 命名规则

1. 只能由数字、字母、下划线、美元符号组成
2. 不能以数字开始
3. 严格区分大小写



### 命名规范

> 命名不能以下划线、美元符号开始或结束，且尽量表意清晰，不要嫌名字太长

- 类名：每个单词首字母大写，有复数含义则使用复数形式
- 方法名、参数名、成员变量、局部变量：采用驼峰命名法
  - getHttpMessage / localValue
- 常量：全部字母大写，单词间用下划线隔开
  - MAX_STOCK_COUNT
- 抽象类：以 Abstract 或 Base 开头
- 异常类：以 Exception 结尾
- 测试类：以它要测试的类名开始， Test 结束
- 包名：全部字母小写，且只含有一个自然语义的英语单词的单数形式
- 如果模块、接口、类、方法使用了设计模式，应该在名字中体现出来
  - 工厂模式：public class OrderFactory;

> 详见阿里巴巴Java编程规范



### 常见

- 类名
- 接口名
- 常量名
- 变量名
- 方法名
- 抽象类
- 异常类
- ......



## 字面值

> Java程序中的固定值称为字面值，本质就是数据

- 整数型字面值
- 浮点数型字面值
- 字符串型字面值
- 字符型字面值
- 布尔型字面值



## 变量

- 同一作用域内不能出现同类型的同名变量
- 可重复赋值



### 方法变量

> 在方法内声明的变量，又称局部变量



### 成员变量

> 在方法外声明的变量，又称全局变量



### 静态变量

> 由 `static` 修饰的变量称为静态变量，见 [static](#static)



### 区别

|          | 方法变量                                                     | 成员变量                   |
| -------- | ------------------------------------------------------------ | -------------------------- |
| 默认值   | 没有默认值，所以必须先赋值才能使用                           | 有默认值，可以不赋值就使用 |
| 修饰符   | 不可加修饰符                                                 | 可加修饰符                 |
| 作用域   | 定义它的方法内可用                                           | 本类、其他类中通过对象调用 |
| 命名     | 可以和当前类的成员变量重名，调用时遵循就近原则，或用 `this` 指定调用成员变量 |                            |
| 生命周期 | 短，与方法共存亡                                             | 长，与对象共存亡           |





## 数据类型 <span id="leixing"> </span> 

<table>
    <tr>
        <td colspan="3"><strong><i>八大基本类型，三大引用类型</i></strong></td>
        <th>默认值</th>
        <th>占用字节</th>
    </tr>
    <tr>
        <td rowspan="8">基本数据类型</td>
        <td rowspan="4">整型</td>
        <td>byte</td>
        <td>0</td>
        <td>1</td>
    </tr>
    <tr>
        <td>short</td>
        <td>0</td>
        <td>2</td>
    </tr>
    <tr>
        <td>int</td>
        <td>0</td>
        <td>4</td>
    </tr>
    <tr>
        <td>long</td>
        <td>0</td>
        <td>8</td>
    </tr>
    <tr>
        <td rowspan="2">整型</td>
        <td>float</td>
        <td>0.0</td>
        <td>4</td>
    </tr>
    <tr>
        <td>double</td>
        <td>0.0</td>
        <td>8</td>
    </tr>
    <tr>
        <td>字符型</td>
        <td>char</td>
        <td>\u0000</td>
        <td>2</td>
    </tr>
    <tr>
        <td>布尔型</td>
        <td>boolean</td>
        <td>false</td>
        <td>1</td>
    </tr>
    <tr>
        <td rowspan="3">引用数据类型</td>
        <td>数组</td>
        <td></td>
        <td>NULL</td>
        <td></td>
    </tr>
    <tr>
        <td>类</td>
        <td></td>
        <td>NULL</td>
        <td></td>
    </tr>
    <tr>
        <td>接口</td>
        <td></td>
        <td>NULL</td>
        <td></td>
    </tr>
</table>

- 数据类型能够帮助计算机为程序正确分配存储空间
- 使用==进行判断时，返回值为布尔型
  - **基本数据类型**比较**数据值**是否相同
  - **引用数据类型**比较**地址值**是否相同




## 加载顺序



## 关键字

###    `static`  <span id="static"> </span> 

- 静态变量 与 静态方法

  -  `static` 修饰的变量，称为静态变量，在整个程序运行过程中，静态变量随类的加载而被创建，且仅在内存中占有一份空间。它不属于任何对象，仅属于它所在的类，即访问静态变量不需要先实例化对象，可以直接访问

    ```java
    public class Demo {
    	static int number = 0;
    }
    ```

    - 访问：在其他类中 `Demo.number` 
      - 因为静态变量不属于对象，所以不能用 `this` 关键字访问静态变量

  -  `static` 修饰的方法，称为静态方法，
  - 如果我们不希望通过对象去访问变量或者方法，那么就使用静态变量和静态方法吧。但这并不意味着被 `static` 修饰的变量和方法就不能通过对象去访问

-  `static {}` 代码块在类加载时执行，且只执行一次

  ```java
  static {
  	Class.forName("org.mairadb.jdbc.Driver");
  }
  ```

  - 在JDBC中，我们常常把注册驱动、取得连接等步骤 封装 到工具类中，而注册驱动这一步在工具类运行过程中执行一次就可以了，没有必要每次都执行，就可以将其写在静态代码块中



###   `private` 

- 可修饰成员（成员变量和成员方法），用于 [封装](#fengzhuang) 
- 保护成员不被其他类使用，即被 `private` 修饰的成员只在本类中能被访问，被 `private` 修饰的构造方法不能被实例化
- 如需要被其它类使用，则
  -  `get变量名()` <span id="getbm"> </span>，用于**获取**成员变量的**值**，方法用 `public` 修饰
  -  `set变量名(参数)` <span id="setbm"> </span>，用于**设置**成员变量的**值**，方法用 `public` 修饰

``` java
import java.util.Random;

public class student {
    private int age = createRandomNum();
    public int createRandomNum() {
        Random r = new Random();
        return r.nextInt(101);	//返回一个[0, 101)的随机数
    }
    public int getAge() {
        return age;
    }
}
```

```java
public class showDetails {
	student person = new student();
	System.out.println(person.getAge());
}
```



###   `this` 

- 用于访问成员变量或方法（成员方法、构造方法）

  eg.

  ```java
  public class student {
  	String name;
      
      //都取名为name是为了提高程序可读性
      public void setname(name) {
          this.name = name;
      }
  }
  ```

  - this等同于变量名


###   `extends` 

- 在[继承](#jicheng)中使用，让子类继承父类的属性和方法

  `public class child_class extends parent_class`

- 子类中有调用的属性或方法，则不会调用父类的；子类的方法调用的属性，现在方法内找，再在当前类的成员变量中找，最后才到父类中找

###   `super` 

- 用于访问父类中的成员变量或方法（成员方法、构造方法）





## 内存

#### 栈内存<span id="zhan"> </span>

存储局部变量，即定义在方法中的变量，用完就消失。

- 没有默认值，需要先定义和赋值后，才能使用

#### 堆内存<span id="dui"> </span>

存储new出的东西（实体、对象），这些东西都有地址值（都开辟了新的内存空间），用完后会在垃圾回收器空闲时回收。

- 都有默认值




## 生成随机数

#### Random

```java
import java.util.Random;
Random r = new Random();
int rn = r.nextInt(X);	//生成[0, X)的随机数
```



## 数组

### 数据类型 数组名[]

`int a[length];`

### 数据类型[] 数组名

`int[] a;`

#### 动态规划

`int[] a = new int[length];`

- 初始化时的默认值，见[类型](#leixing)


#### 静态规划

`int[] a = (值1, 值2, ...); `



## 方法（method）

将具有独立功能的代码块组织成一个整体，使其具有特殊功能的代码集，我认为与C中的函数极其相似。



#### 方法重载

##### 定义

在同一个类中定义多个方法，若满足以下条件，则其相互之间构成重载

- 多个方法在同一个类中
- 这些方法具有相同的方法名
- 这些方法的参数不相同（类型不同、数量不同）



##### 作用

- 提高程序健壮性
  - 例如，编写一个计算器时，可以用方法重载来解决用户输入的数据与方法返回值类型不同的问题



##### 调用

需要强制类型转换

```java
```



## 变量

| 区别         | 全局变量     | 局部变量                    | 成员变量                   |
| ------------ | ------------ | --------------------------- | -------------------------- |
| 作用域       |              |                             |                            |
| 内存中的位置 |              | 栈内存                      | 堆内存                     |
| 生命周期     | 与程序共存亡 | 与方法共存亡                | 与对象共存亡               |
| 默认值       |              | 无默认值，见[栈内存](#zhan) | 有默认值，见[堆内存](#dui) |



## 类和对象

#### 类（class）

现实生活中具有共同属性和行为的事物的抽象

- 类是对象的数据类型
- 类是具有相同属性和方法的一组对象的集合
  - 属性：对象的特征
  - 方法：对象的行为

##### 定义 

```java
//类
public class pet {
    //成员变量
	String name;
    int age;
    
    //成员方法
    public void eat() {
        /* 代码块 */
    }
}
```





#### 对象（object）

##### 定义

`class_name object_name = new class_name();`



##### 使用

成员变量

`object_name.member_name;`

成员方法

`object_name.member_name();`



#### 构造方法

- 一个类中可以提供多种构造方法，在调用时会根据传递参数的类型、顺序选择对应的构造方法（构造方法的重载）
- 程序员没定义构造方法时，系统会给出默认的无参构造方法，当定义了构造方法后，则不再提供

```java
//若定义时括号内没有要求参数，则称无参构造方法
public calss_name(参1, 参2, ...) {
	/* 代码块 */
}
```



#### 标准类

- 使用private修饰成员变量
- 定义至少一个含参构造方法和一个无参构造方法
- 提供每个成员变量对应的[get方法](#getbm)和[set方法](#setbm)，方便其他类调用和修改

```java
public class student {
//    成员变量
    private String name;
    private int age;

//    构造方法
    public student() {}
    public student(String name, int age) {
        this.name = name;
        this.age = age;
    }

//    set方法
    public void setName(String name) {
        this.name = name;
    }
    public void setAge(int age) {
        this.age = age;
    }

//    get方法
    public String getName() {
        return name;
    }
    public int getAge() {
        return age;
    }

//    show方法
    public void show() {
        System.out.println(name + "," + age);
    }
}
```

```java
public class studentDemo {
    public static void main(String[] args) {
        //先创建空对象，再赋值
        student s1 = new student();
        s1.setName("Eason Chan");
        s1.setAge(30);
        s1.show();
        
        //在创建对象时直接赋值
        student s2 = new student("Hubert Wu", 27);
        s2.show();
    }
}
```



## API

有对应API文档



## 常用方法（见API文档）

#### String

##### 查询字符

###### 是否包含某个字符

`object_name.contains("目标字符");`

###### 从前往后查

`object_name.indexOf("目标字符");`

###### 从后往前查

`object_name.lastIndexOf("目标字符");`

##### 比较字符串内容

`object_name.equals()	//参数为要比较的字符串，返回值为布尔型`



#### StringBuilder

这是一个可变字符串对象，相较String而言，该对象仅需开辟一次内存空间，此后都在此空间上对字符串进行修改

##### 创建

- 构造方法

  `public StringBuilder();	//创建一个空的StringBulider对象，不含任何值`

  `public StringBulider(String str);	//根据字符串str的内容创建一个可变字符串对象`



##### 添加

`public StringBuilder append()	//任意类型的数据都可作为参数，添加数据并返回对象本身`

```java
//什么是返回对象本身
object_name.append("java");
object_name.append("java");

//等价于

object_name.append("java").append("java");	//链式编程

//最终输出结果为javajava
```



##### 反转

`public StringBuilder reverse();`

`object_name.reverse();`



#### String<=>StringBuilder

##### StringBuilder => String

- toString()方法

  ```java
  StringBuilder str = new StringBuilder();
  str.append("user");
  String s = str.toString();
  ```



##### String => StringBuilder

- 构造方法

  ````java
  String s = "user";
  StringBuilder str = new StringBuilder(s);
  ````

- append

  ```java
  String s = "user";
  StringBuilder str = new StringBuilder();
  str.append(s);
  ```

  


## 集合类

#### ArrayList<type>

- type常见为String

- 可调整大小的数组实现
- E是一种特殊的数据类型，泛型

##### 创建

`public ArrayList();	//创建一个空的集合对象`

`ArrayList<type> object_name = new ArrayList();`

`ArrayList<type> object_name = new ArrayList<type>();`

##### 添加

`public boolean add(E, e);	//将e添加到E的末尾，返回值为布尔型（表示是否添加成功）`

`public void add(int index, E element);	//将元素添加到index位置上`

##### 删除

`public boolean remove(Object o);	//删除指定元素，返回值为布尔型（表示是否删除成功）`

`public type remove(int index);	//删除指定索引处的元素，返回值为被删除的元素`

##### 修改

`public type set(int index, type element);	//修改指定索引处的元素，返回值为被修改的元素`

##### 查询

`public type get(int index);	//返回指定索引处的元素`

`public int size();	//返回集合中的元素个数`



## 封装

### 工具类（utils）

工具类中的构造方法都是私有的、静态的，不需要实例化对象，直接使用类名进行调用





## 继承

- 一个子类只能继承一个父类

#### 访问特点

##### 构造方法

- 子类中所有的构造方法默认都会访问父类中的无参构造方法

  - 子类初始化前，必须先完成父类初始化（否则可能会出现子类初始化失败的问题）

  - 每个子类构造方法的第一条语句默认是super()

    ```java
    public child_name() {
        super();	//此句无参数，调用父类中的无参构造方法，虽然编程时我们没有写，但它默认就是存在的
        /* 代码块 */
    }
    ```

    - 因此，我们应该在父类中**给出一个无参构造方法**或者显式使用super关键字去调用父类的带参构造方法（推荐前者）

##### 成员方法

- 先在子类中找，再到父类中找



#### 方法重写

子类中出现了和父类中一模一样的方法声明

- 在重写方法时，加上@override注释，编译器就会检查重写的方法声明是否有误

- 当父类的方法被private修饰时，不能重写，即子类不能继承父类的私有方法



###### 

