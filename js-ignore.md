## 温js权威指南

#### 一、类型、值、变量

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
1 + {} // => "1[Object Object]" 先将对象转换为字符串
true + true // => 2 先布尔值转换为数值类型
2 + null // => 2 先将null转换后为0
2 + undefined // => NaN 先将undefined转换为NaN
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

1.7 .2 对象转换为原始值

- 对象到布尔值：所有对象(包括包装对象)都将转换为true

- 对象到字符串和数字：所有对象都会继承两个转换方法，

  toString()--作用是反映这个对象的字符串

  ```javascript
  ({x: 1,y: 2}).toString() // => "{object object}"
  [1, 2, 3].toString() // => "1,2,3"
  (function(x){ f(x);}).toString() // => "function(x){ f(x);}"
  /\d+/g.toString() // => "/\d+/g" 
  new	Date(2010,0,1).toString() // => "Fri Jan 01 2010 00:00:00 GMT+0800 (中国标准时间)"
  ```

  valueOf()--如果存在任意原始值，就默认将对象装换为表示他的原始值，如果对象是复合值，将返回对象本身，然而，对于日期类型，将返回它的一个内部表示：1970年1月1日以来的毫秒数。

  ```javascript
  new String('abc').valueOf() // => "abc" 对象装换为表示他的原始值
  ({x: 1,y: 2}).valueOf() // => "{x: 1,y: 2}"
  [1, 2, 3].valueOf() // => "[1, 2, 3]"
  (function(x){ f(x);}).valueOf() // => "function(x){ f(x);}"
  /\d+/g.valueOf() // => "/\d+/g"
  new	Date(2010,0,1).valueOf() // => "1262275200000"
  ```

  > 对象到字符串转换步骤：先调用toString()，如果他返回一个原始值，JavaScript再将这个值转换为字符串返回
  >
  > 如果对象没有toString()方法，**或者这个方法并不返回一个原始值**，JavaScript会调用valueOf()，如果返回值是原始值，如果他返回一个原始值，JavaScript再将这个值转换为字符串返回
  >
  > 如果对象没有以上两个方法，则会抛出异常

  对象转换到数字：跟对象转换到字符串相同，只是会先尝试调用valueOf()

  > 综上：**对于所有非日期类型，对象到原始值的转换基本上是对象到数字的转换(首先调用valueOf())，原因是绝大部分对象都不是包装类型对象，所以调用toString()方法时不会返回原始值，对于日期类型会使用对象到字符串的转换规则**

  对象和“+”、“-”、“==”、“>” 的运算中，会先做对象到原始值的转换，除了日期类型：所有对象都会先尝试调用valueOf()，再调用toString()，不管得到的原始值是否可用，都不再进一步转换为字符串或数字，对于日期类型：

  ```javascript
  var now = new Date(); // 创建一个日期对象
  typeof (now + 1) // string "+"将日期转换为字符串
  typeof (now - 1) // number "-"使用对象到数字的转换
  now == now.toString(); // true 隐式和显式的字符串转换
  now > （now - 1）// true ">"将日期转换为数字
  ```


#### 二、表达式和运算符

###### 2.1 原始表达式

> 原始表达式是表达式的最小单位--它们不再包含其它表达式，包括常量(直接量)、关键字和变量都属于原始表达式

###### 2.2 属性访问表达式

> expression .identifier：适合访问的属性名为合法的标识符
>
> expression [identifier]：会先计算方括号内的表达式的值并将其装换为字符串，如果属性名称是一个保留字或者包含空格和标点符号，或是一个数字(对于数组)，必须使用方括号的写法

###### 2.3 算术表达式

> 注意a++ 和++a的区别
>
> ++x 会先将x转换为数字再加1 如x = "1" 则结果为2
>
> 5/2 结果为2.5而不是2
>
> 求余运算符(%)也适用于浮点数，如6.5%2.1 结果为0.2

2.3.1 位运算符 ==》用时查阅

###### 2.4 关系表达式

2.4.1 相等和不相等运算符var i= 2;
data = [1, 2, 3, 4, 5, 6]
data[i++] = data[i++] * 2

对于“===”

> 如果两个值都是null 或undefined 则这两个值不相等
>
> 通过x !== x来判断x是否为NaN
>
> 0和-0相等

对于“<”

> "11" < "3" 结果为true 两个字符串的比较 如果为字母是区分大小写的 所有大写字母ASCII值都“小于”小写字母ASCII值
>
> "11" < 3 结果为false 先将"11" 转换为 11
>
> "one" < 3 结果为NaN 先将"one" 转换为NaN

对于逻辑非“!”

> !(p && q) === !p || !q
>
> !(p || q) === !p && !q

2.4.2 ”in“运算符

```javascript
var point = {x: 1, y: 2};
"x" in point // => true 
"toString" in point // => true 对象继承了toString()方法

var data = [1, 2, 3];
"0" in data // => true 数组包含元素"0"
1 in data // => true 数字转换为字符串
3 in data // => false 没有索引为3的元素
```

2.4.3 ”instanceof“运算符

> a instanceof b 希望a为一个对象，希望b为标识对象的类(初始化它们的构造函数来定义)，如果a是b(或者b的“父类”)的实例则返回true，否则返回false

2.4.3 带操作的赋值运算符

```javascript
var i= 2;
data = [1, 2, 3, 4, 5, 6]
data[i++] *= 2
console.log(data);
// => [1, 2, 6, 4, 5, 6]
var n= 2;
data = [1, 2, 3, 4, 5, 6]
data[n++] = data[n++] * 2
// => [1, 2, 8, 4, 5, 6]
```

2.4.4 eval()

> 只有一个参数，如果此参数不是字符串，则直接返回这个参数，如果是字符串，则会把字符串当成js代码进行编译

2.4.5 typeof运算符

> 可带括号typeof(x)

| x          | typeof x                                 |
| ---------- | ---------------------------------------- |
| undefined  | undefined                                |
| null       | object                                   |
| true或false | boolean                                  |
| 任意数字或NaN   | number                                   |
| 任意字符串      | string                                   |
| 任意函数       | function                                 |
| 任意内置对象     | object                                   |
| 任意宿主对象     | 由编译器各自实现的字符串，但不是“undefined”, "boolean", "number", "string" |

> 内置对象：如数组、函数、日期和正则表达式等
> 宿主对象：有宿主提供的对象，在浏览器中window对象以及其下边所有的子对象(如bom、dom等等)，在node中是globla及其子对象，也包含自定义的类对象。【何为“宿主对象”？  在web中，ECMAScript中的“宿主”当然就是我们网页的运行环境，即“操作系统”和“浏览器”。所有非本地对象都是宿主对象（host object），即由 ECMAScript 实现的宿主环境提供的对象。】
> 全局对象：一般全局对象会有两个，一个是ecma提供的Global对象，一个是宿主提供。如在浏览器中是window、在nodejs中是global。【所以啊，在浏览器中全局对象是Global+window】
> 通常情况下ecma提供的Global对象对是不存在的，没有具体的对象，

2.4.5 delete运算符

> 不能操作var语句声明的变量、function语句定义的函数和函数的参数

```javascript
//操作数组时：
var a = [1, 2, 3];
delete a[2]
2 in a  // => false // => 3 
a.length() // => 3 并不改变数组长度 只是将数组“掏了个洞”
```

#### 三、语句

###### 3.1 条件语句

3.1.1 switch

> 在函数中使用switch，break可用return代替，case后面避免使用表达式，case匹配时执行的是“===”
>
> continue可代替break终止本轮循环并开始下一次循环

###### 3.2 循环语句

3.2.1 for语句

> for循环中那三个表达式任意一个都可以不写，但两个分号必须写，如果不写“test”表达式则会是一个死循环，
>
> 如for(;;), for(var i = 0;; i++)

3.2.1 for/in语句

```javascript
//可将o中属性复制到数组中
var o = {x: 1, y: 2, z: 3};
var a = [], i = 0;
for(a[i++] in o) //此处为a[i++] 而不能为a[i],否则i不会自增导致数组中只有一个元素
```

3.2 标签语句

> break和continue是javascript中唯一可以使用语句标签的语句
>
> break labelname;

3.3 "use strict"

> 严格模式中禁止使用with语句
>
> 所有变量需先声明
>
> 调用的函数(不是方法)中的this值是undefined
>
> 通过call()或apply()来调用函数时，其中this值是通过call()或apply()传入的第一个参数(在非严格模式中，null和undefined值被全局对象和转换为对象的非对象值所代替)
>
> 给只读属性赋值或给不可扩展的对象增加新成员会抛出类型异常错误(在非严格模式中这些操作会失败而不是报错)

#### 四、对象

理解

> JsvaScript中的对象可以从一个称为原型的对象继承属性，对象的方法通常是继承的属性，这种“原型式继承”是JsvaScript的核心特征，”除了字符串、数字、true、false、null、undefined之外，JsvaScript中的值都是对象。“

对象的三大特性

> 对象的原型(prototype) 指向一个继承自它的原型对象的对象，检测一个对象是否是另一个对象的原型可用p.isPropertyOf(o)

> 对象的类(class) 是一个标示对象类型的字符串

```javascript
//判断一个对象的类
function calssof(o) {
  if(o === null) return NUll;
  if(o === undefined) return Undefined;
  return Object.prototype.toString.call(o).slice(8, -1);
}
console.log(calssof(1)); // => Number           
console.log(calssof("")); // => String
console.log(calssof(false)); // => Boolean
console.log(calssof({})); // => Object
console.log(calssof([])); // => Array
console.log(calssof(/./)); // => RegExp
console.log(calssof(window)); // => Window(客户端宿主对象)
function f() {}
console.log(calssof(new of())); // => Objec
```

> 对象的扩展标记(extensible flag) 用以表示是否可以给对象添加新属性，在es5中指明了是否可以向该对象添加新属性，所有内置对象和自定义对象都是显示可扩展的，宿主对象的可扩展性由JavaScript引擎决定。
> es5中Object.esExtensible()判断对象是否可扩展，还可用Object.seal()、Object.preventExtensions()、Object.freeze()设置对象

###### 4.1 创建对象

4.1.1 Object.create()函数

4.1.2 对象直接量

```javascript
var empty = {};
var point = {x: 0,y: 0, "for": "all audiences"};
```

4.1.3 new

```javascript
var o = new Object();
var a = new Array();
```

###### 4.2 原型

> 每一个JsvaScript对象(null除外)都和另一个对象相关联，此处”另一个对象“就是原型，每一个对象都从原型继承属性。没有原型的对象为数不多，Object.prototype就是其中之一，它不继承任何属性。

```javascript
var a = new Array(); //a 的原型即为 Array.prototype 而Array.prototype对象的属性又继承自Object.prototype，故a对的属性同时继承自Array.prototype和Array.prototype---原型链接
//绝对空对象
var o = new Object(null); //o不继承任何属性方法，如toString()
//普通空对象{}
var o = new Object(Object.prototype); //同{}和new Object()
```

###### 4.3 继承

4.3.1 自定义inherit()函数

```javascript
//返回一个继承自原型对象p的新对象
function inherit(p){
    if(typeof p !== 'object' && typeof p !== 'function') return;
    if(p == null) throw TypeError();
    function f() {}; //定义一个空构造函数
    f.prototype = p; //将其原型对象设为p
    return new f(); //使用构造函数f()创建p的继承对象
}
```

> 对象属性的赋值或修改操作不会涉及到原型链，只有查询对象属性的时候才会涉及到

###### 4.4  属性访问错误

```javascript
Object.prototype = 0 //赋值失败 但没报错 Object.prototype没有被修改(非严格模式下)
```

###### 4.5 delete

```javascript
//当delete删除成功或没有任何副作用时返回true
o = {x: 1}
delete o.x; // true
delete o.x; // true 尽管o.x已经不存在
delete o.toString(); // true 什么也不做
delete 1 // true 无意义 

//delete不能删除那些可配置性为false的属性(可以删除不可扩展对象的可配置属性)
delete Object.prototype //不能删除 属性是不可配置的
var x = 1;
delete this.x; //不能删除这个属性
function f() {}
delete this.f; //不能删除全局函数

this.x = 1;
delete x; //非严格模式下可删除 严格模式下会报语法错误
delete this.x; //严格模式下可删除
```

###### 4.6 检测属性

> 判断某个属性是否存在于对象中，可用in、hasOwnPreperty()、propertyIsEnumerable()或属性查询，hasOwnPreperty()用来检测给定对象的自有属性，对于继承属性会返回false，propertyIsEnumerable()是hasOwnPreperty()的增强版，这个属性需同时满足是自有属性且可枚举性为true
>
> 当检测属性值为undefined对象的属性时只能用in，如：{x: undefined}只有 "x" in {x: undefined}  =》 true
>
> Object.keys()返回的是对象中的可枚举自有属性名称数组，Object.getOwnPropertyNames()返回的是全部自有属性名称数组

###### 4.7 存取器属性getter和setter

```javascript
var p = {
    //x和y是普通的可读写数据属性
    x: 1.0,
    y: 1.0,
    //r是可读写的存取器属性，它带有getter和setter。
    //函数体结束后不要忘记带上逗号
    get r() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    },
    set r(newvalue) { //可利用p.r = ？给r设置值
        var oldvalue = Math.sqrt(this.x * this.x + this.y * this.y);
        var ratio = newvalue / oldvalue;
        this.x *= ratio;
        this.y *= ratio;
    },
    //theta是只读存取器属性，它只有getter方法，而不能利用p.theta = ？给theta设置值
    get theta() {
        return Math.atan2(this.y, this.x);
    }
};
```

###### 4.7 属性的特性

> 它的值(value)、可写性(writable)、可枚举性(enumerable)、可配置性(configurable)
>
> 存取器属性getter和setter不具有值特性和可写性(由是否存在setter方法决定)，因此其四个属性是读取(get)、写入(set)、可枚举性、可配置性
>
> es5中定义了一个名为“属性描述符”的对象，这个对象的属性和它们所描述的属性特性是同名的，因此描述符对象的属性有value、writable、enumerable、configurable, 存取器属性描述符用get、set代替value和writable
>
> 通过调用Object.getOwnPropertyDescriptor()可获得某个对象特定属性(自有属性)的属性描述符

```javascript
Object.getOwnPropertyDescriptor({x: 1}, "x") // => 返回：{value: 1、writable:true、enumerable:true、configurable:true} 对于继承属性和不存在的属性则返回undefined
```

> 要想获得继承属性的特性，需遍历原型链Object.getPrototypeOf()



> 要想设置属性的特性，需调用Object.defineProperty()，此方法要么修改已有属性要么新建自有属性，但不能修改继承属性，要同时修改或创建某个对象中的多个属性用Object.defineProperties()，***还有要熟悉规则***

```javascript
var o = {};
//添加一个不可枚举属性x，赋值为1，不一定必须必包含所有四个特性，默认对应的特性值是false或undefined
Object.defineProperty(o,"x",{value: 1,writable: true,enumerable: false,configurable: true})
o.x // => 1 //属性"x"存在
Object.keys(o) // => [] //却不可枚举

Object.defineProperties({},{
  x: {value: 1,writable: true,enumerable: false,configurable: true},
  y: {value: 1,writable: true,enumerable: false},
  r: {
      get: function () { return Math.sprt(this.x * this.x + this.y * this.y) },
      enumerable: false,
      configurable: true
  },
})

//复制属性的特性,实现extend()
Object.defineProperty(Object.prototype,
                     "extend",
                     {
                     writable: true,
                     enumerable: false,
                     configurable: true,
                     value: function(o) {
                       	//Object.getOwnPropertyNames():返回对象可枚举和不可枚举自有属性的集合
  						var names = Object.getOwnPropertyNames(o);
						for(var i = 0; i < names.length; i++){
							if(names[i] in this) continue;
                          	  var desc = Object.getOwnPropertyDescriptor(o, names[i]);
							Object.defineProperty(this, names[i], desc);
                          }
				    }
				    });
```

#### 五、函数

> 在JavaScript中，函数即对象

###### 5.1 函数的调用

5.1.1 有4种方式调用JavaScript函数：

> 作为函数
>
> 作为方法
>
> 作为构造函数
>
> 通过它们的call()和apply()方法间接调用

5.1.2 函数和方法

> 函数作为对象的属性时称为方法，方法和函数的一个重要区别就是调用上下文

5.1.3  方法调用

> 面向对象编程的核心就是方法和this关键字

```javascript
var o = {
  m: function () {
    var self = this; // 将this的值保存在一个变量中
    console.log(this === o); // true this就是这个对象
    f();
    function f() {
      console.log(this === o); // false this指向全局对象(非严格模式)或undefined(es5严格模式)
      console.log(self === o);// true self指向外部函数的this
    }
  }
}
o.m();

//利用方法的返回值是一个对象时，这个对象还可以再调用自己的方法，这一特性可以形成方法的链式调用
shape.setX(100).setX(200).setY(300).setSize(400).setOutline("red").setFill("blue").draw();
```

5.1.4 构造函数的调用

> 如果函数或方法之前带有关键字new，它就构成构造函数调用

