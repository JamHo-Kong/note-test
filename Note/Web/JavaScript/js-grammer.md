# 概念

1. JS中的 `''`与 `""` 是一样的，不像C++中一个用于字符，一个用于字符串
1. 基本包装类型：把简单数据类型包装成复杂数据类型，使得简单数据类型也拥有属性和方法（String、Number、Boolean）
1. 字符串不可变性：当我们声明了一个字符串后，内存中会开辟一段内存来存储字符串内容，此时我们改变字符串的值，新值会被存储在一个新的内存空间中，之前的开辟的空间不会被回收，所以我们要谨慎的改变字符串的值，以提高运行速度。



# 预解析

将var、function声明提到当前作用域的最前面

- var提升：将变量声明提升到当前作用域的最前面，但赋值等操作不会被提升
- function提升：将函数声明提升到当前作用域的最前面，但不会调用



# 对象

一个具体的事物

- 属性：事物的特征
- 方法：事物的行为



## 自定义对象

### 创建

1. 使用字面量{}

   `var pet = {};	//创建了一个叫pet的空对象`

   ```javascript
   //创建了一个叫siamese的对象
   var siamese = {
       //siamese的属性
   	name: "Siamese",
   	age: 1,
       
       //siamese的方法
   	sayHi: function() {
   		/* 代码块 */
   	}
   }
   ```

   

2. new object

   ```javascript
   var siamese = new object();
   //siamese的属性
   siamese.name = "Siamese";
   siamese.age = 1;
   
   //siamese的方法
   siamese.sayHi = function() {
       /* 代码块 */
   }
   ```

   

3. 使用构造函数（将公共部分抽象出来，封装到一个函数中，不需要return即可返回，其返回值为对象）

   ```javascript
   function pet(name, age) {
   	//siamese的属性
   	this.name = name;
   	this.age = age;
       
   	//siamese的方法
   	this.sayHi = function() {
   		/* 代码块 */
   	}
   }
   new siamese = pet("Siamese", 1);	//实例化了一个叫siamese的对象，与C中调用函数一致，需要传递实参
   ```

- 构造函数创建对象的过程
  - new在内存中创建空对象
  - this指向空对象
  - 执行构造函数中的代码
  - 返回对象



### 调用

调用属性<span id="shuxingdiaoyong"> </span>

`siamese.name;`

`siamese[name];`

调用方法<span id="fangfadiaoyong"> </span>

`siamese.sayHi();`



### 区分

1. 变量&属性

   - 同：都用来存放数据

   - 异

     | 变量                 | 属性                           |
     | -------------------- | ------------------------------ |
     | 单独声明    var age; | 直接写    age: 1;              |
     | 引用直接写变量名     | 见[调用属性](#shuxingdiaoyong) |

2. 函数&方法

   - 都能实现某功能

   - 异

     | 函数            | 方法                          |
     | --------------- | ----------------------------- |
     | 单独声明        | 直接写                        |
     | 调用    sayHi() | 见[调用方法](#fangfadiaoyong) |



## 遍历对象

for...in：可用于对象和数组的遍历，但不推荐用于数组遍历

```javascript
for (k in siamese) {
	console.log(k);				//输出属性名
	console.log(siamese[k]);	//输出属性值
}
```



## 内置对象

### Math

#### 圆周率

​		`Math.PI` 

#### 最大值

​		`Math.max()` 

#### 最小值

​		`Math.min()` 

#### 向下取整

​		`Math.floor()` 

#### 向上取整

​		`Math.ceil()` 

#### 四舍五入

​		`Math.round()` 

#### 绝对值

​		`Math.abs()` 

#### 随机数

​		`Math.random()	//生成的数的取值范围[0, 1)` 

eg.生成指定个不重复的随机数

```javascript
var x = 9; //变量x为预期生成的随机数的个数
var numArray = []; //声明存放随机数的数组
var temp; //声明用来临时存放随机数的变量
 
function createRandNum() {
    function randNum() {
        return Math.floor(Math.random() * x); //生成一个随机数，其取值范围为[0, x)
    }
    while (numArray.length < x) {
        temp = randNum();
        if (numArray.indexOf(temp) == -1) { //判断numArray里有没有和temp相等的数
            numArray[numArray.length] = temp;
        }
    }
}
createRandNum();
```



### Date

处理时间和日期，本身是一个构造函数，使用时需要new将其实例化

 `var date0 = new Date();` 

- 没有参数，返回系统当前时间

- 有参数

  - 数字型

     `var date1 = new Date(2022, 3, 30);` 

    - 数字型输出时，月份会比输入的月份大1个月

  - 字符串型

     `var date2 = new Date('2022-3-30 8:0:0');` 

格式化

#### 返回年

 `对象名.getFullYear();	//以下用date2表示对象名` 

#### 返回月

 `date2.getMonth()+1;` 

#### 返回日期

 `date2.getDate();` 

#### 返回周几

 `date2.getDay();` 

#### 返回时

 `date2.getHours();` 

#### 返回分

 `date2.getMinutes();` 

#### 返回秒

 `date2.getSeconds();` 

#### 时间戳（返回毫秒）

距离1970年1月1日的总毫秒数，有参数则为参数与1970年1月1日的总毫秒数，一般**用于倒计时的制作**

 `date2.valueOf();` 

 `date2.getTime();` 

 `var date2 = +new Date();` 

 `Date.now();	//Html5新特性，可能存在兼容性问题` 



### Array

#### 是否为数组

-  `Array.isArray()	//Html5新特性，可能存在兼容性问题` 
  - 将需要判断的作为参数放在括号内，返回值为布尔型

-  `对象名 instanceof 构造函数名` 

  - 并不是专门用来判断数组的，此处作为补充

  eg.

```javascript
function Car(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
}
const auto = new Car('Honda', 'Accord', 1998);

//auto是Car对象吗？预期输出结果为true
console.log(auto instanceof Car);

//auto是对象吗？预期输出结果为true
console.log(auto instanceof Object);
```

#### 增加数组元素

- 从尾部增加

   `array_name.push(元素1, 元素2, ...);	//返回值为新数组长度` 

- 从头部添加

   `array_name.unshift(元素1, 元素2, ...);	//返回值为新数组长度` 

#### 删除数组长度

- 从尾部删除

   `array_name.pop();	//删除最后一个元素，返回值为被删除元素` 

- 从头部删除

   `array_name.shift();	//删除第一个元素，返回值为被删除元素` 

#### 筛选数组

- 翻转数组

   `array_name.reverse();` 

- 内置冒泡排序

   `array_name.sort();	//只能对个位数进行排序` 

解决办法：

```javascript
//升序
array_name.sort(function(a, b) {
    return a-b;
})

//降序
array_name.sort(function(a, b) {
    return b-a;
})
```

- 获取指定数组元素索引（索引就是位置）

  - 可用于判断数组内是否有某元素（查重）

 **从前往后查** 

 `array_name.indexOf();	//查询参数在数组中的索引号，若不存在则返回-1` 

 `array_name.indexOf(5, 3);	//从第3个位置开始向后查询5在数组中的位置` 

 **从后往前查** 

 `array_name.lastIndexOf();	//查询参数在数组中的索引号，若不存在则返回-1` 

 `array_name.indexOf(5, 3);	//从第3个位置开始向前查询5在数组中的位置` 

- 将数组转化为字符串

   `array_name.toString();//输出用,隔开的字符串，如‘1,2,3’` 

   `array_name.join(分隔符);	//可以指定分隔符，输出的字符串将会用指定分隔符隔开数组元素` 



### String

- 获取指定字符在字符串中的索引

   **从前往后查** 

   `array_name.indexOf();	//查询参数在数组中的索引号，若不存在则返回-1` 

   `array_name.indexOf('a', 3);	//从第三个位置开始向后查询a在数组中的位置` 

   **从后往前查** 

   `array_name.lastIndexOf();	//查询参数在数组中的索引号，若不存在则返回-1` 

   `array_name.indexOf('a', 3);	//从第三个位置开始向前查询a在数组中的位置` 



# 函数

