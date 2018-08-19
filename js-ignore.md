## 重温js权威指南

#### 一. 类型、值、变量

###### 1.1 js数据类型分为原始类型和对象类型

1.1.1 原始类型：包含数字、字符串、布尔值(严格来说还包括null和undefined，只是他们的类型成员当中只有自己)，除此之外就是对象了。

1.1.2 对象类型：包含普通对象(key: value形式)、数组、函数

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

回到js中函数的理解，我们可以对比java中的来看

