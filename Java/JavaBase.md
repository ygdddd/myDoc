# Java 基础

J2SE：Java SE，支持桌面级应用的Java平台，提供核心API
J2EE：Java EE，面向开发企业的解决方案，针对Web应用开发，包含如Servlet，JSP等
J2ME：Java ME，支持Java在移动端运行的平台，精简API并加入针对移动端的支持

## Java 特性

1. 面向对象（OOP）
2. 健壮性，强类型语言，包括强类型机制、异常处理、垃圾回收机制保障
3. 跨平台，Java代码无关平台，通过JVM、JRE来满足不同平台运行要求
4. Java是解释性语言，即不能直接运行，需要解释器执行；（C/C++可以直接被机器执行）

### Java 跨平台运行

1. JVM Java virtual machine Java虚拟机，具有指令集并使用不同的存储区域，负责执行指令，管理数据、内存、寄存器，包含在JDK中
2. 不同平台，有不同的虚拟机
3. 编译：Javac（Java complier）将.java编译成.class（JVM识别的字节码文件），再用Java将.class装载到JVM执行
4. JVM屏蔽了不同平台的执行机制，实现跨平台


- JDK(Java Development Kit)：单独给开发人员使用的工具，包括JRE
- JRE(Java Runtime Environmen)：包括JVM和核心类库，运行程序只需要JRE
- JDK = JRE + Java开发工具
- JRE = JVM + Java核心类库

JVM结构：JVM包含栈、堆、方法区

### Java文件中的Class

1. 一个源文件最多一个public类，其他类数量不限，其他类中也可以运行main方法
2. 编译时一个类对应一个.class文件
3. 如果Java文件包含public类，则文件名必须和public类名相同

API：Application Programming Interface
- [中文API参考文档](https://www.matools.com/api)

## 变量与数据类型

概念：内存开辟一块空间来存储变量的值，并使用一个索引来指向这块空间
该区域有自己的索引【变量名】和数据类型

### Java中基本数据类型占用空间大小

|   TYPE  |    SIZE   |      RANGE      |  BOX TYPE |
|:-------:|:---------:|:---------------:|:---------:|
| byte    | 1 Byte    | -2^7 ~ 2^7-1    | Byte      |
| short   | 2 Byte    | -2^15 ~ 2^15-1  | Short     |
| int     | 4 Byte    | -2^31 ~ 2^31-1  | Integer   |
| long    | 8 Byte    | -2^63 ~ 2^63-1  | Long      |
| float   | 4 Byte    |                 | Float     |
| double  | 8 Byte    |                 | Double    |
| char    | 2 Byte    |                 | Character |
| boolean | 1or4 Byte | true `OR` false | Boolean   |

- Java是强类型语言，需要显式指定数据类型
- 1 Byte = 8 bits
- long数据的值后要带`l`或者`L`
- float数据的值后要带`f`或者`F`
- boolean不可以用0或非0代替
- char的本质是整数(码值)，输出对应的unicode字符
- 常见编码(英｜汉字节)：ASCII，unicode(2｜2)，utf-8(1｜3），gbk(1｜2)
- [unicode在线转换](http://tool.chinaz.com/Tools/Unicode.aspx)
- （char和short）和byte不会自动转换，计算时总是转换为int

### String和基本数据类型

String-基本类型： (X类型)包装类.parseX(String)
基本类型-String： BaseType + ""

## 运算符

算术运算符：+ - * / % ++ --
- 除以的时候，右边先运算，再赋值到左边
- 取模公式：a % b = a - a / b * b

关系运算符：== != < > <= >= instanceof

逻辑运算符：& | && || ! ^ （返回值都为boolean）
- 短路与&& 逻辑与&：短路与判断时，第一个为false则直接返回；逻辑与必须全部判断完
- 短路或|| 逻辑或|：短路或判断时，第一个为true则直接返回；逻辑或必须全部判断完
- 逻辑异或^：相同时false，不同时true

三元运算符： 条件 ? a : b
- 条件为true结果为a， 条件为false结果b

赋值运算符：= += -= *= /= %=
- a += b -> a = a + b
- 复合赋值运算符会自动进行类型转换

## 标识符

变量、方法、类等命名时使用的字符序列为标识符（所有自己命名的）
- 由字母、数字、_或$组成
- 数字不能开头
- 不能使用关键字/保留字，但可以包含（关键字：public, class, static...）

### 标识符规范

Package: 多单词组成时所有字母小写

Class & Interface: 所有单词首字母大写（大驼峰）

Variable & Method: 第一个单词首字母小写，其余首字母大写（小驼峰）

Constant: 所有字母大写，_连接

### 键盘输入

``` Java
//使用java.util.Scanner
//一个使用正则表达式解析基本类型和字符串的文本扫描器
import java.util.Scanner;

Scanner inputScanner = new Scanner(System.in);
int number = inputScanner.nextInt();
String str = inputScanner.next();
```

## 进制

二进制：0，1，开头0B，0b
- 二进制转八进制：先在高位补0直到位数是3的倍数，每`3`位转换一位八进制数字
- 二进制转十六进制：先在高位补0直到位数是4的倍数，每`4`位转换一位十六进制数字
- 八进制转二进制：每位转换3个二进制数字
- 十六进制转二进制：每位转换4个二进制数字

八进制：0-8，开头0 

十六进制：0-9，a-f，0x或0X开头

十进制：0-9
- N进制转十进制：每位数字乘以每位的次方值，再相加。
- 十进制转N进制：不断除以`n`，直到商为0，然后将每步的余数倒过来。

### 位运算

1. 原码：有符号二进制码，最高位为符号位，正数0，负数1
2. 反码：正数反码等于原码，负数的反码 = 除符号位以外，原码按位取反
3. 补码：正数补码等于原码，负数的补码 = 反码 + 1

- 0的反码，补码都为0
- Java没有无符号数
- 计算机运行的时候，都是以补码的方式来运算的，结果再以原码的方式展示
- 原->补（统一计算符号），补->原（还原符号）

位运算符：&，|，^，~, >>, <<, >>>

- 算术右移`>>`:低位溢出，符号位不变，高位补符号位（/2）
- 算术左移`<<`:符号位不变，低位补0（*2）
- 逻辑（无符号）右移`>>>`:低位溢出，高位补0

## 分支控制

1. 顺序控制：先定义后引用，不涉及判断
2. 分支控制：if if-else else
3. switch：switch(表达式) case(值): break default
    - case执行完不会主动退出，如果没有break，会跳过下一个case的判断，穿透执行下一个case的语句，直到break
    - 表达式和case值的类型应该相同，或允许自动转换后比较
    - 表达式返回值必须是：byte short int char enum String中的一种
    - case值只能是常量
4. 循环控制：for(初始化循环变量; 循环条件; 循环变量迭代) { 循环操作; }
    - 循环变量迭代在循环操作之后执行
    - 可以初始化、迭代多个循环变量，也可以在循环外执行

5. 循环控制：(初始化;) while(循环条件){循环操作; 循环变量迭代;}

6. 循环控制：(初始化;) do{循环操作; 循环变量迭代;}while(循环条件);
    - 循环条件放在最后，即至少会执行一次

7. 多重循环控制：for / while / do while

8. break可以终止某个循环
    - 可以通过标签来指定终止哪一层循环，若没有标签则默认退出最近一层循环。
    - 尽量不使用标签，会影响可读性
    ```Java
    //在{}前加上标签名
    label1: 
    for(;;){
        label2:
        for(;;){
            if(){
                break label1;
            }
        }
    }
    ```

9. continue跳过此次循环，执行下一次循环
    - 可以通过标签指定跳过哪一层的循环，若没有标签则默认跳过最近一层循环。
    - 尽量不使用标签，影响可读性


10. return跳出当前方法（main方法的return直接退出程序）

## 数组Array

数组的定义：引用类型，可以存放多个同一类型的数据

数组的长度：array.length

数组中元素可以指定为任何类型，包括基本类型和引用类型，不能混用

数组属于引用类型，数组型数据是对象。

直接赋值整个数组在默认情况下是引用传递

数组创建时元素默认值：
| type | default |
|:----:|:-------:|
| int | 0|
| short | 0 |
| byte | 0 |
| long | 0 |
| float | 0.0 |
| double| 0.0 |
| char| \u0000 |
| boolean | false |
|String | null|

```Java
    //定义数组的多种方式，下标从0开始
    //type[] arr 等价 type arr[]
    type[] arr1 = {1, 2, 3, 5}; //静态初始化，直接填充数据元素
    
    //等价于先声明，再分配
    type[] arr2 = new type[len];//动态初始化，长度为len
```

### 二维数组

相当于一维数组的每个元素也是一维数组，使用两层下标访问特定元素。

共三种声明方式
- type[][] name
- type name[][]
- type[] name[]

```Java
    type[][] _2Darr1 = {{1}, {2,3}, {1,3,5}}; //静态初始化，直接填充数据元素
    
    type[][] _2Darr2 = new type[amount][size];//动态初始化，有amount个一维数组，每个一维数组的大小为size
```
## 排序

内部排序：将所有需要处理的数据都加载到内存中进行排序
- 交换排序
- 选择排序
- 插入排序

外部排序：数据量过大导致无法全部加载到内存中，需要借助外部存储进行排序
- 合并排序
- 直接合并排序

## 查找

顺序查找：

二分查找：

# 面向对象 Object Oriented Programming

