---
title: 我的Java学习笔记（2）对象和类
date: 2019-05-18 10:14:00
tags:
	- Java
categories: notes
icon: /images/notes.svg
---

# Java中的对象

**对象**：对象是类的一个实例，有状态和行为。软件对象的状态就是属性，行为通过方法体现。

# Java中的类

**类**：类是一个模板，它描述一类对象的行为和状态。

## 类中的变量

一个类可以包含以下类型变量：

- **局部变量**：**在方法、构造方法或者语句块中定义的变量**被称为局部变量。**变量声明和初始化都是在方法中，方法结束后，变量就会自动销毁。**
- **成员变量**：**成员变量是定义在类中，方法体之外的变量**。这种变量在创建对象的时候实例化。成员变量可以被类中方法、构造方法和特定类的语句块访问。
- **类变量**：类变量也**声明在类中，方法体之外**，但**必须声明为static类型**。

## 类中的构造函数

**每个类都有构造方法。如果没有显式地为类定义构造方法，Java编译器将会为该类提供一个默认构造方法。**

**在创建一个对象的时候，至少要调用一个构造方法。构造方法的名称必须与类同名，一个类可以有多个构造方法。**

# 创建对象

对象是根据类创建的。在Java中，使用关键字new来创建一个新的对象。创建对象需要以下三步：

- **声明**：声明一个对象，包括对象名称和对象类型。
- **实例化**：使用关键字new来创建一个对象。
- **初始化**：使用new创建对象时，会调用构造方法初始化对象。

## 源文件声明规则

- 一个源文件中**只能有一个public类。**
- **一个源文件可以有多个非public类。**
- **源文件的名称应该和public类的类名保持一致。**
- **如果一个类定义在某个包中，那么package语句应该在源文件的首行。**
- 如果源文件包含**import语句，那么应该放在package语句和类定义之间**。如果没有package语句，那么import语句应该在源文件中最前面。
- **import语句和package语句对源文件中定义的所有类都有效。在同一源文件中，不能给不同的类不同的包声明。**

### Java包

包主要用来**对类和接口进行分类**。当开发Java程序时，可能编写成百上千的类，因此很有必要对类和接口进行分类。

### Import语句

在Java中，如果给出一个完整的限定名，包括包名、类名，那么Java编译器就可以很容易地**定位到源代码或者类**。Import语句就是用来提供一个合理的路径，使得编译器可以找到某个类。

**程序都是从main方法开始执行。为了能运行这个程序，必须包含main方法并且创建一个实例对象。**

# 实例

```java
import java.io.*; //import将会命令编译器载入java_installation/java/io路径下的所有类
public class Cat {
    /*成员变量*/
    String catColor;
    int catAge;
    /*构造函数*/
    public Cat(String name){
      /*name为局部变量*/
      System.out.println("The name of cat is "+name);
    }
    /*方法*/
    public void setColor(String color){
      /*color为局部变量*/
      catColor = color;
    }
    public String getColor(){
      System.out.println("The color of cat is "+catColor);
      return catColor;
    }

    public void setAge(int age){
      /*age为局部变量*/
      catAge = age;
    }
    public int getAge(){
      System.out.println("The age of cat is "+catAge);
      return catAge;
    }
    /*程序都是从main方法开始执行。为了能运行这个程序，必须包含main方法并且创建一个实例对象*/
    public static void main(String[] args){
      /*实例化对象*/
      Cat myCat = new Cat("Tom");
      /*访问类中的方法*/
      myCat.setColor("black");
      myCat.getColor();
      myCat.setAge(1);
      myCat.getAge();
      /*myCat.catAge和myCat.Color为访问类中的变量*/
      System.out.println("Age:"+myCat.catAge);
      System.out.println("Color:"+myCat.catColor);
    }
}
```

编译执行，如图：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/JavaStudy/Java-2-2.png?raw=true)

**参考连接**：

[菜鸟教程-Java对象和类](https://www.runoob.com/java/java-object-classes.html)

[掘金-Java 基础思维导图，让 Java 不再难懂](https://juejin.im/entry/58e467a961ff4b006b2fb7f9)

---

写这篇Java笔记的原因主要是记一些我学习Java过程中的笔记，我想要写一个更适合我后期回顾的Java笔记。其中引用了不少菜鸟教程里的内容，如有侵权，请联系我，谢谢~