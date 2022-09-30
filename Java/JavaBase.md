# Java 基础

J2SE：Java SE，支持桌面级应用的Java平台，提供核心API
J2EE：Java EE，面向开发企业的解决方案，针对Web应用开发，包含如Servlet，JSP等
J2ME：Java ME，支持Java在移动端运行的平台，精简API并加入针对移动端的支持

## Java 特性
1. 面向对象（OOP）；
2. 健壮性，强类型语言，包括强类型机制、异常处理、垃圾回收机制保障；
3. 跨平台，Java代码无关平台，通过JVM、JRE来满足不同平台运行要求；
4. Java是解释性语言，即不能直接运行，需要解释器执行；（C/C++可以直接被机器执行）

### Java 跨平台运行
1. JVM Java virtual machine Java虚拟机，具有指令集并使用不同的存储区域，负责执行指令，管理数据、内存、寄存器，包含在JDK中；
2. 不同平台，有不同的虚拟机，
3. 编译：Javac（Java complier）将.java编译成.class（JVM识别的字节码文件），再用Java将.class装载到JVM执行；
4. JVM屏蔽了不同平台的执行机制，实现跨平台。

``` 
JDK(Java Development Kit)：单独给开发人员使用的工具，包括JRE
JRE(Java Runtime Environmen)：包括JVM和核心类库，运行程序只需要JRE
JDK = JRE + Java开发工具
JRE = JVM + Java核心类库
```


