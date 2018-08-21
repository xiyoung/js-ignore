## 重温js权威指南

#### 一. 类型、值、变量

###### 1.1 js数据类型分为原始类型和对象类型

> 原始类型储存在栈内存中，对象类型储存在堆内存中(其访问地址存放在栈内存中)

1.1.1 原始类型：包含数字、字符串、布尔值(严格来说还包括null和undefined，只是他们的类型成员当中只有自己)，除此之外就是对象了。

1.1.2 对象类型：包含普通对象(key: value形式)、数组、函数。

1.1.2.1 函数：得先了解两个概念-类、类对象、构造函数，这里我们借用强类型语言java中的解释，类是一类具有相同属性和方法的对象的集合，所以类是对对象的抽象，而对象又是对客观事物的抽象。来举个例子，比如Person(人)是一个类，Person类中可以有70亿人，那么具体的某个人“Jack”就是一个客观实例(实例对象)，类似“Jack”这样的人我们都可以称之为对象。Java中定义类用class关键字，初始化一个实例对象用new关键字(构造函数)。

```java
public class Person {
　　 private String name; //属性：姓名
　　 private int height; //属性：身高
　　 private int weight; //属性：体重
　
　　 public Person() {}
　　 public Person(String name, int height, int weight) {//构造函数
　　 this.name = name;
　　 this.height = height;
　　 this.weight = weight;
　　 }
　　
　　 //... some methods...
　　
　　 public void doSth() { //行为(方法)：
　　 //... do something
　　 }
　　}

//新建一个实例对象"Jack"
Person Jack = new Person("Jack", 170, 65);
zhangsan.doSth(); //调用类中的方法(对象行为：对象自己发出行为)

```

js跟java一样也是一种面向对象的语言，也有类和对象的概念，然后js中的类可以看作是对象(Object)类型的子类型，后面我么讲函数的的时候会详细讲到。js中除了**数组(Array)**类和**函数(Function)**类之外，还定义了其他三种类：**日期(Date)；正则(RegExp)；错误(Error)**。

> js亦可分为拥有方法的类型和不能拥有方法的类型，除了undefined和null，其他类型均拥有自己的方法
>
> js亦可分为可变类型和不可变类型，除了对象和数组是可变类型，其他均为不可变类型

###### 1.2 数字

> js中能够表示的整数范围是-2^53~2^53

> js十六进制直接量的表示方法是以“0x”或“0X”为前缀，其后跟随十六进制数串的直接量。十六进制是0~9之间的数字和a(A)~f(F)之间的字母构成，a~f之间的字母对应的表示数字是10~15，如：
> 0xff // 15*16 + 15 = 255(十进制)

> es6严格模式下不支持八进制直接量

> 返回NaN(not a number)的情况: 零除以零；无穷大(Infinity)除以无穷大；给负数开方运算；算术运算符与不是数字或无法转换为数字的操作一起使用时

> NaN：它和任何值都不相等，包括自身。所以可用x != x来判断x是否为NaN。

###### 1.3 布尔值

> 任意javascript值都可以转换为布尔值，其中这些值将会转换为false: undefined, null, '', 0,-0, NaN，其他都会转换为true

###### 1.4 null和undefined

> typeof null返回object; typeof undefined返回undefined; 初始化变量或属性最好用null

###### 1.5全局对象

当javascript解释器启动时或web浏览器加载新页面时，它将创建一个新的全局对象(比如window对象)，并给他一组定义的初始属性：

- 全局属性：undefined、Infinity(无穷大)、NaN
- 全局函数：isNaN、parseInt()、eval()
- 构造函数：Date()、RegExp()、String()、Object()、Array()
- 全局对象：Math、Json

> 对于客户端JavaScript，Window对象定义了一些额外的全局属性

###### 1.6 包装对象

> 存取字符串、数字或布尔值的属性时创建的临时对象叫做包装对象

以JavaScript创建字符串(var s = 'hello world')为例：字符串既然不是对象，为什么会有属性和方法呢？

> 这是因为：只要引用了字符串的属性，JavaScript就会将字符串值通过调用new String(s)的方式转换为对象，这个对象继承了字符串的方法，并被用来处理属性的引用，一旦属性引用结束，这个新建的对象就会销毁(其实在实现上并不一定创建会销毁这个临时对象，只是过程看起来如此罢了)

```javascript
var s = "test";
s.len = 4;
var t = s.len; //t的值将会是undefined,因为程序执行到这的时候，第二行中通过字符串s创建的临时对象已经销毁，这说明在读取字符串、数字、和布尔值的属性值(或方法)到时候，表现得像对象一样。但是如果给其赋值，则会忽略此操作：修改只是发生在临时对象上。
```

```javascript
//关于包装对象和原始值直接的关系，“==”运算时视为相等，但“===”运算时则不等
var s = 'test',n = 1, b = true;
var S = new String(s)
var N = new Number(n)
var B = new Boolean(b)

console.log(typeof(s)) //string
console.log(typeof(S)) //object
console.log(typeof(b)) //boolean
console.log(typeof(B)) //object
console.log(typeof(n)) //number
console.log(typeof(N)) //object
```

> 原始值数值、字符串、布尔值、null和undefined中只有前三者有包装对象，后两者自身是其类型的唯一成员，所以没有包装对象。

###### 1.7 类型转换

先看一些例子：

```javascript
10 + " objects" // => "10 objects" 数字10转换为字符串
"7" * "4" // => 28 两个字符换均转换为数字
1 - 'x' // => NaN 字符串"x"无法转换为数字
//关于JavaScript的类型转换可查阅文档 其中值得注意的是undefined转换为数值类型为NaN，""、null、false转换为数值类型时为0，原始值转换为对象可用String()、Number()、Boolean()构造函数转换为各自的包装类型
undefined、null转换为对象会报错
```

1.7.1 显示类型转换

```javascript
Number("3") // => 3
String(false) // => "false"
Boolean([]) // => true
Object(3) // => new Number(3)
//注意
Object(null) // => {}
Object(undefined) // => {}
```

关于Number()、parseInt()、parseFloat()三个函数

- Number()转换字符串时，会基于十进制将其转换为整数或浮点数，并且不能出现非法尾随字符，否则转换结果为NaN(如Number('123dd'))

- parseInt()、parseFloat()是全局函数，不属于任何类的方法，parseInt()只解析整数，parseFloat()可解析整数和浮点数，如果字符串前缀是“0X”或“0x”，parseInt()将其解析为十六进制数，parseInt()、parseFloat()都会跳过任意数量的前导空格，并忽略后面的非法内容，如果第一个非空格字符是非法数字直接量，则返回NaN

  ```javascript
  parseInt("3 blind mice") // => 3
  parseFloat(" 3.14 meters") // => 3.14
  parseInt("-12.34") // => -12
  parseInt("0xFF") // => 255 识别十六进制
  parseInt("-0xFF") // => -255 识别十六进制
  parseFloat(" .14 meters") // => 0.14
  parseInt(".1") // => NaN 整数不能以.开始
  parseFloat("$3.14") // => NaN 数字不能以$开始
  
  //parseInt可接受第二个参数 这个参数指定数字转换的基数，取值范围是(2 ~ 36)
  parseInt("11", 2) // => 3 (1*2 + 1)
  parseInt("ff", 16) // => 255 (15*16 + 5)
  parseInt("zz", 36) // => 1295 (35*36 + 35)
  parseInt("077", 8) // => 63 (7*8 + 7)
  parseInt("077", 10) // => 77 (7*10 + 7)
  ```

1.7 .1 对象转换为原始值

