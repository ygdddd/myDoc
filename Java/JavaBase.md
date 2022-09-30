# Java 基础

J2SE：Java SE，支持桌面级应用的Java平台，提供核心API
J2EE：Java EE，面向开发企业的解决方案，针对Web应用开发，包含如Servlet，JSP等
J2ME：Java ME，支持Java在移动端运行的平台，精简API并加入针对移动端的支持

## Java 特性

1. 面向对象（OOP）；
2. 健壮性，强类型语言，包括强类型机制、异常处理、垃圾回收机制保障；
3. 跨平台，Java代码无关平台，通过JVM、JRE来满足不同平台运行要求；
4. Java是解释性语言，即不能直接运行，需要解释器执行；（C/C++可以直接被机器执行）。

### Java 跨平台运行

1. JVM Java virtual machine Java虚拟机，具有指令集并使用不同的存储区域，负责执行指令，管理数据、内存、寄存器，包含在JDK中；
2. 不同平台，有不同的虚拟机；
3. 编译：Javac（Java complier）将.java编译成.class（JVM识别的字节码文件），再用Java将.class装载到JVM执行；
4. JVM屏蔽了不同平台的执行机制，实现跨平台。

``` 
JDK(Java Development Kit)：单独给开发人员使用的工具，包括JRE；
JRE(Java Runtime Environmen)：包括JVM和核心类库，运行程序只需要JRE；
JDK = JRE + Java开发工具；
JRE = JVM + Java核心类库；
```

### Java文件中的Class

1. 一个源文件最多一个public类，其他类数量不限，其他类中也可以运行main方法；
2. 编译时一个类对应一个.class文件；
3. 如果Java文件包含public类，则文件名必须和public类名相同。

### 变量与数据类型

概念：内存开辟一块空间来存储变量的值，并使用一个索引来指向这块空间；
该区域有自己的索引【变量名】和数据类型

- Java是强类型语言，需要显式指定数据类型

#### Java中基本数据类型占用空间大小

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

- 1 Byte = 8 bits， long数据的值后要带`l`或者`L`。