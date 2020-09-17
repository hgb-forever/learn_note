# 学习方法

CRUD 

快速上手需了解创建、查找、修改、删除

ocp原则：Open close Principles，对扩展开放，对修改关闭。即添加新功能的时候，尽量不修改代码

# scala中eq,==和equals的区别

- equals比较的是值是否相等
- eq比较的是 地址是否相等
- ==如果比较的对象是null，==调用的是eq方法，如果比较的对象不是null，==调用的是equals方法



# 花括号和圆括号的区别

有这么几条原则：

- 当调用的函数有两个及其以上的参数的时候，这时候你只能用小括号。
- 当调用的函数只有一个函数的时候，花括号和小括号都可以使用。但是还有区别的。
- 如果使用小括号，意味着你告诉编译器：它只接受单一的一行，因此，如果你意外地输入2行或更多，编译器就会报错。但对花括号来说则不然，它可以接受多行的输入。foreachRDD和foreachPartition就是例子。
- 在调用一个单一参数的函数的时候，如果参数是用case实现的偏函数，那么你只能使用花括号。

# 第一章概述

## scala简介

1. Scala是Scalable Language的简写，是一门多范式的编程语言。
2. Spark新一代内存级大数据计算框架，是大数据的重要内容。
3. 为了更好的学习Spark技术需要学习Scala。

## Scala与Java以及JVM的关系

Scala的创始者马丁·奥德斯基是jdk5.0和jdk8.0和JVM主流编译器的开发者，创造Scala语言目的是为了让写程序的工作变得高效，Scala是对Java的提升和扩展。

1. Scala支持部分Java语法。
2. 增加了新的类库
3. 对Java的类进行包装修改。

![image-20200424104535449](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200424104535449.png)

### 特点：

Scala是一门以JVM为运行环境并将面向对象编程和函数式编程的最佳特性结合在一起的静态型编程语言。

### 快速高效学习：

- 学习Scala特有的语法
- 区别Java和Scala
- 规范使用Scala

## Scala环境配置

下载安装包并配置系统环境变量即可

## 编辑工具IDEA

安装IDEA的Scala插件以支持Scala开发

## Scala开发入门

### scala的工作流程

.scala(源文件) 通过scalac 编译器编译成 .class(字节码文件) 通过 scala 运行到JVM 最后在客户机上输出结果。

也可通过scala命令一步编译运行得到结果，但是运行较慢，因为底层还是先进行编译再运行。

使用scala可以运行部分.java文件，因为scala的库比java大。

编译后的.scala会生成两个.class文件，Hello$.class和Hello.class，底层真正的类名是Hello$类型的一个静态对象MOUDLE$。

scala运行流程：

先从Hello的main开始运行，main里面调用了Hello$.MOUDLE$.main(),然后执行真正的代码内容

### scala开发注意事项

1. scala源文件以".scala"为扩展名
2. scala程序的执行入口是main
3. scala语言严格区分大小写
4. scala语句结尾不需要加分号，一行有多条语句除外

## Scala语言输出的三种模式

​	1.print("hgb"+"NB")      				   //变量相加

​	2.printf("%s","hgbNB")					//控制输出格式

​	3.var name:String ="hgb"				//变量索引

​	var action:String ="NB"		

​	print(s"$name+$action")

​	print(s"$name ${action+2}")      //${..}大括号里面可以进行运算

## 注释

- 单行注释
- 多行注释
- 文档注释

## 文档注释

在函数前加入/**/在里面写入注释符，找到文件目录，在cmd里输入scaladoc -d(指定目录)  d:/mydoc HelloScala 如果目录文件不存在则自动创建。

```java
  /**
    * @author
    * hgb
    * @example
    * 输入n1,n2输出n1+n2
    * @version 1.0
    * @return 和
    * @param n1
    * @param n2
    */
```

# 第二章 变量

[^注意]: 变量在声明时就要进行初始化！

## 基本语法

var name : String = "hgb"

var | val 变量名 [: 变量类型] = 变量值

- 声明变量时，基本类型可以省略，即编译器进行类型推断
- 在 scala 中，小数默认为 Double ,整数默认为 Int
- 类型确定后不能再更改类型
- var修饰变量，可更改变量值，val修饰变量不可再更改变量值，val底层编译添加了final属性

变量.isInstanceOf[变量类型]可判断变量是否为某个类型

## 数据类型

在scala中数据类型都是对象，即一个基本变量可以使用很多方法

在scala中如果一个变量的方法没有形参，可以省略（）

![image-20200420225703951](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200420225703951.png)

1) 在 scala 中有一个根类型 Any ,他是所有类的父类. 

2) scala 中一切皆为对象，分为两大类 AnyVal(值类型)， AnyRef(引用类型)， 他们都是 Any 子类. 

3) Null 类型是 scala 的特别类型，它只有一个值 null, 他是 bottom calss ,是 所有 AnyRef 类型的子类. 

4) Nothing 类型也是 bottom class ,他是所有类的子类，在开发中通常可以将 Nothing 类型的值返回
给任意变量或者函数， 这里抛出异常使用很多

5）在 scala 中，小数默认为 Double ,整数默认为 Int

6）在 scala 中仍然遵守，低精度的值，向高精度的值自动转换(implicit conversion) 隐式转换

| 数据类型 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| Byte     | 8位有符号补码整数。数值区间为 -128 到 127                    |
| Short    | 16位有符号补码整数。数值区间为 -32768 到 32767               |
| Int      | 32位有符号补码整数。数值区间为 -2147483648 到 2147483647     |
| Long     | 64位有符号补码整数。数值区间为 -9223372036854775808 到 9223372036854775807 |
| Float    | 32 位, IEEE 754标准的单精度浮点数                            |
| Double   | 64 位 IEEE 754标准的双精度浮点数                             |
| Char     | 16位无符号Unicode字符, 区间值为 U+0000 到 U+FFFF             |
| String   | 字符序列                                                     |
| Boolean  | true或false                                                  |
| Unit     | 表示无值，和其他语言中void等同。用作不返回任何结果的方法的结果类型。Unit只有一个实例值，写成()。 |
| Null     | null                                                         |
| Nothing  | Nothing类型在Scala的类层级的最低端；它是任何其他类型的子类型。 |
| Any      | Any是所有其他类的超类                                        |
| AnyRef   | AnyRef类是Scala里所有引用类(reference class)的基类           |

## 整数类型

1) Scala 各整数类型有固定的表数范围和字段长度，不受具体 OS 的影响，以保证 Scala 程序的可
移植性。
2) Scala 的整型 常量/字面量 默认为 Int 型，声明 Long 型 常量/字面量 须后加‘l’’或‘L’[反编译看]
3) Scala 程序中变量常声明为 Int 型，除非不足以表示大数，才使用 Long

## 浮点类型

Scala 的浮点类型可以表示一个小数，比如 123.4f，7.8 ，0.12 等等，默认为Double类型,声明Float需加上'f'或'F',Float在7位以内的小数精准。

## 字符类型

字符类型可以表示单个字符,字符类型是 Char， 16 位无符号 Unicode 字符(2 个字节), 区间值为U+0000 到 U+FFFF

注意：

var c2 : Char = 'a'+ 1  						//报错

var c3 : Char =98 + 1 						//报错

var c4 : Char = 99								//正确

原因：

1. 当把一个计算的结果赋值一个变量，则编译器会进行类型转换及判断（即会看范围+类型）
2. 当把一个字面量赋值一个变量，则编译器会进行范围的判定

## 布尔类型

布尔类型也叫 Boolean 类型，Booolean 类型数据只允许取值 **true** 和 **false**
boolean 类型占 1 个字节。
boolean 类型适于逻辑运算，一般用于程序流程控制[后面详解]：

## Unit 类型、Null 类型和 Nothing 类型

1) Null 类只有一个实例对象，null，类似于 Java 中的 null 引用。null 可以赋值给任意引用类型(AnyRef)，但是不能赋值给值类型(AnyVal: 比如 Int, Float, Char, Boolean, Long, Double, Byte, Short)
2) Unit 类型用来标识过程，也就是没有明确返回值的函数。由此可见，Unit 类似于 Java 里的 void。Unit 只有一个实例，()，这个实例也没有实质的意义
3) Nothing，可以作为没有正常返回值的方法的返回类型，非常直观的告诉你这个方法不会正常返回，而且由于 Nothing 是其他任意类型的子类，他还能跟要求返回值的方法兼容。

## 值类型转换

### 值类型的隐式转换

当 Scala 程序在进行赋值或者运算时，精度小的类型自动转换为精度大的数据类型，这个就是自动类型转换(隐式转换)。

![image-20200420231635742](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200420231635742.png)

自动类型转换细节说明
1) 有多种类型的数据混合运算时，系统首先自动将所有数据转换成容量最大的那种数据类型，然后再进行计算。 5.6 + 10 = 》double
2) 当我们把精度(容量)大 的数据类型赋值给精度(容量)小 的数据类型时，就会报错，反之就会进行自动类型转换。
3) (byte, short) 和 char 之间不会相互自动转换。
4) byte，short，char 他们三者可以计算，在计算时首先转换为 int 类型。
5) 自动提升原则： 表达式结果的类型自动提升为 操作数中最大的类型

## 高级隐式转换和隐式函数

## 强制类型转换

### 介绍

​	自动类型转换的逆过程，将容量大的数据类型转换为容量小的数据类型。使用时要加上强制转函数，但可能造成精度降低或溢出,格外要注意。

### 案例

```
java : int num = (int)2.5

scala : var num : Int = 2.7.toInt //对象
```

### 细节说明

1) 当进行数据的 从 大——>小，就需要使用到强制转换
2) 强转符号只针对于最近的操作数有效，往往会使用小括号提升优先级

3) Char 类型可以保存 Int 的常量值，但不能保存 Int 的变量值，需要强转
4) Byte 和 Short 类型在进行运算时，当做 Int 类型处理

## 值类型和String类型的转换

### 基本类型转 String 类型

语法： 将基本类型的值+"" 即可
案例演示：
val d1 = 1.2
//基本数据类型转 string
val s1 = d1 + "" //以后看到有下划线，就表示编译器做了转换

### String 类型转基本数据类型

语法：通过基本类型的 String 的 toXxx 方法即可

### 细节注意

1）在将 String 类型转成 基本数据类型时，要确保 String 类型能够转成有效的数据，比如 我们可以把 "123" , 转成一个整数，但是不能把 "hello" 转成一个整数

2) 思考就是要把 "12.5" 转成 Int          //不能成功，在 scala 中，不是将小数点																后的数据进行截取，而是会抛出异常

## 标识符命名规范

### 概念

1) Scala 对各种变量、方法、函数等命名时使用的字符序列称为标识符
2) 凡是自己可以起名字的地方都叫标识符

### 命名规则

1) Scala 中的标识符声明，基本和 Java 是一致的，但是细节上会有所变化。
2) 首字符为字母，后续字符任意字母和数字，美元符号，可后接下划线_
3) 数字不可以开头。

4) 首字符为操作符(比如+ - * / )，后续字符也需跟操作符 ,至少一个(反编译)

++ => $plus$plus

5) 操作符(比如+-*/)不能在标识符中间和最后. 

6) 用反引号\`....\`包括的任意字符串，即使是关键字(39 个)也可以 [true]

## 特殊符号_

用途：

作为缺省值赋给变量 	var hgb:String = _

导入包是作为类的所有子类   	import scala.io._

foreach循环时，作为每次取出的元素，即item 	("hello").foreach(println(_))

import java.util.{ HashMap=>_, _} // 含义为 引入 java.util 包的所有类，但是忽略 HahsMap 类.

# 第三章 运算符

## 运算符介绍

运算符是一种特殊的符号，用以表示数据的运算、赋值和比较等。
1) 算术运算符
2) 赋值运算符
3) 比较运算符(关系运算符)
4) 逻辑运算符
5) 位运算符

## 算数运算符

![image-20200421201139921](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200421201139921.png)

scala里的整数除是对运算结果的向下取整

% 的运算的原则: a % b = a - a/b * b 

println(-10 % -3 ) // -1 // -10 % -3 = (-10)- (3) * -3 = -10 + 9 = -1

说明,在 scala 中没有 ++ 和 --， 而使用 +=1 和 -= 1

## 赋值运算符

![image-20200421201907923](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200421201907923.png)

![image-20200421201915633](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200421201915633.png)

## 比较运算符（关系运算符）

![image-20200421201559043](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200421201559043.png)

使用陷阱: 如果两个浮点数进行比较，应当保证数据类型一致.

## 逻辑运算符

![image-20200421201726723](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200421201726723.png)

## 位运算符(难点)

![image-20200421202134940](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200421202134940.png)

## 特别说明

Scala 不支持三目运算符 , 在 Scala 中使用 if – else 的方式实现

```scala
val num = 5 > 4 ? 5 : 4 //没有
val num2 = if (5>4) 5 else 4
//若没有取到值，则返回Unit的实例()
val num3 = if(1>2)5      //()
```

## 运算符优先级

1.() []
2.单目运算
3.算术运算符
4.移位运算
5.比较运算符(关系运算符)
6.位运算
7.关系运算符
8.赋值运算
9.,

![image-20200421202341157](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200421202341157.png)

## 用户键盘输入

导入包

```scala
import scala.io.StdIn
val name = StdIn.readLine()
```

# 第四章 程序流程控制

## 1) 顺序控制

## 2) 分支控制（if-else）

## 3)Switch分支控制

在 scala 中没有 switch,而是使用模式匹配来处理match-case

## 4) 循环

### for循环控制方式一

Scala 也为 for 循环这一常见的控制结构提供了非常多的特性，这些 for 循环的特性被称为 for 推导式（for comprehension）或 for 表达式（for expression）

案例：

```scala
for(i <- 1 to 3){      	//表示i从1到3取值，每次加一
print(i + " ")
}
```

说明：

i 表示循环的变量， <- 规定好 to 规定
i 将会从 1-3 循环， 前后闭合

这里的i是val类型，不予许在for循环内修改值

也可以对一个List列表遍历里面的元素

### for循环控制方式二

案例：

```scala
for(i <- 1 until 3) {
print(i + " ")
}
```

说明：

1) 这种方式和前面的区别在于 i 是从 1 到 3-1，左闭右开
2) 前闭合后开的范围,和 java 的 arr.length() 类似
for (int i = 0; i < arr.lenght; i++){}

### 循环守卫

案例：

```scala
for(i <- 1 to 3 if i != 2) {
print(i + " ")
}
```

当且仅当if后面的判断条件为真时运行for循环体，为假则跳过，类似continue

等价于：

```scala
for (i <- 1 to 3) {
    if (i != 2) {
    println(i+"")
    }
}
```

### 引入变量

案例：

```
for(i <- 1 to 3; j = 4 - i) {
print(j + " ")
}
```

说明：

没有关键字，所以范围后一定要加；来隔断逻辑
上面的代码等价

```
for ( i <- 1 to 3) {
    val j = 4 –i
    print(j+"")
}
```

### 嵌套循环

案例：

```
for(i <- 1 to 3; j <- 1 to 3) {
	println(" i =" + i + " j = " + j)
}
```

等价于：

```
for ( i <- 1 to 3) {
    for ( j <- 1to 3){
  	  println(i + " " + j + " ")
    }
}
```

### 循环返回值

案例：

```
val res = for(i <- 1 to 10) yield i
println(res)            //Vector(1,2,...,10)
```

说明：

yield i 将每次循环得到 i 放入到集合 Vector 中，并返回给 res

i 这里是一个代码块，这就意味我们可以对 i 进行处理
下面的这个方式，就体现出 scala 一个重要的语法特点，就是将一个集合中个各个数据，进行处理，并返回给新的集合

```
val res = for(i <- 1 to 10) yield {
    if (i % 2 == 0) {
    i
    }else {
    "不是偶数" 
    }
 }
println(res)
```

### 使用花括号可以代替小括号

### for循环控制步长

使用循环守卫控制，for(i <- 1 to 10 if i % 2 == 0)

使用Range（start,end,step）函数控制,  for(i <- Range(1,10,2))

### while循环

和java一样，可以用，但不推荐

### do-while循环

和上面一样

### 循环中断break()

#### 说明：

Scala 内置控制结构特地去掉了 break 和 continue，是为了更好的适应函数化编程，推荐使用函数式的风格解决 break 和 contine 的功能，而不是一个关键字。

案例：

```
import util.control.Breaks._
breakable {
    while (n <= 20) {
    n += 1
    println("n=" + n)
    if (n == 18) {
    //中断 while
    //说明
    //1. 在 scala 中使用函数式的 break 函数中断循环
    //2. def break(): Nothing = { throw breakException }
    //3. breakable 对 break()抛出的异常做了处理,代码就继续执行
     break()
   	 }
	}
}
```

```
breakable 是一个高阶函数：可以接收函数的函数就是高阶函数（后面详解）
// def breakable(op: => Unit) {
// try {
// op
// } catch {
// case ex: BreakControl =>
// if (ex ne breakException) throw ex
// }
// }
// (1) op: => Unit 表示接收的参数是一个没有输入，也没有返回值的函数
// (2) 即可以简单理解可以接收一段代码块
```

### 实现continue

利用循环守卫或if-else来实现

# 第五章 函数式编程基础

搞懂函数式编程需要先搞懂函数式编程基础——》面向对象编程——》函数式编程高级

## 函数式编程介绍

1）scala中的函数和java中的方法是同等地位，两个东西类似，但是函数更加灵活

2）函数像变量一样，既可以作为函数的参数使用，也可以将函数赋值给一个变量.，函数的创建**不用依赖**于类或者对象，而在 Java 当中，函数的创建则要依赖于类、抽象类或者接口.

![image-20200422212933515](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200422212933515.png)

### 小结：

1) "函数式编程"是一种"**编程范式**"（programming paradigm）。
2) 它属于"**结构化编程**"的一种，主要思想是把运算过程尽量写成一系列嵌套的函数调用。
3) 函数式编程中，**将函数也当做数据类型**，因此可以接受函数当作输入（参数）和输出（返回值）。
4) 函数式编程中，**最重要**的就是函数。

## 为什么需要函数？

解决两个问题：

1. 代码冗余
2. 不利于代码的维护

## 如何写一个函数

**抽取**代码块的**功能**，形成函数

## 基本语法

def 函数名 ([参数名: 参数类型], ...)[[: 返回值类型] =] {
语句... return 返回值
}

函数可以有返回值,也可以没有
返回值形式 1:	 : 返回值类型 = 

返回值形式 2: 	= 表示返回值类型不确定，使用类型推导完成
返回值形式 3: 	   表示没有返回值，return 不生效
如果没有 return ,默认以执行到最后一行的结果作为返回值

## 函数递归调用机制

函数递归需要遵守的重要原则（总结）:
1) 程序执行一个函数时，就创建一个新的受保护的**独立空间**(新函数栈)
2) 函数的局**部变量是独立的**，不会相互影响，**除非是引用类型**，引用类型改变的是变量名在堆里的地址的指向
3) 递归必须向退出递归的条件逼近，否则就是无限递归，死龟了，**递归出口**
4) 当一个函数执行完毕，或者遇到 return，就会返回，遵守谁调用，就将结果返回给谁

![image-20200422214024064](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200422214024064.png)

递归可以从前往后计算（一元二次方程），也可以从后往前计算（猴子吃桃），关键是找到规律和递归出口

## 函数注意事项和细节讨论

1) 函数的形参列表可以是多个, 如果函数没有形参，调用时 可以**不带()**
2) 形参列表和返回值列表的数据类型可以是**值类型**和**引用类型**

3) Scala 中的函数可以根据函数体最后一行代码**自行推断**函数返回值类型。那么在这种情况下，**return 关键字可以省略**

4) 因为 Scala 可以自行推断，所以在省略 return 关键字的场合，**返回值类型也可以省略**

5) 如果函数明确**使用 return** 关键字，那么函数返回就**不能使用自行推断**了,这时要明确写成 : 返回类型 = ，当然如果你什么都不写，即使有 return 返回值为() .

6) 如果函数明确声明**无返回值**（声明 Unit），那么函数体中即使使用 return 关键字也**不会有返回值**

7) 如果明确函数无返回值或不确定返回值类型，那么返回值类型可以省略(或声明为 Any

8) Scala 语法中任何的语法结构都可以嵌套其他语法结构(灵活)，即：**函数中可以再声明/定义函数，类中可以再声明类** ，方法中可以再声明/定义方法。每个函数开辟新的栈，局部变量相互独立。

9) Scala 函数的**形参**，在声明参数时，直接赋初始值(**默认值**)，这时调用函数时，**如果没有指定实参，则会使用默认值**。**如果指定了实参，则实参会覆盖默认值。**

10) 如果函数存在多个参数，每一个参数都可以设定默认值，那么这个时候，传递的参数到底是覆盖默认值，还是赋值给没有默认值的参数，就不确定了(默认按照**声明顺序[从左到右]**)。在这种情况下，可以采用**带名参数**

11) 递归函数未执行之前是无法推断出来结果类型，在使用时必须有明确的返回值类型，**即递归函数必须有返回值**

## Scala 函数支持可变参数

### 基本语法

//支持 0 到多个参数
def sum(args :Int*) : Int = {
}
//支持 1 到多个参数
def sum(n1: Int, args: Int\*) : Int = {
}

### 使用的注意事项

1. args 是集合, 通过 for 循环 可以访问到各个值。
2. 可变参数需要写在形参列表的

### 案例

```scala
def sum(n1: Int, args: Int*): Int = {...}
```

## 过程

### 基本概念

**没有返回值**类型的**函数**叫做**过程**

## 惰性函数

### 基本概念

当真正用到某些代码的时候，才去执行他们，**您可以将耗时的计算推迟到绝对需要的时候。**Java 并没有为惰性提供原生支持，Scala 提供了。

与大数据及时查询的思想一致，要用的时候再算！极大提高效率。

### lazy

当函数返回值被声明为 lazy 时，函数的执行将被推迟，直到我们首次对此取值，该函数才会执行。在 Java 的某些框架代码中称之为懒加载(延迟加载)。

### 案例

```scala
lazy val res = sum(10, 20)
lazy def sum(n1: Int, n2: Int): Int = {
    println("sum() 执行了..") //输出一句话
    return n1 + n2
}
```

### 注意事项

1) **lazy 不能修饰 var** 类型的变量
2) 不但是 在调用函数时，**加了 lazy ,会导致函数的执行被推迟**，我们在声明一个变量时，如果给变量声明了 lazy ,**那么变量值得分配也会推迟**。 比如 lazy val i = 10

## 异常

### 介绍

Scala 提供 try 和 catch 和finally块来处理异常。try 块用于包含可能出错的代码。catch 块用于处理 try 块中发生的异常。可以根据需要在程序中有任意数量的 try...catch 块。语法处理上和 Java 类似，但是又不尽相同

### 案例：

```scala
try {
    val r = 10 / 0
    } catch {
    //说明
    //1. 在 scala 中只有一个 catch
    //2. 在 catch 中有多个 case, 每个 case 可以匹配一种异常 	 case ex: ArithmeticException
    //3. => 关键符号，表示后面是对该异常的处理代码块
    //4. finally 最终要执行的
    case ex: ArithmeticException=> {
    println("捕获了除数为零的算数异常")
    }
    case ex: Exception => println("捕获了异常")
    } finally {
    // 最终要执行的代码
    println("scala finally...")
}
```

### 小结

可疑代码封装在 try 块中， 程序将不会异常终止。

Scala 没有“checked(编译期)”异常。

用 throw 关键字，抛出一个异常对象。

越具体的异常越要靠前，越普遍的异常越靠后。

所有异常都是 Throwable 的子类型。throw是Nothing类型，因为 Nothing 是所有类型的子类型，所以 throw 表达式可以用在需要类型的地方

# 第六章 面向对象编程（基础）

## Scala语言是面向对象的

1) Java 是面向对象的编程语言，由于历史原因，Java 中还存在着非面向对象的内容:基本类型 ，null，静态方法等。
2) Scala 语言来自于 Java，所以天生就是面向对象的语言，而且 Scala 是纯粹的面向对象的语言，即在 Scala 中，一切皆为对象。
3) 在面向对象的学习过程中可以对比着 Java 语言学习

在类里面，当我们声明了 var name :String 时, 在底层对应 private name
同时会**生成 两个 public 方法** name() <=类似=> **getter** public name_$eq() => **setter**

在调用类的方法时，如果是赋值，则cat.name = "小白" 相当于cat.name_$eq("小白")；如果是取值，cat.name相当于cat的getter方法

每个类对应生成一个class文件，默认是public

## 类和对象的区别和联系

1) 类是抽象的，概念的，代表一类事物,比如人类,猫类.. 

2) 对象是具体的，实际的，代表一个具体事物
3) 类是对象的模板，对象是类的一个个体，对应一个实例
4) Scala 中类和对象的区别和联系 和 Java 是一样的。

## 如何定义类？

### 基本语法

[修饰符] class 类名 {
类体
}

### 定义类的注意事项

1) scala 语法中，类并不声明为 public，所有这些类都具有公有可见性(即默认就是 public),[修饰符在后面再详解].

 2) 一个 Scala 源文件可以包含多个类.,而且默认都是 public



## 如何定义对象？

### 基本语法

val | var 对象名 [：类型] = new 类型()

### 说明

1) 如果我们不希望改变对象的引用(即：内存地址), 应该声明为 val 性质的，否则声明为 var, scala设计者推荐使用 val ,因为一般来说，在程序中，我们只是改变对象属性的值，而不是改变对象的引用
2) scala 在声明对象变量时，可以根据创建对象的类型自动推断，所以类型声明可以省略，但当类型和后面 new 对象类型有继承关系即多态时，就必须写了

```scala
val p1 = new Person2
val p2 = p1
println(p1 == p2) // true，将p2的地址引用指向p1的地址引用
```

![image-20200424211718945](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200424211718945.png)

## 方法

scala中的方法就是函数

## 构造器

### 介绍

构造器也叫**构造方法**，类创建时的方法

和 Java 一样，Scala 构造对象也需要调用构造方法，并且可以有任意多个构造方法（即 scala 中构造器也**支持重载**）。

Scala 类的构造器包括： **主构造器** 和 **辅助构造器**

### 基本语法

class 类名 (形参列表) { // （）为主构造器
// 类体
def this(形参列表) { // 辅助构造器
}
def this(形参列表) { //辅助构造器可以有多个... }
}

### 说明

1.辅助构造器 函数的名称 this, 可以有多个，编译器通过不同参数来区分

2.重写的方法在def前加上override关键字	override def toString: String ={}

3.**每个辅助构造器都要直接或间接的调用主构造器**

### 注意事项和细节

1) Scala 构造器作用是完成对新对象的初始化，构造器没有返回值。
2) 主构造器的声明直接放置于类名之后 [反编译]
3) **主构造器会执行类定义中的所有语句**，构造器也是方法（函数），传递参数和使用方法和前面的函数部分内容没有区别【案例演示+反编译】
4) 如果主构造器无参数，小括号可省略，构建对象时调用的构造方法的小括号也可以省略
5) 辅助构造器名称为 this（这个和 Java 是不一样的），多个辅助构造器通过不同参数列表进行区分， 在底层就是 f 构造器重载。

```scala
def this(name : String) {
//辅助构造器无论是直接或间接，最终都一定要调用主构造器，执行主构造器的逻辑
//而且需要放在辅助构造器的第一行[这点和 java 一样，java 中一个构造器要调用同类的其它构造
器，也需要放在第一行]
this() //直接调用主构造器
this.name = name
}
```

6) 如果想让主构造器变成私有的，可以在()之前加上 private，这样用户只能通过辅助构造器来构造对象了【反编译】	class Person2 private() {}

## 属性高级

### 构造器参数

如果构造器**没有任何修饰**符修饰，则认为是**局部变量**

**被var修饰**，认为是**私有的成员属性**，生成对应的xxx() [setter]、xxx_$eq() [getter]方法

被**val修饰**，认为是**私有的只读的成员属性**，**只生成getter**方法

### Bean属性

#### 介绍

​	JavaBeans 规范定义了 Java 的属性是像 getXxx（）和 setXxx（）的方法。许多 Java 工具（框架）都依赖这个命名习惯。为了 Java 的互操作性。将 Scala 字段加@BeanProperty 时，这样会自动生成规范的 setXxx/getXxx 方法。这时可以使用 对象.setXxx() 和 对象.getXxx() 来调用属性。

注意:给某个属性加入@BeanPropetry 注解后，会生成 getXXX 和 setXXX 的方法
并且对原来底层自动生成类似 xxx(),xxx_$eq()方法，**没有冲突，二者可以共存**

## scala对象创建的流程分析

1) 加载类的信息(属性信息，方法信息)
2) 在内存中(堆)开辟空间
3) 使用父类的构造器(主和辅助)进行初始
4) 使用主构造器对属性进行初始化 【age:90, naem nul】
5) 使用辅助构造器对属性进行初始化 【 age:20, naem 小倩 】
6) 将开辟的对象的地址赋给 p 这个引用

# 第七章 面向对象编程（中级）

## 包

### Scala 包的作用

1) 区分相同名字的类
2) 当类很多时,可以很好的管理类
3) 控制访问范围
4）可以对类的功能进行扩展

### Scala 包的基本介绍

和 Java 一样，Scala 中管理项目可以使用包，但 Scala 中的包的功能更加强大，使用也相对复杂些，下面我们学习 Scala 包的使用和注意事项

```
val tiger1 = new com.atguigu.chapter07.scalapackage.xh.Tiger
```

Scala 中**包名和源码所在的系统文件目录结构要可以不一致**，但是编译后的**字节码文件路径和包名会保持一致**(这个工作由**编译器完成**)。

### Scala中自动引入的包

- java.lang.*
- scala.的包
- Predef包

### 使用细节

三种导包的方式：

1.package com.athgb.scala 	写在文件头，后面再写class，特质 trait,object

2.package com.atguigu{} 表示我们创建了包 com.atguigu ,在{}中
即 sacla 支持，在一个文件中，可以同时创建多个包，以及给各个包创建类,trait 和 object

3.包中有包想

**作用域原则**：可以直接向上访问。即: Scala 中子包中直接访问父包中的内容, 大括号体现作用域。(提示：Java 中子包使用父包的类，需要 import)。在子包和父包 **类重名时**，**默认采用就近原则**，如果希望**指定使用某个类，则带上包名**即可。

**包名可以相对也可以绝对**，比如，访问 BeanProperty 的绝对路径是：
\_root\_. scala.beans.BeanProperty ，在一般情况下：我们使用相对路径来引入包，只有当包名冲突时，使用绝对路径来处理

### 包对象

基本介绍：包可以包含类、对象和特质 trait，但不能包含函数/方法或变量的定义。这是 Java 虚拟机的局限。为了弥补这一点不足，scala 提供了包对象的概念来解决这个问题

```scala
package com.atguigu { //包 com.atguigu
//说明
//1. 在包中直接写方法，或者定义变量，就错误==>使用包对象的技术来解决
//2. package object scala 表示创建一个包对象 scala, 他是 com.atguigu.scala 这个包对应的包对象
//3. 每一个包都可以有一个包对象
//4. 包对象的名字需要和子包一样
//5. 在包对象中可以定义变量，方法
//6. 在包对象中定义的变量和方法，就可以在对应的包中使用
//7. 在底层这个包对象会生成两个类 package.class package$.class
    package object scala {
  	  var name = "king" 
  	  def sayHiv(): Unit = {
 	  println("package object scala sayHI~")
    }
 package  scala{...可以直接使用name和sayHiv()}
}
```

![image-20200424220651144](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200424220651144.png)

## 包的可见性问题

java 提供四种访问控制修饰符号控制方法和变量的访问权限（范围）:
1) 公开级别:用 **public** 修饰,对外公开
2) 受保护级别:用 **protected** 修饰,对子类和同一个包中的类公开
3) 默认级别:没有修饰符号,向同一个包的类公开. 
4) 私有级别:用 **private** 修饰,只有类本身可以访问,不对外公开

**在 Scala 中**，你可以通过类似的修饰符达到同样的效果。但是**使用上有区别**

1) 当**属性访问权限为默认**时，从底层看**属性是 private** 的，但是因为提供了 xxx_$eq()[类似setter]/xxx()[类似 getter] 方法，因此从**使用效果看是任何地方都可以访问**）
2) 当**方法访问权限为默认**时，**默认为 public 访问权限**
3) **private** 为私有权限，只在**类的内部和伴生对象中可用** 
4) **protected** 为受保护权限，scala 中受保护权限比 Java 中更严格，**只能子类访问，同包无法访问**
5) 在 **scala 中没有 public** 关键字,即**不能用 public 显式的修饰属性和方法**。
6) 包访问权限[]（表示属性有了限制。同时包也有了限制），这点和 Java 不一样，体现出 Scala 包使用的灵活性

![image-20200424222005702](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200424222005702.png)

当一个文件中出现了 class Clerk 和 object Clerk
//1. class Clerk 称为**伴生类**
//2. object Clerk 的**伴生对象**
//3. 因为 scala 设计者将 static 拿掉, 他就是设计了 伴生类和伴生对象的概念
//4. **伴生类 写非静态的内容 伴生对象 就是静态内容**

## 包的引入

1.全局引入，即在文件开头引入

2.局部引入，在要用到的地方引入

```scala
class User {
	import scala.beans.BeanProperty
	@BeanProperty var name : String = ""
}
```

3.按需引入

```scala
import scala.collection.mutable.{HashMap, HashSet}
```

4.重命名引入

```
import java.util.{ HashMap=>JavaHashMap, List}
```

4.除去个别包引入

```
import java.util.{ HashMap=>_, _} // 含义为 引入 java.util 包的所有类，但是忽略 HahsMap 类.
```

## 继承

scala有和java一样的继承关系

好处：

1) 代码的**复用性**提高了
2) 代码的**扩展性**和**维护性提高**了【面试官问:当我们修改父类时，对应的子类就会继承相应的方法和属性】

子类继承了所有的属性，只是私有的属性不能直接访问，需要通过公共的方法去访问

![image-20200424223458985](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200424223458985.png)

### Scala 中类型检查和转换

#### 基本介绍

要测试某个对象是否属于某个给定的类，可以用 isInstanceOf 方法。用 asInstanceOf 方法将引用转换为子类的引用。classOf 获取对象的类名。

classOf[String]就如同 Java 的 String.class 。
obj.isInstanceOf[T]就如同 Java 的 obj instanceof T 判断 obj 是不是 T 类型。
obj.asInstanceOf[T]就如同 Java 的(T)obj 将 obj 强转成 T 类型，返回一个T对象

### 超类构造

#### 超类的构造说明

1.类有一个主构造器和多个辅助构造器，每个辅助构造器都需要先调用主构造器

2.只有主构造器可以调用父类的构造器，辅助构造器不能直接调用父类的构造器

即 class Person（name:string） extends Student(name){},可选择继承父类的构造器，即传入参数多样化

### 覆写字段

#### 基本介绍

在scala中，子类改写父类的字段，称为覆写/重写字段，关键字override

java中只有方法的重写，没有属性/字段的重写，准确的讲，是隐藏字段代替了重写。

java的动态绑定机制：

1.如果调用的是方法，则JVM会将该方法和对象的内存地址绑定

2.如果调用的是一个属性，则没有动态绑定，遵循就近原则

#### Scala覆写字段案例

```scala
class Person{
	val name :String = _		//自动生成public name()
}
class Student exdents Person{
	override val name : String ="hgb" //重写了public name()
}
```

因为scala禁止对成员属性直接访问，只提供方法来获取成员属性，所以scala的覆写字段实际上是覆写了对应的方法，又因为JVM的动态绑定机制，所以调用的是内存地址上的方法，即引用对象的方法。

#### 细节

1. def只能重写def
2. val只能重写另一个val属性或者重写不带参数的def，根本是对scala自动生成的xxx()和xxx_$eq()方法的重写
3. var只能重写另一个抽象的var属性，本质是对抽象方法的实现

### 继承层级

在scala中，所有其他类的都是AnyRef的子类类似Java中的Object

AnyVal和AnyRef都扩展自Any，Any是根节点

Any中定义了isInstantceOf、asInstantceOf，以及哈希方法等

Null类型有唯一实例null，是所有AnyRef的子类

Nothing类型没有实例

## 抽象

### 基本介绍

一个属性没有初始化，那么这个属性就是抽象属性

抽象属性编译成class文件时，并不会声明，只会生成对应的抽象方法，所以类必须声明为抽象类（abstract）

覆写父类的抽象属性时，可以省略override关键字

### 快速入门

```scala
abstract class Aniaml{
	val name :String 		//抽象字段
	val age : Int			//抽象字段
	var color : String ="yellow"	//普通字段
	def cry()				//抽象方法
}
```

**即抽象属性不初始化，抽象方法没有方法体**

### 说明

抽象类的价值在于设计，方便管理类

### 细节说明

1. 抽象类中可以有实现的方法，也可以没有抽象字段或方法
2. 一个抽象类是不能直接实例化的，但是可以在实例化的同时，动态的实现抽象方法
3. 一旦存在抽象字段或者抽象方法，必须声明为抽象类
4. 抽象方法不能用abstract修饰
5. 如果一个类继承了抽象类，则必须实现抽象类的方法，除非自己也声明为抽象类
6. 抽象字段和方法不能被private和final修饰

# 第八章 高级特性

## 伴生对象

![image-20200426152556601](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200426152556601.png)

### Java回顾

Java中静态方法并不是通过对象调用的，而是通过**类对象**调用的，所以静态操作并**不是面向对象**的。

在scala中因为一切皆对象，所以没有静态的概念，但为了和Java交互，产生了一种特殊的对象来模拟类对象，即伴生对象。一个类的所有静态内容可以放在它的伴生对象中声明和调用

### 伴生对象小结

伴生对象可以通过伴生对象类名直接引用，伴生对象和伴生类名字一样，scala并没有生成静态的内容，只是实现了方法和属性的调用。伴生对象的实现特性依赖于public staic final MOUDLE$实现的。

伴生类和伴生对象应该声明在同一源文件中。

### apply方法

**在伴生对象中定义apply方法**，可以实现： 类名(参数) 方式来创建对象实例. 

![image-20200426161507497](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200426161507497.png)

## 特质

### Java回顾

- 在Java中, 一个类可以实现多个接口。
- 在Java中，接口之间支持多继承
- 接口中属性都是常量
- 接口中的方法都是抽象的

### Scala接口介绍

在scala中没有接口的概念，因为**接口不是面向对象**的，scala采用**特质trait来代替接口**的概念，如果多个类具有相同的特征就可以把这个特征抽取出来，用关键字trait修饰。trait = interface + abstract class

### 基本语法

```
trait 特质名 {
	trait体
}
//没有父类
class  类名   extends   特质1   with    特质2   with   特质3 ..
//有父类
class  类名   extends   父类   with  特质1   with   特质2   with 特质3
```

### 细节说明

在scala中，**java中的接口可以当做特质使用**

特质需要用extents关键字来继承，**有多个特质时，用with连接**

可以把特质可以看作是对继承的一种**补充**，因为父类只能单继承，特质可以多继承

特质可以**同时拥有抽象方法和实现方法**

特质可以**动态混入**，var oracle = new OracleDB with  Operate3

## 叠加特质

### 基本介绍

构建对象的同时如果**混入多个特质**，称之为叠加特质，那么**特质声明顺序从左到右**，**方法执行顺序从右到左**。

### 案列

```scala
trait File4 extends  Data4 {
  println("File4")
  override def insert(id : Int): Unit = {
    print("向文件")
    super.insert(id)			//向左边调用特质的方法
    super[Data4].insert(id)		//调用指定特质的方法
  }}
```

### 说明

1.Scala在叠加特质的时候，**会首先从后面的特质开始执行**
2.Scala中特质中如果调用super，并不是表示调用父特质的方法，**而是向前面（左边）继续查找特质**，如果找不到，才会去父特质查找

```
val mysql = new MySQL4 with DB4 with File4
//val mysql = new MySQL4 with File4 with DB4
```

因为没有完全的实现，需要其它特质继续实现[通过混入顺序，可以加入abstract override 关键字

## 富接口

富接口：即该特质中既有抽象方法，又有非抽象方法

## 特质中的具体字段

混入该特质的类就具有了该字段，字段不是继承，而是直接加入类，成为自己的字段。

## 特质构造顺序

### 介绍

特质也是有构造器的，构造器中的内容由“字段的初始化”和一些其他语句构成。具体实现请参考“特质叠加”

第一种特质构造顺序(声明类的同时混入特质)

```scala
class FF extends EE with CC with DD {
  println("F....")
}
 val ff1 = new FF()		//这时会先调用父类，再从左往右调用特质的父类，	如果特质父类已经调用过了，则调用子类特质的构造器，最后调用FF的构造器
```

第二种特质构造顺序(在构建对象时，动态混入特质)

```scala
 class KK extends EE {
  println("K....")
}
 val ff2 = new KK() with CC with DD
 //这时会先调用KK的父类再调用kk的构造器，然后调用特质父类和子类的构造
```

## 扩展类的特质

特质可以继承类，以用来拓展该类的一些功能

所有混入该特质的类，会自动成为那个特质所继承的超类的子类

### 案列说明

```scala
trait LoggedException extends Exception{
  def log(): Unit ={
    println(getMessage()) // 方法来自于Exception类
  }
}
//UnhappyException 就是Exception的子类.
class UnhappyException extends LoggedException{
    // 已经是Exception的子类了，所以可以重写方法
    override def getMessage = "错误消息！"
}
```

如果混入该特质的类，已经继承了另一个类(A类)，则**要求A类是特质超类的子类**，否则就会出现了**多继承现象**，发生错误。

## 自身类型

### 基本介绍

自身类型：主要是为了解决特质的循环依赖问题，同时可以确保特质在不扩展某个类的情况下，依然可以做到限制混入该特质的类的类型。

### 案例说明

```scala
//Logger就是自身类型特质
trait Logger {
  // 明确告诉编译器，我就是Exception,如果没有这句话，下面的getMessage不能调用
  this: Exception =>
  def log(): Unit ={
    // 既然我就是Exception, 那么就可以调用其中的方法
    println(getMessage)
  }
}
```

## 嵌套类

### 基本介绍

在Scala中，你几乎可以在任何语法结构中内嵌任何语法结构。如在类中可以再定义一个类，这样的类是嵌套类，其他语法结构也是一样。
嵌套类类似于Java中的内部类。

面试题：Java中，类共有五大成员，请说明是哪五大成员
1.属性
2.方法
3.内部类
4.构造器
5.代码块

### Scala中创建嵌套类

定义Scala 的成员内部类和静态内部类，并创建相应的对象实例

案例：

```scala
class ScalaOuterClass {
  class ScalaInnerClass { //成员内部类
  }
}
object ScalaOuterClass {  //伴生对象
  class ScalaStaticInnerClass { //静态内部类
  }
}

 val outer1 : ScalaOuterClass = new ScalaOuterClass();
 val outer2 : ScalaOuterClass = new ScalaOuterClass();

 // Scala创建内部类的方式和Java不一样，将new关键字放置在前，使用  对象.内部类  的方式创建
 val inner1 = new outer1.ScalaInnerClass()
 val inner2 = new outer2.ScalaInnerClass()
 //创建静态内部类对象
 val staticInner = new ScalaOuterClass.ScalaStaticInnerClass()
 println(staticInner)

```

### 内部类使用外部类的属性

内部类如果想要访问外部类的属性，也可以通过外部类别名访问(推荐)。

访问方式：外部类名别名.属性名   【外部类名.this  等价 外部类名别名】

```scala
class ScalaOuterClass {
  myOuter =>  //这样写，你可以理解成这样写，myOuter就是代表外部类的一个对象.即别名
  class ScalaInnerClass { //成员内部类
    def info() = {
      println("name = " + ScalaOuterClass.this.name
        + " age =" + ScalaOuterClass.this.sal)
      println("-----------------------------------")
      println("name = " + myOuter.name
        + " age =" + myOuter.sal)
    }}
  // 当给外部指定别名时，需要将外部类的属性放到别名后.
  var name : String = "scott"
  private var sal : Double = 1.2
}
```

### 类型投影

在scala中内部类从属于外部类的对象，所以外部类的对象不一样，创建出来的内部类也不一样。内部类的实例和创建改内部类的外部类的实例相关联

#### 基本介绍

在方法声明上，如果使用  **外部类#内部类**  的方式，表示忽略内部类的对象关系，等同于Java中内部类的语法操作，我们将这种方式称之为 类型投影（即：**忽略对象的创建方式，只考虑类型**）

# 第九章 隐式转换和隐式函数

## 隐式函数

### 基本介绍

以关键字implicit声明，自动调用，将值从一种类型转换到另一种类型

### 快速入门

```scala
implicit def f1(d:Double):Int={
	d.toInt
}
var num :Int=3.5
```

编译器会自动在需要调用的时候调用隐式函数

隐式函数只与参数类型和返回值有关

### 丰富类库

使用隐式函数给一个类动态增加方法

#### 快速入门

```scala
class A{}
class B{
	def insert(){
		printf("ok")
	}
}
implicit def addInsert(a:A):B= new B

var a:A = new A
a.insert()								//调用了隐式函数
```

## 隐式值

隐式值又叫隐式变量，implicit修饰变量时则为隐式变量

### 快速入门

```scala
 def main(args: Array[String]): Unit = {
      // 隐式变量（值）
      implicit val name: String = "Scala"

      def hello(implicit content: String = "jack"): Unit = {
        println("Hello " + content)
      } //调用hello
      hello	//匹配隐式函数hello()和隐式值name,输出“Hello Scala"
    		//底层调用hello$1(name)
 }
```

### 小结：

编译器优先级：实参 > 隐式值 > 缺省值

隐式值有唯一性

## 隐式类

### 基本介绍

使用implicit声明类，同样可以扩展类的功能，比前面使用隐式转换丰富类库功能更加的方便，在集合中隐式类会发挥重要的作用。

### 特点：

- 其所带的构造参数有且只能有一个
- 隐式类必须被定义在“类”或“伴生对象”或“包对象”里，即隐式类不能是 顶级的(top-level  objects)。
- 隐式类不能是case class（case class在后续介绍 样例类）
- 作用域内不能有与之相同名称的标识符

### 应用案列

```scala
class MySQL1 {
  def sayOk(): Unit = {
    println("sayOk")
  }
}


def main(args: Array[String]): Unit = {
    //DB1会对应生成隐式类
    implicit class DB1(val m: MySQL1) {
      def addSuffix(): String = {
        m + " scala"
      }
    }
    val mysql1 = new MySQL1
    mysql1.sayOk()
    //mysql1.addSuffix() ==> DB1$1(mysql1).addSuffix()
    //DB1$1(mysql1)返回的类型是 ImplicitClass$DB1$2
    println(mysql1.addSuffix())
  }
```



## 隐式转换的时机

- 当方法中的参数类型与目标类型不一致时
- 当对象调用本身没有的方法时
- 隐式转换存在作用域限制
- 注意自身不能调用自身

# 第十章 数据结构

## 数据结构特点

### scala集合基本介绍

1.集合分为**可变集合**和**不可变集合**

不可变集合：scala.collection.inmutable		线程安全

可变集合 ： scala.collection.mutable			线程不安全

2.scala默认采用不可变集合，**几乎所有**集合类都支持不可变集合和可变集合

3.scala的三大集合类：序列Seq、集Set、隐射Map，所有集合都扩展自Iterable特质

### 可变集合和不可变集合举例

不可变集合：集合本身不能动态变化，即集合的长度不能变

可变集合：集合本身可以动态变化。即集合的长度可以变化

## 不可变集合继承图

![image-20200427224253003](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200427224253003.png)

### 小结：

1. Set、Map 是 Java 中也有的集合
2.  Seq 是 Java 没有的，我们发现 List 归属到 Seq 了,因此这里的 List 就和 java 不是同
    一个概念了
3. 我们前面的 for 循环有一个 1 to 3 ,就是 IndexedSeq 下的 Vector
4. String 也是属于 IndexeSeq
5. 经典的数据结构比如 Queue 和 Stack 被归属到 LinearSeq
6. Scala 中的 Map 体系有一个 SortedMap,说明 Scala 的 Map 可以支持
    排序
7. IndexSeq 和 LinearSeq 的区别[IndexSeq 是通过索引来查找和定位，因此速度快
8. LineaSeq 是线型的，即有头尾的概念，这种数据结构一般是通过遍历来查找。

## 可变集合继承图

![image-20200427224608467](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200427224608467.png)

### 小结：

1. 在可变集合中比不可变集合更加丰富
2. 在 Seq 集合中， 增加了 Buffer 集合，将来开发中，我们常用的有 ArrayBuffer 和 ListBuffer
3. 如果涉及到线程安全可以选择使用 syn.. 开头的集合
4. 其它的说明参考不可变集合

## 定长数组-声明泛型

### 创建

```scala
val arr1 = new Array[Int](10)		//用new创建
val arr2 = Array(1, 2,"hgb")  //使用了Array对象的apply方法，
							//返回了一个Array类型的对象，再赋给arr2
							//这种方法可以创建多种类型的Array
```

### 查找

```scala
arr1(1) = 7			//单元素查看

for (i <- arr01) {				//数组遍历
println(i)
}
```

### 修改

```scala
arr01(1) = 10  		//赋值,集合元素采用小括号访问
```

### 删除

```scala
arr01.remove(index)		//删除下标为index的元素
```

## 变长数组-声明泛型

### CRUD

```scala
val arr2 = ArrayBuffer[Int]()		//创建
arr2.append(7)						//增加，可扩容
arr2(0) = 7							//修改
for (i <- arr01) {					//遍历
println(i)
}
arr01.remove(0)						//删除
```

## 定长数组与可变数组的转换

### 案例

```scala
arr1.toBuffer //定长数组转可变数组
arr2.toArray //可变数组转定长数组
```

### 注意事项：

arr2.toArray 返回结果才是一个定长数组， arr2 本身没有变化
arr1.toBuffer 返回结果才是一个可变数组， arr1 本身没有变化

## 多维数组

### 说明

```scala
val arr = Array.ofDim[Double](3,4)//创建一个3行4列的二维数组
arr(1)(1) = 11.11				//给1行1列赋值
for (item <- arr) {		 //取出二维数组的各个元素（一维数组）
	for (item2 <- item) { // 元素（一维数组） 遍历
		print(item2 + "\t")
	}
println()
}
arr(1)(1) = 100         //修改
println(arr(1)(1))		//取出
```

## Scala数组与Java的List转换

```scala
val arr = ArrayBuffer("1", "2", "3")
import scala.collection.JavaConversions.bufferAsJavaList
val javaArr = new ProcessBuilder(arr)
// 这里 arrList 就是 java 中的 List
val arrList = javaArr.command()
```

## Java 的 List 转 Scala 数组(mutable.Buffer)

```scala
import scala.collection.JavaConversions.asScalaBuffer
// java.util.List ==> Buffer
val scalaArr: mutable.Buffer[String] = arrList //arrList是List
```

## 元组

**将多个无关的数据封装为一个整体**，称为元组，**没有类型限制**

**元组有22个子类**，从tuple1到tuple22，**编译器自动识别参数的个数**来选择对应的子类

创建：

```
val tuple1 = (1, 2, 3, "hello", 4) //创建一个tuple5
```

访问元组中的数据,可以采用顺序号（_顺序号），也可以通过索（productElement）访问。

读取：

```scala
val t1 = (1, "a", "b", true, 2)
println(t1._1) // 1 //访问元组的第一个元素 ，从 1 开始
println(t1.productElement(0)) // 0 // 访问元组的第一个元素，从 0 开始
```

遍历：

```scala
for (item <- t1.productIterator) {
	println("item=" + item)
}
```

## 列表-List

### 基本介绍

Scala 中的 List 和 Java List 不一样，在 Java 中 List 是一个接口，真正存放数据是 ArrayList，而 **Scala的 List 可以直接存放数据，就是一个 object**，默认情况下 Scala 的 List 是不可变的，List 属于序列 Seq。

### 创建：

```scala
val List = scala.collection.immutable.List
//在 scala 中,List 就是不可变的，如需要使用可变的 List,则使用 //ListBuffer
val Nil = scala.collection.immutable.Nil // List()，Nil是空集合
val list01 = List(1, 2, 3) //创建时，直接分配元素
```

### 读取：

```scala
val value1 = list01(1) // 1 是索引，表示取出第 2 个元素
```

### 增加：

通过 :+ 和 +: 给 list 追加元素(**本身的集合并没有变化**)

```scala
// :+运算符表示在列表的最后增加数据
val list2 = list1 :+ 4 // (1,2,3,"abc", 4)
//在最前面增加数据
val list3 = 10 +: list1 // (10,1, 2, 3, "abc")
```

- **符号::表示向集合中 新建集合添加元素**。
- 运算时，**集合对象一定要放置在最右边**，
- 运算规则，**从右向左**。
- **::: 运算符是将集合中的每一个元素加入到集合中去**

```scala
val list4 = List(1, 2, 3, "abc")
//说明 val list5 = 4 :: 5 :: 6 :: list4 :: Nil 步骤
//1. List()
//2. List(List(1, 2, 3, "abc"))
//3. List(6,List(1, 2, 3, "abc"))
//4. List(5,6,List(1, 2, 3, "abc"))
//5. List(4,5,6,List(1, 2, 3, "abc"))
val list5 = 4 :: 5 :: 6 :: list4 :: Nil
println("list5=" + list5)
```

## 列表-ListBuffer

### 基本介绍

ListBuffer是可变的list集合，可以添加，删除元素,ListBuffer属于序列

### 基本语法

```scala
val lst0 = ListBuffer[Int](1, 2, 3)

println("lst0(2)=" + lst0(2))
for (item <- lst0) {
println("item=" + item)
}

val lst1 = new ListBuffer[Int]
lst1 += 4		//添加一个元素
lst1.append(5)
	
lst0 ++= lst1		//将lst1的所有元素添加到lst0
val lst2 = lst0 ++ lst1
val lst3 = lst0 :+ 5

println("=====删除=======")
println("lst1=" + lst1)
lst1.remove(1)				//根据索引删除
lst1 -= 3					//根据内容删除
for (item <- lst1) {
println("item=" + item)
}
```

## 队列-Queue

### 基本介绍

1. 队列是一个有**序列表**，在底层可以用**数组**或是**链表**来实现
2. 遵循**先入先出**的原则
3. **scala中有可变队列和不可变队列**，开发中一般用可变队列

### CRUD：

```scala
val q1 = new Queue[Int]
//添加单个元素
q1 += 20 // +=是包装的方法，底层使用append方法
println(q1)//(20)
//添加集合内的所有元素
q1 ++= List(2,4,6) // ++=是包装的方法，底层循环遍历集合将其元素添加
println(q1)  //(20,2,4,6)
//添加一个集合
q1 += List(1,2,3) //泛型为Any才ok
println(q1)
q1.dequeue() //删除队列头元素
println(q1)
q1.enqueue(9, 8, 7)//在队列的末尾添加
println(q1)
println(q1.head)//返回队列头部元素
println(q1.last)//返回队列最后一个元素
println(q1.tail)//返回除了第一个以外剩余的元素， 可以级联使用
println(q1.tail.tail)
```

## 映射Map

### 基本介绍

HashMap 是一个散列表(数组+链表)，它存储的内容是键值对(key->value)映射，Java中的HashMap是无序的，key不能重复。

### 创建：

**Scala中不可变的Map是有序的，可变的Map是无序的**

构造不可变Map集合（有序）

```
val map1 = Map("Alice" -> 10, "Bob" -> 20, "Kotlin" -> "北京")
```

在底层构建Map集合中，集合中的元素其实是**Tuple2类型**

构造可变Map集合（无序）

```
//需要指定可变Map的包
val map2 = scala.collection.mutable.Map("Alice" -> 10, "Bob" -> 20, "Kotlin" -> 30)
```

创建空的Map

```
val map3 = new scala.collection.mutable.HashMap[String, Int]
println(map3)
```

对偶元组

```
val map4 = mutable.Map( ("A", 1), ("B", 2), ("C", 3),("D", 30) )
println("map4=" + map4)
println(map4("A"))
```

### 取值：

```scala
map2.contains("Alice")		//返回布尔值，检查是否包含某个key
val value1 = map2("Alice") //如果key不存在，则抛出异常
println(value1)

println(map4.get("A")) //map.get方法会将数据进行包装Some集合
println(map4.get("A").get) //得到Some在取出
println(map4.getOrElse("a",default = 8)) //key存在"a" ?value : 8
```

#### 如何选择取值方式建议

1. 如果我们确定map有这个key ,则应当使用map(key), 速度快
2. 如果我们不能确定map是否有key ,而且有不同的业务逻辑，使用map.contains() 先判断在加入逻辑 
3. 如果只是简单的希望得到一个值，使用map4.getOrElse("ip","127.0.0.1")

### 修改：

```scala
val map4 = mutable.Map( ("A", 1), ("B", "北京"), ("C", 3) )
map4("AA") = 20//存在key?赋值20 : 添加新的键值对("AA",20)
println(map4)
map4 += ( "D" -> 4 ) 		//添加键值对，如果键值对存在，则重新赋值
map4 += ( "B" -> 50 )
println(map4)
```

### 删除：

```scala
val map4 = mutable.Map( ("A", 1), ("B", "北京"), ("C", 3) )
map4 -= ("A", "B") //如果key不存在，则会报错
println("map4=" + map4)
```

### 遍历：

```scala
for ((k, v) <- map1) println(k + " is mapped to " + v)
for (v <- map1.keys) println(v)		//map的keys是Set集合
for (v <- map1.values) println(v)	//map的values是HashMap集合
for(v <- map1) println(v)
```

## Set集合

### 基本介绍

**集是不重复元素的结合**。**集不保留顺序**，默认是以哈希集实现

默认情况下，**Scala 使用的是不可变集合**，如果你想使用**可变集合，需要引用** scala.collection.mutable.Set 包

### 创建：

```scala
val set01 = Set(1,2,4,"abc")
println(set01)
import scala.collection.mutable
val set02 = mutable.Set(1,2,4,"abc")
println(set02)
```

### 添加和删除：

```
set02.add(90)
set02 += 78// 操作符形式
set02.+= (90)// 方法形式
println(set02)
set02 -= 2 // 操作符形式
set02.remove("abc") // 方法的形式，scala的Set可以直接删除值
println(set02)
```

### 查看：

```
val set02 = mutable.Set(1, 2, 4, "abc")
for(x <- set02) {
println(x)
}
```

| 序号 | 方法                                     | 描述                                                 |
| :--: | ---------------------------------------- | ---------------------------------------------------- |
|  1   | def +(elem: A): Set[A]                   | 为集合添加新元素，并创建一个新的集合，除非元素已存在 |
|  2   | def -(elem: A): Set[A]                   | 移除集合中的元素，并创建一个新的集合                 |
|  3   | def contains(elem: A): Boolean           | 如果元素在集合中存在，返回 true，否则返回 false。    |
|  4   | def &(that: Set[A]): Set[A]              | 返回两个集合的交集                                   |
|  5   | def &~(that: Set[A]): Set[A]             | 返回两个集合的差集                                   |
|  6   | def ++(elems: A): Set[A]                 | 合并两个集合                                         |
|  7   | def drop(n: Int): Set[A]]                | 返回丢弃前n个元素新集合                              |
|  8   | def dropRight(n: Int): Set[A]            | 返回丢弃最后n个元素新集合                            |
|  9   | def dropWhile(p: (A) => Boolean): Set[A] | 从左向右丢弃元素，直到条件p不成立                    |
|  10  | def max: A                               | 查找最大元素(泛型需要唯一)                           |
|  11  | def min: A                               | 查找最小元素(泛型需要唯一)                           |
|  12  | def take(n: Int): Set[A]                 | 返回前 n 个元素                                      |

# 第十一章 数据结构下

## 映射操作

能接受函数作为参数的函数，叫做高阶函数

### 基本介绍-map

将集合中的每一个元素通过指定功能（函数）映射（转换）成新的结果集合这里其实就是所谓的将函数作为参数传递给另外一个函数,这是函数式编程的特点

所有集合都可以使用map映射

通用公式

```scala
集合.map(f:(B,A)=>B)		//函数可以是匿名的，也可以定义的
```

### 理解：

map(f:()=>())方法接受一个函数，把集合里的每个元素做为实参循环调用该函数，返回新集合。

### 基本介绍-flatmap

flat即压扁，压平，扁平化映射,效果就是将集合中的每个元素的子元素映射到某个函数并返回新的集合。

### 案例：

```scala
val names = List("Alice", "Bob", "Nick")
def upper( s : String ) : String = {
    s. toUpperCase
}
//注意：每个字符串也是char集合
println(names.flatMap(upper)) 
```

### 理解：

如果集合的元素也是一个集合，那么flamap方法也会被这个集合调用，直到取到的元素不是一个集合，返回一个新的集合

## 集合元素过滤

### 基本介绍

filter：将符合要求的数据(筛选)放置到新的集合中

### 案例：

将  val names = List("Alice", "Bob", "Nick") 集合中首字母为'A'的筛选到新的集合。

```scala
val names = List("Alice", "Bob", "Nick")
def startA(s:String): Boolean = {
s.startsWith("A")
}
val names2 = names.filter(startA)
println("names=" + names2)
```



### 基本使用

```scala
object TestHighOrderDef {
  def main(args: Array[String]): Unit = {
    val res = test(sum, 6.0)
    println("res=" + res)
  }
  def test(f: Double => Double, n1: Double) = {
    f(n1)
  }
  def sum(d: Double): Double = {
    d + d
  }}
```



## reduce_方法（化简）

![img](https://img-blog.csdnimg.cn/20190612154808800.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RybF9ibG9ncw==,size_16,color_FFFFFF,t_70)

reduceLeft(\_-_)从集合的左边第一个元素开始，两个元素通过-号运算的结果给下一个元素运算，属于迭代运算，reduceRight从右边开始，其他一样，\_符号表示运算式左边或右边取出的元素。reduce（）是对reduceLeft()做了一个包装

## fold_方法（折叠）

c.foldLeft(0)(_ * _)方法 

![img](https://img-blog.csdn.net/20170828155720926?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTmV4dF9fT25l/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```scala
    val a2 = a.foldLeft(0)(_ - _) //    0-1-7-2-9 = -19
```

对于foldLeft方法还有一种简写，这种写法的本意是让你通过/:来联想一棵树的样子 
对/:操作符来说，初始值是第一个操作元本题中是0，：后是第二个操作元a

```scala
    val a3 = (0 /: a)(_ - _) //      等价于a.foldLeft(0)(_ - _)
```

scala同样也提供了foldRight或:\的变体，计算

![img](https://img-blog.csdn.net/20170828160531891?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTmV4dF9fT25l/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

```scala
    val a4 = a.foldRight(0)(_ - _)//    1-(7-(2-(9-0))) = -13
    val a5 = (a :\ 0)(_ - _)   //    等价于a.foldRight(0)(_ - _)
```

折叠可以接受函数，折叠fold有时候可以代替循环，例如： 统计每个字母中字符串出现的次数

```scala
    val frep  = collection.mutable.Map[Char,Int]() //   使用可变集合
//      使用for循环来统计次数
    for(c <- "Mississippi") frep(c) = frep.getOrElse(c, 0)+1
//      使用/: foldLeft()()来统计次数
    val result = (Map[Char,Int]() /: "Mississippi"){
        (m,c) => m + (c -> (m.getOrElse(c, 0)+1))
    }
//      任何while循环都可用fold来代替
    println(frep,result) //(Map(M -> 1, s -> 4, p -> 2, i -> 4),Map(M -> 1, i -> 4, s -> 4, p -> 2))
```

这是使用fold的步骤：在每一步，将频率映射和新遇到的字母结合在一起，产生一个新的频率映射。这就是折叠：

![img](https://img-blog.csdn.net/20170828161238014?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTmV4dF9fT25l/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

fold的初始值和操作符是分开定义的**柯里化参数**，这样scala就可以根据类型来推断操作符的类型定义。

该函数的初始值是String，因此操作符的类型应该是(String,Int) => String的函数

```scala
    val a7 = a.foldLeft("")(_ + _)
    val a8 = a.foldRight("")(_+_)
    val a9 = a.fold(0)(_+_)
    println(a7,a8,a9) //    (1729,1729,19)
```



## scan(扫描)

### 基本介绍

扫描，即对某个集合的所有元素做fold操作，但是会把产生的所有中间结果放置于一个集合中保存

### 案例：

```scala
def minus( num1 : Int, num2 : Int ) : Int = {
num1 - num2
}
//5 (1,2,3,4,5) =>(5,4,2,-1,-5,-10)		默认值在新集合中被保存
val i8 = (1 to 5).scanLeft(5)(minus) //IndexedSeq[Int]
println(i8)
def add( num1 : Int, num2 : Int ) : Int = {
num1 + num2
}
//5 (1,2,3,4,5) =>(5,6,8, 11,15,20)
val i9 = (1 to 5).scanLeft(5)(add) //IndexedSeq[Int]
println(i9)
```

### 理解：

保存了计算过程和默认值的fold方法

## 拉链(合并)

### 基本介绍

在开发中，当我们需要将两个集合进行 对偶元组合并，可以使用拉链。

### 应用实例：

```scala
// 拉链
val list1 = List(1, 2 ,3)
val list2 = List(4, 5, 6)
val list3 = list1.zip(list2) // (1,4),(2,5),(3,6) 
println("list3=" + list3)
```

### 注意事项：

1. 拉链的**本质**就是**两个集合的合并**操作，**合并后每个元素是一个 对偶元组**
2. 合并规则是两个集合下标相同为一组，长度不同的集合合并，会造成数据丢失
3. 合并后的集合，操作方式和普通集合一样

## 迭代器

### 基本说明

通过iterator方法从集合获得一个迭代器，通过while循环和for表达式对集合进行遍历.(学习使用迭代器来遍历)

### 应用案例：

```scala
val iterator = List(1, 2, 3, 4, 5).iterator // 得到迭代器
    println("--------遍历方式1 -----------------")
    while (iterator.hasNext) {
        println(iterator.next())
    }
    println("--------遍历方式2 for -----------------")
    for(enum <- iterator) {
      println(enum) //
    }
```

### 说明：

 iterator 的构建实际是 AbstractIterator 的一个匿名子类，该子类提供了  hasNext next 等方法

## 流(Stream)

### 基本说明

stream是一个集合。这个集合，可以用于**存放无穷多个元素**，但是这无穷个元素并不会一次性生产出来，而是需要用到多大的区间，就会**动态的生产**，**末尾元素遵循lazy规则**(即：要使用结果才进行计算的) 。默认只显示头元素，没有使用到的元素用？表示

### 创建：

```scala
def numsForm(n: BigInt) : Stream[BigInt] = n #:: numsForm(n + 1)
val stream1 = numsForm(1)
```

使用tail，**动态生成新元素**：

```scala
//创建Stream
def numsForm(n: BigInt) : Stream[BigInt] = n #:: numsForm(n + 1)
val stream1 = numsForm(1)
println(stream1) //Stream(1, ?)
//取出第一个元素
println("head=" + stream1.head) //head=1
println(stream1.tail) //Stream(2, ?)
println(stream1) //?Stream(1, 2, ?)
```

使用map映射：

```scala
//创建Stream
def numsForm(n: BigInt) : Stream[BigInt] = n #:: numsForm(n + 1)
def multi(x:BigInt) : BigInt = {
x * x
}
println(numsForm(5).map(multi)) //? (25,?)
```

### 说明：

使用stream集合不能用last属性，否则会无限循环

## 视图(view)

### 基本介绍

Stream的懒加载特性，也可以对其他集合应用view方法来得到类似的效果，具有如下特点：

1. view方法产出一个总是**被懒执行的集合**。
2. view不会缓存数据，每次都要重新计算，比如遍历View时。

### 应用案例

请找到1-100 中，数字倒序排列 和它本身相同的所有数。(1 2, 11, 22, 33 ...)

```scala
def multiple(num: Int): Int = {
num}
def eq(i: Int): Boolean = {
i.toString.equals(i.toString.reverse)
}
//说明: 没有使用view
val viewSquares1 = (1 to 100)
.map(multiple)
.filter(eq)
println(viewSquares1)
//for (x <- viewSquares1) {}
//使用view
val viewSquares2 = (1 to 100)
.view
.map(multiple)
.filter(eq)
println(viewSquares2)
```

## 线程安全集合

### 基本介绍

所有线程安全的集合都是以Synchronized开头的集合

```scala
SynchronizedBuffer
SynchronizedMap
SynchronizedPriorityQueue
SynchronizedQueue
SynchronizedSet
SynchronizedStack
```

## 并行集合

### 基本介绍

Scala为了充分使用多核CPU，提供了并行集合（有别于前面的串行集合），用于多核环境的并行计算。
**主要用到的算法有**： Divide and conquer : 分治算法，Scala通过splitters(分解器)，combiners（组合器）等抽象层来实现，主要原理是将计算工作分解很多任务，分发给一些处理器去完成，并将它们处理结果合并返回Work stealin算法【学数学】，主要用于任务调度负载均衡（load-balancing），**通俗点完成自己的所有任务之后，发现其他人还有活没干完，主动（或被安排）帮他人一起干，这样达到尽早干完的目的**

### 应用案例：

```scala
(1 to 5).foreach(println(_))	//1,2,3,4,5
println()
(1 to 5).par.foreach(println(_)) 	//顺序不确定，同时打印
```



# 第十二章 模式匹配

## match

### 基本介绍

Scala中的模式匹配类似于Java中的switch语法，但是更加强大。

### 应用案例

```scala
val oper = '#'
val n1 = 20
val n2 = 10
var res = 0
oper match {			//对oper进行模式匹配
case '+' => res = n1 + n2
case '-' => res = n1 - n2
case '*' => res = n1 * n2
case '/' => res = n1 / n2
case _ => println("oper error")
}
println("res=" + res)
```

### 注意事项

1. 如果所有case都不匹配，那么会执行case _ 分支，类似于Java中default语句
2. 如果所有case都不匹配，又没有写case _ 分支，那么会抛出MatchError
3. 每个case中，不用break语句，自动中断case
4. => 后面的代码块到下一个 case， 是作为一个整体执行，可以使用{} 扩起来，也可以不扩。
5. 匹配顺序**由上到下**，**匹配到一个就退出**

## 守卫

### 基本介绍

如果想要表达**匹配某个范围的数据**，就需要在模式匹配中增加条件守卫

### 应用案列

```scala
for (ch <- "+-3!") {
var sign = 0
var digit = 0
ch match {
case '+' => sign = 1
case '-' => sign = -1
// 说明..
case _ if ch.toString.equals("3") => digit = 3 //添加条件，忽略了ch的值
case _ => sign = 2
}
println(ch + " " + sign + " " + digit)
}
```

## 模式中的变量

### 基本介绍

如果在case关键字后跟变量名，那么match前表达式的值会赋给那个变量

### 应用案列

```scala
val ch = 'V'
ch match {
case '+' => println("ok~")
case mychar => println("ok~" + mychar)//获取ch的值并赋给mychar
case _ => println ("ok~~")
}
```

## 类型匹配

### 基本介绍

可以匹配对象的任意类型，这样做避免了使用isInstanceOf和asInstanceOf方法

### 应用案列

```scala
// 类型匹配, obj 可能有如下的类型
val a = 7
val obj = if(a == 1) 1
else if(a == 2) "2"
else if(a == 3) BigInt(3)
else if(a == 4) Map("aa" -> 1)
else if(a == 5) Map(1 -> "aa")
else if(a == 6) Array(1, 2, 3)
else if(a == 7) Array("aa", 1)
else if(a == 8) Array("aa")


val result = obj match {
case a : Int => a
case b : Map[String, Int] => "对象是一个字符串-数字的Map集合"
case c : Map[Int, String] => "对象是一个数字-字符串的Map集合"
case d : Array[String] => "对象是一个字符串数组"
case e : Array[Int] => "对象是一个数字数组"
case f : BigInt => Int.MaxValue
case _ => "啥也不是"
}
println(result)
```

### 注意事项

1. Map[String, Int] 和Map[Int, String]是两种不同的类型，其它类推。
2. 在进行类型匹配时，编译器会预先检测是否有可能的匹配，如果没有则报错，可以使用if语句忽略检测，多一种类型，使obj成为Any类型
3. case _ : BigInt => Int.MaxValue 表示只进行类型匹配

## 匹配数组

### 基本介绍

Array(0) 匹配只有一个元素且为0的数组。

Array(x,y) 匹配数组有两个元素，并将两个元素赋值为x和y。当然可以依次类推Array(x,y,z) 匹配数组有3个元素的等等....

Array(0,_*) 匹配数组以0开始的任意个元素的数组

### 应用案列

```scala
for (arr <- Array(Array(0), Array(1, 0), Array(0, 1, 0),
Array(1, 1, 0), Array(1, 1, 0, 1))) {
val result = arr match {
case Array(0) => "0"
case Array(x, y) => x + "=" + y
case Array(0, _*) => "以0开头和数组"
case _ => "什么集合都不是"
}
println("result = " + result)
} 
```

## 匹配列表

### 应用案列

```scala
for (list <- Array(List(0), List(1, 0), List(0, 0, 0), List(1, 0, 0))) {
val result = list match {
case 0 :: Nil => "0" //
case x :: y :: Nil => x + " " + y //
case 0 :: tail => "0 ..." //
case _ => "something else"
}
println(result)
}
```

### 注意事项

Array不能直接打印出来，需要转成ArrayBuffer或者array.toBuffer再打印

## 匹配元组

### 应用案列

```scala
// 元组匹配
for (pair <- Array((0, 1), (1, 0), (1, 1),(1,0,2))) {
val result = pair match { // 
case (0, _) => "0 ..." //
case (y, 0) => (0,y) // 返回元组数据对调的元组
case _ => "other" //.
}
println(result)
}
```

## 对象匹配

### 基本介绍

**case中对象的unapply方法**(对象提取器)**返回Some集合**则为匹配成功返回none集合则为匹配失败

### 应用案列1

```
object Square {
def unapply(z: Double): Option[Double] = Some(math.sqrt(z))
def apply(z: Double): Double = z * z
}
// 模式匹配使用：
val number: Double = 36.0
number match {
case Square(n) => println(n)
case _ => println("nothing matched")
}
```

### 注意事项

1. 构建对象时apply会被调用 ，比如 val n1 = Square(5)
2. 当将 Square(n) 写在 case 后时[case Square(n) => xxx]，会默认调用unapply 方法(对象提取器)
3. number 会被 传递给def unapply(z: Double) 的 z 形参
4. 如果返回的是Some集合，则unapply提取器返回的结果会返回给 n 这个形参
5. case中对象的unapply方法(提取器)返回some集合则为匹配成功
6. 返回none集合则为匹配失败

### 应用案例2

```scala
object Names {
    def unapplySeq(str: String): Option[Seq[String]] = {
      if (str.contains(",")) Some(str.split(","))
      else None
    }}
  val namesString = "Alice,Bob,Thomas"
  //说明
  namesString match {
    case Names(first, second, third) => {
      println("the string contains three people's names")
      // 打印字符串
      println(first+ second +third)
    }
    case _ => println("nothing matched")
  }
```

### 注意事项

1. 当case 后面的**对象提取器方法的参数为多个**，**则会默认调用def unapplySeq() 方法**
2. 如果unapplySeq返回是Some，获取其中的值,判断得到的sequence中的元素的个数是否是三个,如果是三个，则把三个元素分别取出，赋值给first，second和third
3. 其它的规则不变.

## 变量声明中的模式

### 基本介绍

match中每一个case都可以单独提取出来，意思是一样的

### 应用案列

```scala
val (x, y) = (1, 2)			//相当于(1,2) match{case (x,y) =>(x,y)}
val (q, r) = BigInt(10) /% 3  //说明  q = BigInt(10) / 3 r = BigInt(10) % 3
val arr = Array(1, 7, 2, 9)
val Array(first, second, _*) = arr // 提出arr的前两个元素
println(first, second)
```

## for表达式中的模式

### 基本介绍

for循环也可以进行模式匹配.

### 应用案例：

```scala
val map = Map("A"->1, "B"->0, "C"->3)
for ( (k, v) <- map ) {
println(k + " -> " + v)
}
//说明
for ((k, 0) <- map) {
println(k + " --> " + 0)
}
//说明
for ((k, v) <- map if v == 0) {
println(k + " ---> " + v)
}
```

## 样例类(模板)

### 基本介绍

**样例类仍然是类**
样例类用**case关键字进行声明**。
样例类是**为模式匹配而优化**的类
**构造器中的每一个参数都成为val的成员变量**——除非它被显式地声明为var（不建议这样做）
在样例类对应的伴生对象中**提供apply方法**让你不用new关键字就能构造出相应的对象
**提供unapply方法**让模式匹配可以工作
将自动生成toString、equals、hashCode和copy方法(有点类似模板类，直接给生成，供程序员使用)
除上述外，样例类和其他类完全一样。你**可以添加方法和字段**，扩展它们

### 应用案列

```scala
abstract class Amount
case class Dollar(value: Double) extends Amount 
case class Currency(value: Double, unit: String) extends Amount
case object NoAmount extends Amount 

//说明: 这里的 Dollar，Currencry, NoAmount  是样例类。
for (amt <- Array(Dollar(1000.0), Currency(1000.0, "RMB"), NoAmount)) {
val result = amt match {
//说明
case Dollar(v) => "$" + v
//说明
case Currency(v, u) => v + " " + u
case NoAmount => ""
}
println(amt + ": " + result)
}
```

### copy方法

样例类的copy方法和带名参数
copy创建一个与现有对象值相同的新对象，并可以通过带名参数来修改某些属性

```scala
val amt = Currency(29.95, "RMB")
val amt1 = amt.copy() //创建了一个新的对象，但是属性值一样
val amt2 = amt.copy(value = 19.95) //创建了一个新对象，但是修改了货币单位
val amt3 = amt.copy(unit = "英镑")//..
println(amt)
println(amt2)
println(amt3)
```

### 中缀表达式

什么是中置表达式？1 + 2，这就是一个中置表达式。如果unapply方法产出一个元组，你可以在case语句中使用中置表示法。比如可以匹配一个List序列

```scala
List(1, 3, 5, 9) match { //修改并测试
//1.两个元素间::叫中置表达式,至少first，second两个匹配才行.
//2.first 匹配第一个 second 匹配第二个, rest 匹配剩余部分(5,9)
case first :: second :: rest => println(first + second + rest.length) //
case _ => println("匹配不到...")
}
```

## 匹配嵌套结构

### 基本介绍

操作原理类似于正则表达式

### 应用案例

现在有一些商品，请使用Scala设计相关的样例类，完成商品捆绑打折出售。要求
商品捆绑可以是单个商品，也可以是多个商品。
打折时按照折扣x元进行设计.
能够统计出所有捆绑商品打折后的最终价格

创建样例类：

```scala
abstract class Item // 项

case class Book(description: String, price: Double) extends Item
//Bundle 捆 ， discount: Double 折扣 ， item: Item* , 
case class Bundle(description: String, discount: Double, item: Item*) extends Item
```

匹配嵌套结构（就是Bundle对象）

```
//给出案例表示有一捆数，单本漫画（40-10） +文学作品(两本书)（80+30-20） = 30 + 90 = 120.0
val sale = Bundle("书籍", 10,  Book("漫画", 40), Bundle("文学作品", 20, Book("《阳关》", 80), Book("《围城》", 30))) 
```

```scala
val sale = Bundle("书籍", 10,  Book("漫画", 40), Bundle("文学作品", 20, Book("《阳关》", 80), Book("《围城》", 30)))

val res = sale match  {
//如果我们进行对象匹配时，不想接受某些值，则使用_ 忽略即可，_* 表示所有
case Bundle(_, _, Book(desc, _), _*) => desc
}
```

通过@表示法将嵌套的值绑定到变量。_*绑定剩余Item到rest

```scala
val sale = Bundle("书籍", 10,  Book("漫画", 40), Bundle("文学作品", 20, Book("《阳关》", 80), Book("《围城》", 30)))
这个嵌套结构中的 "漫画" 和 紫色的部分 绑定到变量，即赋值到变量中.

val result2 = sale match {
case Bundle(_, _, art @ Book(_, _), rest @ _*) => (art, rest)
}
println(result2)
println("art =" + result2._1)
println("rest=" + result2._2)
```

不使用_*绑定剩余Item到rest

```scala
val sale = Bundle("书籍", 10,  Article("漫画", 40), Bundle("文学作品", 20, Article("《阳关》", 80), Article("《围城》", 30)))
这个嵌套结构中的 "漫画" 和 紫色的部分 绑定到变量，即赋值到变量中

val result2 = sale match {
//说明因为没有使用 _* 即明确说明没有多个Bundle,所以返回的rest，就不是WrappedArray了。
case Bundle(_, _, art @ Book(_, _), rest) => (art, rest)
}
println(result2)
println("art =" + result2._1)
println("rest=" + result2._2)
```

统计出所有捆绑商品打折后的最终价格

```
def price(it: Item): Double = {
  it match {
    case Book(_, p) => p
    //生成一个新的集合,_是将its中每个循环的元素传递到price中it中。递归操作,分析一个简单的流程
    case Bundle(_, disc, its @ _*) => its.map(price _).sum - disc
  }
}
```

## 密封类

### 基本介绍

如果想让case类的所有子类都必须在申明该类的相同的源文件中定义，可以将样例类的通用超类声明为**sealed**，这个超类称之为密封类。

**密封就是不能在其他文件中定义子类**。

### 应用实例

![image-20200501101956983](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200501101956983.png)

<img src="C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200501102009126.png" alt="image-20200501102009126" style="zoom:150%;" />

# 第十三章 函数式编程(高级)

## 偏函数

### 基本介绍

在对符合某个条件，而不是所有情况进行逻辑操作时，使用偏函数是一个不错的选择

将包在大括号内的一组**case语句封装为函数**，我们称之为偏函数，它只对会作用于指定类型的参数或指定范围值的参数实施计算，超出范围的值会忽略（未必会忽略，这取决于你打算怎样处理）

**偏函数在Scala中是一个特质PartialFunction**

**用case语句可以快速写一个偏函数**

### 应用案列

```scala
val list = List(1, 2, 3, 4, "abc")
//说明
val addOne3= new PartialFunction[Any, Int] {
def isDefinedAt(any: Any) = if (any.isInstanceOf[Int]) true else false
def apply(any: Any) = any.asInstanceOf[Int] + 1
}
val list3 = list.collect(addOne3)	//map函数不支持偏函数
println("list3=" + list3) //?
```

### 小结

1. 使用构建特质的实现类(使用的方式是PartialFunction的匿名子类)
2. **PartialFunction 是个特质**(看源码)
3. 构建偏函数时，参数形式   [Any, Int]是泛型，第一个表示参数类型，第二个表示返回参数
4. 当使用偏函数时，会遍历集合的所有元素，编译器执行流程时先执行isDefinedAt()如果为true ,就会执行 apply, 构建一个新的Int 对象返回
5. 执行isDefinedAt() 为false 就过滤掉这个元素，即不构建新的Int对象.
6. map函数不支持偏函数，因为map底层的机制就是所有循环遍历，无法过滤处理原来集合的元素
7. **collect函数支持偏函数**

### 化简形式

化简形式1

```
def f2: PartialFunction[Any, Int] = {
  case i: Int => i + 1 // case语句可以自动转换为偏函数
}
val list2 = List(1, 2, 3, 4,"ABC").collect(f2)
```

化简形式2

```
val list3 = List(1, 2, 3, 4,"ABC").collect{ case i: Int => i + 1 }
println(list3)
```

## 匿名函数

### 基本介绍

没有名字的函数就是匿名函数，可以通过函数表达式来设置匿名函数

### 应用实例

```
val triple = (x: Double) => 3 * x
println(triple(3))
(x: Double) => 3 * x 就是匿名函数 
(x: Double) 是形参列表， => 是规定语法表示后面是函数体， 3 * x 就是函数体，如果有多行，可以 {} 换行写.
triple 是指向匿名函数的变量。
```

## 高阶函数

### 基本介绍

能够接受函数作为参数的函数，叫做高阶函数 (higher-order function)。可使应用程序更加健壮

### 应用案例

```scala
//test 就是一个高阶函数，它可以接收f: Double => Double 
def test(f: Double => Double, n1: Double) = {
f(n1)
}
//sum 是接收一个Double,返回一个Double
def sum(d: Double): Double = {
d + d
}
val res = test(sum, 6.0)
println("res=" + res)
```

```scala
def minusxy(x: Int) = {
 (y: Int) => x – y //匿名函数
}
val result3 = minusxy(3)(5) 
println(result3)
```

### 小结

也可以分步执行: 

```scala
val f1 = minusxy(3) //此时f1获得x=3时的闭包
val res = f1(90)
```

## 参数类型推断

### 基本介绍

参数推断省去类型信息（在某些情况下[需要有应用场景]，参数类型是可以推断出来的，如list=(1,2,3) list.map()   map中函数参数类型是可以推断的)，同时也可以进行相应的简写。

### 基本写法

- 参数类型是可以推断时，可以省略参数类型
- 当传入的函数，只有单个参数时，可以省去括号
- 如果变量只在=>右边只出现一次，可以用_来代替

### 应用案列

```scala
val list = List(1, 2, 3, 4)
println(list.map((x:Int)=>x + 1)) //(2,3,4,5)
println(list.map((x)=>x + 1))
println(list.map(x=>x + 1))
println(list.map(_ + 1))
val res = list.reduce(_+_)
```

### 小结

当使用匿名函数的时候，参数类型可以被推断，只有一个参数时括号可以省略，方法体中只出现一次参数，可以用_代替。

## 闭包

### 基本介绍

闭包就是一个函数和与其相关的引用环境组合的一个整体(实体)。

### 应用案列

```scala
//1.用等价理解方式改写 2.对象属性理解
def minusxy(x: Int) = (y: Int) => x - y
//f函数就是闭包.
val f = minusxy(20) 		//f获得函数在20时的闭包
println("f(1)=" + f(1)) // 19，使用minusxy(20) 时的相关变量
println("f(2)=" + f(2)) // 18
```

### 小结

(y: Int) => x – y

返回的是一个匿名函数 ，因为该函数引用到到函数外的 x,那么  该函数和x整体形成一个闭包
如：这里 val f = minusxy(20) 的f函数就是闭包

当多次调用f时（可以理解多次调用闭包），发现使用的是同一个x, 所以x不变。

在使用闭包时，主要搞清楚返回函数引用了函数外的哪些变量，因为他们会组合成一个整体(实体),形成一个闭包

## 函数柯里化

### 基本介绍

函数编程中**，接受多个参数的函数**都可以转化为**接受单个参数的函数**，**这个转化过程就叫柯里化**

柯里化就是证明了函数只需要一个参数而已。其实我们刚才的学习过程中，已经涉及到了柯里化操作。

不用设立柯里化存在的意义这样的命题。柯里化就是以函数为主体这种思想发展的必然产生的结果。(即：**柯里化是面向函数思想的必然产生结果**)

### 快速入门

```scala
def mul(x: Int, y: Int) = x * y			//常规方式
println(mul(10, 10))

def mulCurry(x: Int) = (y: Int) => x * y //使用闭包
println(mulCurry(10)(9))

def mulCurry2(x: Int)(y:Int) = x * y 	//使用函数柯里化
println(mulCurry2(10)(8))
```

比较两个字符串在忽略大小写的情况下是否相等

```scala
方式1: 简单的方式,使用一个函数完成.

def eq2(s1: String)(s2: String): Boolean = {
s1.toLowerCase == s2.toLowerCase
}
```

```scala
//方式2：使用稍微高级的用法(隐式类)：形式为 str.方法()

def eq(s1: String, s2: String): Boolean = {
s1.equals(s2)}
implicit class TestEq(s: String) {
def checkEq(ss: String)(f: (String, String) => Boolean): Boolean = {
f(s.toLowerCase, ss.toLowerCase)
}}

//案例演示+说明+简化版(三种形式，直接传入匿名函数方式)
str1.checkEq(str2)(_.equals(_))
```

## 控制抽象

### 基本介绍

控制抽象是这样的函数，满足如下条件
1.参数是函数
2.函数参数没有输入值也没有返回值

### 快速入门

```scala
def myRunInThread(f1: () => Unit) = {
      new Thread {
        override def run(): Unit = {
          f1()
        }
      }.start()
    }
    myRunInThread {
      () => println("干活咯！5秒完成...")
        Thread.sleep(5000)
        println("干完咯！")
    }
```

### 简化处理

```scala
def myRunInThread(f1:  => Unit): Unit = {//说明 
new Thread {
	override def run(): Unit = {   
		f1   
   	 }
	}.start()
}
myRunInThread { //说明  
	println("干活咯！5秒完成...") 
    Thread.sleep(5000)  
    println("干完咯！")
    }
```

### 进阶用法

实现类似while的until函数

```scala
var x = 10
def until(condition: => Boolean)(block: => Unit): Unit = {
//类似while循环，递归
if (!condition) {
block
until(condition)(block)
}
//      println("x=" + x)
//      println(condition)
//      block
//      println("x=" + x)
}

until(x == 0) {
x -= 1
println("x=" + x)
}
```

# 第十四章 递归



## 编程范式

严格意义上的编程范式分为：命令式编程（Imperative Programming）、函数式编程（Functional Programming）和逻辑式编程（Logic Programming）。面向对象编程只是上述几种范式的一个交叉产物，更多的还是继承了命令式编程的基因。

在传统的语言设计中，只有命令式编程得到了强调，那就是程序员要**告诉计算机应该怎么做**。而递归则通过灵巧的函数定义，**告诉计算机做什么**。因此在使用命令式编程思维的程序中，是现在多数程序采用的编程方式，递归出镜的几率很少，而在函数式编程中，大家可以随处见到递归的方式。

## 应用实例

```scala
计算1-50的和
// 递归的方式来解决
def mx(num:BigInt,sum:BigInt):BigInt = {
if(num <= 99999999l) return mx(num+1,sum + num)
else return sum
}

//测试
var num = BigInt(1)
var sum = BigInt(0)
var res = mx(num,sum)
println("res=" + res)
```

```scala
求最大值
def max(xs: List[Int]): Int = {
    if (xs.isEmpty)
      throw new java.util.NoSuchElementException
    if (xs.size == 1)
      xs.head
    else if (xs.head > max(xs.tail)) xs.head else max(xs.tail)
  }
```

```
翻转字符串
//Str = "ab"
def reverse(xs: String): String =
    if (xs.length == 1) xs else reverse(xs.tail) + xs.head
```

## 注意事项

使用递归需要注意尽量避免重复的计算造成的数据冗余，经典的是斐波那契数

```scala
def digui(n : Int):Int={
	if(n == 1 && n == 2)
	1
	else
	digui(n-1)+digui(n-2)
}
```

该递归自身进行两次递归，而n-1的范围包含n-2的范围，造成了重复计算，思考如何优化？

# 项目开发流程

## 可行性分析

干什么？

1.有没有市场

谁来干？

2.市场部+销售部

出东西

## 需求分析-30%

1.需求分析师（懂技术+懂业务）挖掘用户的需求

2.需求分析报告（白皮书）

## 设计阶段-20%

1.项目经理（架构师）

2.使用什么技术+架构+选人

3.设计文档（类图，时序图，部署图，用例图）+数据库

4.界面(原型开发)

## 实现阶段-20%

1.软件工程师（码农）

2.看懂文档，实现各个模块

3.设计功能模块

4.模块代码

### 测试阶段-20%

1.测试工程师

2.测试用例，完成对软件的测试（白盒测试，黑盒测试，灰盒测试）

3.找bug，重现bug（重现bug）

### 实施阶段

1.实施工程师

2.将项目部署到系统并配置好参数，正确运行

## 维护阶段

1.不一定有专人

## 注意事项

写代码不是最重要的，最重要的是分析与设计！

# 客户信息项目

![image-20200503205528987](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200503205528987.png)

心得：

整个项目主要完成对类的CRUD，分为前台页面展示层->业务逻辑层->Bean层

设计顺序先从底层Bean的构建，在到前台客户列表的完成，在到增删改，主要的问题在于客户的查询功能，客户的id需要自增，将客户id设计成静态属性在伴生对象中定义，可读不可写，删除客户时的难点是先要根据id判断是否有这个客户，然后找到对象在列表的索引，用列表的indexOf解决，我这里使用了map映射。更新和查询是同一个问题，主要解决找到对象在列表的索引，因为只知道对象的id，indexOf就被限制了，我这里用map映射解决。查询功能提供id和关键字查找，所以需要对用户的输入进行判断是关键字还是数字，这里用到了正则表达判断，关键字查找在返回列表的时候，因为我用的是map映射返回，就会存在返回Unit的情况，在后续判断的时候会出现问题，所以这边改成用for循环判断，则返回的列表就不会有Unit。最后是序列换操作，系统开始和结束分别读取和保存对象到文件中。

# Akka介绍

## 基本介绍

Akka是编写并发程序的框架。

Akka主要解决的问题是：可以轻松的写出高效稳定的并发程序，程序员不再过多的考虑线程、锁和资源竞争等细节。

Akka类似消息队列

![image-20200504152743239](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200504152743239.png)

## 客服项目总结

各组件的理解：

​	一个JVM进程中只有一个ActorSystem（ActorFactory）即是单例的,要写一个Actor类需要继承Actor类，本地模式是要覆写receive方法，集群模式需要获取服务器（Master）的地址和端口号，并获取服务器的Arf，可通过context.actorSelection()方法，传入服务器的地址，注意需要指定服务器上的哪一个Actor,例如：

```scala
akka.tcp://yellowDuckService@${serverHost}:${serverPort}/user/${Actor}
```

ActorSystem创建时可以带config配置内容,例如：

```scala
val config = ConfigFactory.parseString(
    s"""
       |akka.actor.provider="akka.remote.RemoteActorRefProvider"
       |akka.remote.netty.tcp.hostname=$host
       |akka.remote.netty.tcp.port=$port
        """.stripMargin)
  private val client = ActorSystem("Client001",config)
```

服务器和客户端之间，可通过传递对象获取信息，即构造通信协议。客户端向服务器发送消息直接调用服务器的Arf，服务器回应客户端通过sender()方法。

具体流程：

clientArf ->发送ClientMessage() ->DispatcherMessage ->serverArf 的 MailBox -> 调用serverArf 的receive - > 调用sender()方法回应 -> 发送ServerMessage -> DispatcherMessage-> clientArf 的MailBox ->接受到serverArf的ServerMessage -> 调用clientArf 的receive ->进行分析   再次循环！

## spark Master Worker进程通讯项目

### 项目意义：

深入理解Spark的Master和Worker的通讯机制

为了方便同学们看Spark的底层源码，命名的方式和源码保持一致.(如： 通讯消息类命名就是一样的)

加深对主从服务心跳检测机制(HeartBeat)的理解，方便以后spark源码二次开发。

![image-20200505162921007](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200505162921007.png)

### 理解：

该项目和客服项目类似，只是他们之间的通讯协议有所改变，较复杂，协议创建了多个样例类对Worker发送来的id，cpu，ram进行包装成对象，项目提及了定时任务，是实现心跳机制的关键。

```scala
使用调度器定时任务
case StartTimeOutWorker => {
import context.dispatcher // 使用调度器时候必须导入dispatcher
context.system.scheduler.schedule(0 millis, 9000 millis, self, RemoveTimeOutWorker)
}
```

# 第十七章 设计模式

## 必要性

1. 面试会被问，所以必须学
2. 读源码时看到别人在用，尤其是一些框架大量使用到设计模式，不学看不懂源码为什么这样写，比如Runtime的单例模式.
3. 设计模式能让专业人之间交流方便
4. 提高代码的易维护
5. 设计模式是编程经验的总结，我的理解：即通用的编程应用场景的模式化，套路化（站在软件设计层面思考）。

## 掌握设计模式的层次

第1层：刚开始学编程不久，听说过什么是设计模式
第2层：有很长时间的编程经验，自己写了很多代码，其中用到了设计模式，但是自己却不知道
第3层：学习过了设计模式，发现自己已经在使用了，并且发现了一些新的模式挺好用的
第4层：阅读了很多别人写的源码和框架，在其中看到别人设计模式，并且能够领会设计模式的精妙和带来的好处。
第5层：代码写着写着，自己都没有意识到使用了设计模式，并且熟练的写了出来。

## 设计模式介绍

1.设计模式是程序员在面对同类软件工程设计问题所总结出来的有用的经验，模式【设计，思想】不是代码，而是某类问题的通用解决方案，设计模式（Design pattern）代表了最佳的实践。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。

2.设计模式的本质提高 软件的维护性，通用性和扩展性，并降低软件的复杂度【软件巨兽=》软件工程】。
3.<<设计模式>> 是经典的书，作者是 Erich Gamma、Richard Helm、Ralph Johnson 和 John Vlissides Design（俗称 “四人组 GOF”）

4.设计模式并不局限于某种语言，java，php，c++ 都有设计模式.

## 设计模式类型

**创建型模式**：==单例模式==、抽象工厂模式、建造者模式、==工厂模式==、原型模式。
**结构型模式**：适配器模式、桥接模式、==装饰模式==、组合模式、外观模式、享元模式、==代理模式==。
**行为型模式**：模版方法模式、命令模式、迭代器模式、==观察者模式==、中介者模式、备忘录模式、解释器模式（Interpreter模式）、状态模式、策略模式、职责链模式(责任链模式)、==访问者模式==。

## 简单工厂模式

简单工厂模式的设计方案: 定义一个实例化Pizaa对象的类，封装创建对象的代码

即类的创建交给工厂来做，既可以方便管理和扩展，又可以让创建对象有个缓冲层

## 工厂方法模式

工厂方法模式设计方案：将披萨项目的实例化功能抽象成抽象方法，在不同的口味点餐子类中具体实现。

工厂方法模式：定义了一个创建对象的抽象方法，由子类决定要实例化的类。工厂方法模式将对象的实例化推迟到子类。

## 抽象工厂模式

抽象工厂模式：定义了一个trait用于创建相关或有依赖关系的对象簇，而无需指明具体的类

抽象工厂模式可以将简单工厂模式和工厂方法模式进行整合。

从设计层面看，抽象工厂模式就是对简单工厂模式的改进(或者称为进一步的抽象)。

将工厂抽象成两层，AbsFactory(抽象工厂) 和 具体实现的工厂子类。程序员可以根据创建对象类型使用对应的工厂子类。这样将单个的简单工厂类变成了工厂簇，更利于代码的维护和扩展。

## 工厂模式小结

工厂模式的意义

> 将实例化对象的代码提取出来，放到一个类中统一管理和维护，达到和主项目的依赖关系的解耦。从而提高项目的扩展和维护性。
> 三种工厂模式 
> 设计模式的依赖抽象原则
>
> 创建对象实例时，不要直接 new 类, 而是把这个new 类的动作放在一个工厂的方法中，并返回。也有的书上说，变量不要直接持有具体类的引用。
> 不要让类继承具体类，而是继承抽象类或者是trait（接口）
> 不要覆盖基类中已经实现的方法。

## 单例模式

单例模式是指：保证在整个的软件系统中，某个类只能存在一个对象实例

Scala中没有静态的概念，所以为了实现Java中单例模式的功能，可以直接采用类对象(即伴生对象)方式构建单例对象

### 单例模式之懒汉式:

```scala
object TestSingleTon extends App{
  val singleTon = SingleTon.getInstance
  val singleTon2 = SingleTon.getInstance
  println(singleTon.hashCode() + " " + singleTon2.hashCode())
}
//将SingleTon的构造方法私有化
class SingleTon private() {}

object SingleTon  {
  private var s:SingleTon = null   //这句说明是懒汉式
  def getInstance = {
    if(s == null) {
      s = new SingleTon
    }
    s}}
```

### 单例模式之饿汉式:

```scala
object TestSingleTon extends App {
  val singleTon = SingleTon.getInstance
  val singleTon2 = SingleTon.getInstance
  println(singleTon.hashCode() + " ~ " + singleTon2.hashCode())
  println(singleTon == singleTon2)
}
//将SingleTon的构造方法私有化
class SingleTon private() {
  println("~~~")
}
object SingleTon {
  private val s: SingleTon = new SingleTon  //这句说明是饿汉式
  def getInstance = {
    s}}
```

## 装饰者模式

### 装饰者模式原理

1. 装饰者模式就像打包一个快递
    主体：比如：陶瓷、衣服 (Component)
    包装：比如：报纸填充、塑料泡沫、纸板、木板(Decorator)
2. Component主体：比如类似前面的Drink

3. ConcreteComponent和DecoratorConcreteComponent：具体的主体，比如前面的各个单品咖啡Decorator: 装饰者，比如各调料.
4. 在如图的Component与ConcreteComponent之间，如果ConcreteComponent类很多,还可以设计一个缓冲层，将共有的部分提取出来，抽象层一个类。

### 定义

1. 装饰者模式：动态的将新功能==附加到对象上==。在对象功能扩展方面，它比继承更有弹性，装饰者模式也体现了开闭原则(ocp)
2. 这里提到的==动态的将新功能附加到对象和ocp原则==，在后面的应用实例上会以代码的形式体现，请同学们注意体会。

![image-20200505194014880](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200505194014880.png)

![image-20200505194031500](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200505194031500.png)

## 观察者模式

### 观察者模式原理

观察者模式类似订牛奶业务

奶站/气象局：Subject

用户/第三方网站：Observer

Subject：登记注册、移除和通知

registerObserver 注册
removeObserver 移除
notifyObservers() 通知所有的注册的用户，根据不同需求，可以是更新数据，让用户来取，也可能是实施推送，看具体需求定

Observer：接收输入

观察者模式：对象之间多对一依赖的一种设计方案，被依赖的对象为Subject，依赖的对象为Observer，Subject通知Observer变化,比如这里的奶站是Subject，是1的一方。用户时Observer，是多的一方。

```
trait Subject {
  
  def registerObserver(o: ObServer)
  def removeObserver(o: ObServer)
  def notifyObservers()
}
trait ObServer {
  def update(mTemperatrue: Float, mPressure: Float, mHumidity: Float)
}
```

具体方法由子类实现。

## 代理模式

代码模式的基本介绍

代理模式：为一个对象提供一个替身，以控制对这个对象的访问
被代理的对象可以是远程对象、创建开销大的对象或需要安全控制的对象
代理模式有不同的形式(比如 远程代理，静态代理，动态代理)，都是为了控制与管理对象访问。

### 代理类型：

==远程代理==：使用RMI（Remote Machine Invocation）它是一种机制，能够让在某个 Java虚拟机上的对象调用另一个 Java 虚拟机中的对象上的方法。可以用此方法调用的任何对象必须实现该远程接口，RMI可以将底层的socket编程封装，简化操作。

![image-20200505214617333](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200505214617333.png)

==动态代理==：运行时动态的创建代理类(对象)，==并将方法调用转发到指定类(对象)==

将真实的对象交给代理去管理，外部的类通过访问代理来调用真实的类的方法，可以进行权限控制

动态代理类需要继承InvocationHandler 并覆写invoke方法。

**真实类需要通过代理来创建或者返回**

```scala
def getOwnerProxy(person: PersonBean): PersonBean = {
   Proxy.newProxyInstance(person.getClass().getClassLoader(), 			person.getClass().getInterfaces(), new OwnerInvocationHandler(person)).asInstanceOf[PersonBean]
  }
```

![image-20200505214716566](C:\Users\甘正\AppData\Roaming\Typora\typora-user-images\image-20200505214716566.png)

==保护代理==：

通过前面的分析：大家可以看出动态代理其实就体现出保护代理，即代理时，对被代理的对象（类）的哪些方法可以调用，哪些方法不能调用在InvocationHandler可以控制。因此动态代理就体现(实现)了保护代理的效果

几种常见的代理变体：

1. **防火墙代理**：内网通过代理穿透防火墙，实现对公网的访问。
2. **缓存代理**，比如：当请求图片文件等资源时，先到缓存代理取，如果取到资源则ok,如果取不到资源，再到公网或者数据库取，然后缓存。
3. **静态代理**，静态代理通常用于对原有业务逻辑的扩充。比如持有第二方包的某个类，并调用了其中的某些方法。比如记录日志、打印工作等。可以创建一个代理类实现和第二方方法相同的方法，通过让代理类持有真实对象，调用代理类方法，来达到增加业务逻辑的目的。
4. **Cglib代理**：使用cglib[Code Generation Library]**实现动态代理**，**并不要求委托类必须实现接口**，底层采用asm字节码生成框架生成代理类的字节码。
5. **同步代理**：主要使用在多线程编程中，完成多线程间同步工作

# 第十八章 泛型边界

## 泛型

### 基本介绍

1. 如果我们要求函数的参数可以接受任意类型。可以使用泛型，这个类型可以代表任意的数据类型。
2. 例如 List，在创建 List 时，可以传入整型、字符串、浮点数等等任意类型。那是因为 List 在 类定义时引用了泛型。比如在Java中：public interface List<E> extends Collection<E>

## 枚举

```
// Scala 枚举类型
object SeasonEm extends Enumeration {
  type SeasonEm = Value //自定义SeasonEm，是Value类型,这样才能使用
  val spring, summer, winter, autumn = Value
}
```

## 上界(Upper Bounds)介绍和使用

### java中上界

在 Java 泛型里表示某个类型是 A 类型的子类型，使用 extends 关键字，这种形式叫 upper bounds(上限或上界)，语法如下：

```
<T extends A>
//或用通配符的形式：
<? extends A>
```

### scala中上界

在 scala 里表示某个类型是 A 类型的子类型，也称上界或上限，使用 <: 关键字，语法如下：

```
[T <: A]
//或用通配符:
[_ <: A]
```

## 下界(Lower Bounds)介绍和使用

### Java中下界

在 Java 泛型里表示某个类型是 A类型的父类型，使用 super 关键字

```
<T super A>
//或用通配符的形式：
<? super A>
```

### scala中下界

在 scala 的下界或下限，使用 >: 关键字，语法如下：

```
[T >: A]
//或用通配符:
[_ >: A]
```

### 使用小结

```scala
def biophony[T >: Animal](things: Seq[T]) = things
```

1. 对于下界，可以传入任意类型
2. 传入和Animal直系的，是Animal父类的还是父类处理，是Animal子类的按照Animal处理
3. 和Animal无关的，一律按照Object处理
4. 也就是下界，可以随便传，只是处理是方式不一样
5. 不能使用上界的思路来类推下界的含义

## 视图界定基本介绍

<% 的意思是“view bounds”(视界)，它比<:适用的范围更广，除了所有的子类型，还允许隐式转换类型。

```
def method [A <% B](arglist): R = ... 等价于:
def method [A](arglist)(implicit viewAB: A => B): R = ... 
或等价于:
implicit def conver(a:A): B = …
```

<% 除了方法使用之外，class 声明类型参数时也可使用：
`class A[T <% Int]`

<% 视图界定 view bounds ，会发生隐式转换

## 类型约束-上下文界定(Context bounds)

### 基本介绍

与 view bounds 一样 context bounds(上下文界定)也是隐式参数的语法糖。为语法上的方便， 引入了”上下文界定”这个概念

```scala
def geatter = {
    //这句话就是会发生隐式转换，获取到隐式值 personComparetor，隐式值要自行赋值
    val comparetor = implicitly[Ordering[T]]
    println("CompareComm6 comparetor" + comparetor.hashCode())
    if(comparetor.compare(o1, o2) > 0) o1 else o2
  }
```

## 协变、逆变和不变

在这里引入关于这个符号的说明，在声明Scala的泛型类型时，“+”表示协变，而“-”表示逆变 
C[+T]：如果A是B的子类，那么C[A]是C[B]的子类，称为协变 
C[-T]：如果A是B的子类，那么C[B]是C[A]的子类，称为逆变 
C[T]：无论A和B是什么关系，C[A]和C[B]没有从属关系。称为不变.