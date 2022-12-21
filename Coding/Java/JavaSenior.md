# Java高级

## Static关键字

### 类变量（静态变量）

在类定义时使用static修饰的变量，该变量会此类所有的实例对象共享，任何一个该类的对象访问/修改此变量时，操作的都是同一个变量。定义：「modifier static type name」
> 需要让某个类的所有对象共享一个变量时使用static类变量
- 静态变量在类加载的时候生成，不创建对象实例也能访问
- 静态变量可通过类名直接访问「class.staticVar」或者「obj.staticVar」
- 静态变量定义后会单独存放在一个堆空间（>JDK8）或者（<=JDK8）方法区中存在静态域。 
- 静态变量的生命周期从类加载开始，类销毁而结束

### 类方法（静态方法）

和类变量类似，定义方法时加入static关键字。
> 当方法中不涉及到任何和对象相关的属性，则可以将方法设计为静态方法。
> - Math类，Arrays类，Collection类等工具类
- 静态方法和普通方法都是随着类加载而加载，将结构信息存储在方法区
- 静态方法无this的参数，普通方法隐含this的参数
- 静态方法可以通过类名和对象名来调用，普通方法只能通过对象调用
- 静态方法不允许使用和对象相关的关键字（如this和super）
- 静态方法只能访问静态变量或静态方法，普通方法可以访问成员变量和静态变量

### main方法解析

「public static void main(String[] args) 」
1. main方法被JVM调用，所以访问权限需要是public
2. JVM在执行main方法时，不必创建对象，所以需要是static方法
3. 使用args来接收执行Java命令时传递的String类型参数
4. 遵守静态方法访问规则，可以直接访问main方法所在类的静态方法或静态属性

### 代码块

代码化块又称为初始化块，属于类的成员，与方法类似，将逻辑语句封装在代码体中，通过{}包围起来。「modifier {//code};」
- 与方法不同，没有方法名，参数和返回值，只有方法体，方法体无限制
- 不用通过对象或类显式调用，而是加载类时或创建对象时隐式调用
- 修饰符只有默认或静态两种，带static的为静态代码块，默认的为普通代码块
- 末尾的分号可以写上或省略
---
1. 代码块相当于另外一种形式的构造器，可以做初始化操作
3. 代码块的顺序优先于构造器，不管调用哪个构造器都会先调用代码块
2. 场景：如果多个构造器中都有重复的语句，可以抽取到初始化块中，提高重用性
---
1. 静态代码块的作用是对类进行初始化，随着类的加载而执行且只执行一次；普通代码块在每个对象创建时都执行一次
2. 类加载的场景：
    1. 创建对象实例时（new）
    2. 创建子类对象实例时，父类也会加载
    3. 使用类的静态成员时
3. 普通代码块在创建对象时会被隐式调用，使用类的静态成员时不会执行普通代码块
4. 创建对象时，在类中调用顺序是：
    1. 调用静态代码块和静态属性初始化（静态代码块和属性初始化优先级相同，存在多个时按照定义顺序加载）
    2. 调用普通代码块和普通属性初始化（普通代码块和属性初始化优先级相同，存在多个时按照定义顺序加载）
    3. 调用构造方法
5. 构造方法的最前面隐含了super()和调用普通代码块。
    ```Java
    class A {
        public A{
            super();//隐藏
            //隐式调用普通代码块
            //其他语句
        }
    }
    ```
6. 创建子类对象时，执行顺序：
    1. 父类静态代码块和静态属性
    2. 子类静态代码块和静态属性
    3. 父类普通代码块和普通属性初始化
    4. 父类的构造方法
    5. 子类普通代码块和普通属性初始化
    6. 子类的构造方法
7. 静态代码块只能调用静态成员

### 单例模式

单例模式就是采用一定方法在整个软件系统中，对某个类只能存在一个对象实例，并且该类只提供一个取得该实例的方法

饿汉式：只要类加载，单例对象就创建。可能导致创建了对象但是没有使用的资源浪费。
1. 构造器私有化：禁止通过new创建对象
2. 类的内部创建对象
3. 向外暴露静态的公共方法（getInstance）
经典类：Java.lang.Runtime

```Java
    class SingleTon01{
        private SingleTon01(){}

        private static SingleTon01 instance = new SingleTon01();

        public static SingleTon01 getInstance(){
            return instance;
        }
    }
```

懒汉式：用户使用getInstance时才创建唯一对象，避免资源的浪费

1. 构造器私有化：禁止通过new创建对象
2. 类的内部创建对象引用，加载时不创建对象
3. 向外暴露静态的公共方法（getInstance），调用方法时再创建
---
风险：懒汉式单例模式存在线程安全问题，等待创建对象时，可能多个线程都判断为null
```Java
    class SingleTon02{
        private SingleTon02(){}

        private static SingleTon02 instance;

        public static SingleTon02 getInstance(){
            if(instance == null) instance = = new SingleTon02();
            return instance;
        }
    }
```

## final关键字

final修饰类、属性、方法、局部变量
final修饰的情况：
1. 不希望类被继承时
2. 不希望父类的某个方法被子类重写
3. 不希望类的某个属性的值被修改
4. 不希望某个局部变量被修改
---
- final修饰的属性被称为常量，一般用「XX_XX」命名；定义时必须赋值且之后不能再修改
- 赋值位置：
    1. 定义时
    2. 构造器中
    3. 代码块中
- final修饰静态属性时，初始化只能定义时赋值或者静态代码块中赋值
- 如果一个类是final类，就没有必要将方法再修饰成final方法
- final不能修饰构造器
- final和static往往搭配使用，效率更高，不会导致类加载

## 抽象类
用abstract关键字修饰的方法为抽象方法，抽象方法没有方法体，往往不确定具体实现的方法为抽象方法；
包含抽象方法的类也需要abstract关键字修饰，为抽象类。
```Java
abstract class A(){
    modifier abstract type name();
}
```
抽象类的价值在于设计，是设计者设计好后，让子类继承并实现抽象类（在设计模式和框架中使用较多）
---
1. 抽象类不能被实例化。
2. 抽象类可以没有abstract方法，可以有实现的方法。
3. 一旦类包含了abstract方法，这个类必须声明为abstract
4. abstract只能修饰类和方法
5. 抽象类仍属于类，可以有任意成员
6. 抽象方法不能有方法体，不能有具体实现
7. 如果抽象类被继承，则子类必须实现所有抽象方法，除非子类也声明为抽象方法
8. 抽象方法不能被与重写冲突的关键字修饰【private，final，static】

---
### 模板模式
一个抽象类公开定义了执行它的方法的方式/模板。它的子类可以按需要重写方法实现，但调用将以抽象类中定义的方式进行。

意图：定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

主要解决：一些方法通用，却在每一个子类都重新写了这一方法。

## Interface接口

接口将一些没有实现的方法封装到一起，某个类需要使用某个方法时，再根据情况实现这些方法。
> JDK7.0以前，接口里只能有抽象方法
> JDK8.0以后，接口可以包含静态和默认方法（使用default关键字）

```Java
interface name{
    //attr
    //method
}
class Name implements name{
    //privatr attr
    //private method
    //override interface method
}
```
---
1. 接口是抽象概念，接口不能被实例化，接口修饰符只能是public和default
2. 接口中所有成员都是`public`方法，接口中抽象方法可以省略`abstract`关键字。
3. 普通类implements接口时需要实现类中全部抽象方法，抽象类实现接口则不用
4. 一个类可以同时实现多个接口，多个接口的所有方法都需要实现
5. 接口的属性默认且只能是final的（public static final）「interfaceName.attr」
6. 接口不能继承类，但是可以继承多个其他接口

---
实现接口 VS 继承类
> 接口和继承解决的问题不同
- 继承：解决代码的复用性和可维护性
- 接口：设计好的各种规范/方法，让其他类实现

> 实现机制是Java单继承机制的一种补充
- 子类继承父类，就自动拥有了父类的功能
- 子类需要扩展功能，可以通过实现接口的方式

> 接口比继承更灵活
- 继承 -> is-a
- 接口 -> like-a

> 接口一定程度上解耦
- 接口规范性 + 动态绑定

### 接口的多态
> 和继承多态类似
1. 多态参数：接口作为引用，可以指向实现类的实例对象
2. 多态数组：接口类型数组可以存放实现类的实例对象
3. 多态传递：接口A的实现类可以用该接口的父接口B来引用【接口A entends 接口B，C实现A】

## 内部类

一个类的内部完整嵌套了另一个类结构，被嵌套的类被称为内部类，嵌套其他类的类为外部类；内部类最大的特点就是可以直接访问私有属性，并可以体现类与类的包含关系。
> 类的五大成员：属性，方法，构造器，代码块，内部类
```Java
class Outer{
    class Inner{}
}
class Other{}
```

四种内部类：
1. 定义在外部类局部位置（如方法体）：
    1. 局部内部类（有类名）
    2. 匿名内部类（无类名）
2. 定义在外部类的成员位置：
    1. 成员内部类（无static）
    2. 静态内部类（static）

### 局部内部类
> 本质仍然是类，定义在外部类的局部位置，通常在方法中，而且有类名

1. 可以访问外部类所有成员，包括私有成员
2. 地位等同于局部变量，不能用`final`以外的修饰符修饰，作用域仅在定义它的方法或代码块中，不能被外部的其他类访问
3. 局部内部类访问外部类成员可以直接访问，外部类访问局部内部类的成员需要先创建对象
4. 外部类和局部内部类的成员重名时，默认遵守就近原则【使用「Outer.this.attr」来访问外部类成员】

### 匿名内部类
> 本质仍然是类，定义在外部类的局部位置，并且没有类名，同时也是一个对象
jdk底层会自动给匿名内部类分配类名，且创建类之后会立即创建实例，并返回地址，该类使用一次就不能再使用
new后如果是接口，底层默认implements，如果是类，底层默认extends。
匿名内部类可以当作实参传递，简洁高效（本质是对象）。
```Java
    ClassOrInterface name = new classOrinterface(){
        //类名为"外部类+$n"
        //body
    };
```
1. 匿名内部类的语法既是一个类的定义，又是创建一个对象，可以用类/对象两种方法访问成员
2. 可以访问外部类包括私有成员在内的所有成员
3. 地位等同于局部变量，不能用`final`以外的修饰符修饰，仅在定义时使用
4. 外部类和局部内部类的成员重名时，默认遵守就近原则【使用「Outer.this.attr」来访问外部类成员】

### 成员内部类
定义在外部类的成员位置，且没有static修饰
```Java
    class Out{
        int n1;
        String name;
        class Inner{
            void method(){}
        }
        Inner getInner(){
            return new Inner();
        }
    }
    //外部其他类使用内部类
    class A{
        Out.Inner inner = Out.new Inner();//1
        inner2 = new Out().getInner();//2
        new Out().new Inner();//3
    }
    
```
1. 可以访问外部类包括私有成员在内的所有成员，外部类也能访问内部类的私有属性
2. 可以添加访问修饰符，地位等同于成员，可以被外部其他类访问，作用域为整个类体
3. 成员内部类直接访问外部类的成员，外部类先创建对象再访问内部类成员
4. 外部类和局部内部类的成员重名时，默认遵守就近原则【使用「Outer.this.attr」来访问外部类成员】

### 静态内部类
有static修饰的成员内部类
```Java
    class Out{
        int n1;
        String name;
        static class Inner{
            void method(){}
        }
        public (static) Inner getInner(){
            return new Inner();
        }
    }
    //外部其他类使用内部类
    class A{
        Out.Inner inner = new Out.Inner();//静态内部类可以通过类名直接访问
        inner2 = new Out().getInner();//方法返回
        Out.getInner();//使用静态方法直接返回
    }
    
```
1. 可以访问外部类所有静态成员，包括私有的，不能访问非静态成员
2. 可以添加访问修饰符，地位等同于成员，可以被外部其他类访问，作用域为整个类体
3. 静态内部类直接访问外部类的静态成员，外部类先创建对象再访问静态内部类成员
4. 外部类和局部内部类的成员重名时，默认遵守就近原则【使用「Outer.this.attr」来访问外部类成员】

## 枚举enum和注解

枚举（enumeration，enum）是一组常量的组合，属于一种特殊的类，里面只包含一组有限的特定对象。
- 自定义实现枚举
    1. 将构造器私有化（防止直接new）
    2. 删除set方法，可以保留get方法
    3. 在类内部直接创建固定的public static final对象
    4. 枚举对象名通常使用全部大写（常量的命名规范）
---
- 使用enum关键字实现枚举
    1. 使用`enum`替代`class`，此时会默认继承`Enum`类，不能再继承其他类，但是可以实现接口
    2. 直接使用「常量名(参数)」，参数列表需要和某个构造器对应，如果对应无参构造器，小括号都可以省略
    3. 如果有多个常量，使用`逗号,`间隔，最后分号结尾
    4. 常量对象需要在定义的前面


注解（Annotation）也被称为元数据（Metadata）用于修饰/解释包，类，方法，属性，构造器，局部变量等信息。
- 和注释一样不影响程序逻辑，单注解可以被编译或运行，相当于嵌入代码的补充信息
- 注解前面有@符号，并将该注解当作修饰符使用
- @interface不是接口，表示一个注解类
- 修饰注解的注解为元注解

基本注解：
@Override：限定某个方法是重写父类方法，只能用于方法
- 编译器会检查带`@Override`注解的方法是否重写了父类的方法，如果没有构成重写则编译错误
- 源码中`@Target(ElementType.METHOD)`说明`@Override`只能放在方法上

@Deprecated：用于表示某个程序元素（类、方法等）已过时
- 仅不推荐使用，但仍可以使用
- @Deprecated 能够做到新旧版本的兼容和过渡
- @Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE})

@SuppressWarnings：抑制编译器警告
- 不希望看到warning时使用「@SuppressWarnings({""})」
- 在{""}中写入希望抑制的waning信息，需要接收一个数组
- @SuppressWarnings作用范围和放置的位置相关
- @Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})

元注解：
1. @Retention   //指定注解的作用范围
修饰注解，用于指定某个注解可以保留多久，包含RetentionPolicy的对象，对应`Java文件-class文件-JVM运行`三个阶段
- RetentionPolicy.SOURCE ：编译器使用后直接丢弃该注解
- RetentionPolicy.CLASS  ：注解记录在class文件中，运行时JVM不保留
- RetentionPolicy.RUNTIME：注解记录在class文件中，且运行时JVM保留该注解，可以通过反射获取

2. Target       //指定注解可以在哪些地方使用
修饰注解，用于指定某个注解可以用于修饰哪些元素，包含value成员

3. Documented   //指定注释是否会在javadoc中体现
修饰注解，用于指定某个注解将被javadoc工具提取成文档

4. Inherited    //指定注解是否会被继承
修饰注解，用于指定某个注解是否具有继承性。如果某个类使用类@Inherited修饰的注解，子类将自动具有该注解

## 异常 Exception

运行中的系统可能因为一些并不致命的问题崩溃。Java语言中，程序执行中发生的不正常情况称为异常。（非语法和逻辑错误）

1. Error：Java虚拟机无法解决的严重问题。如JVM内部错误、资源耗尽等。

2. Exception：其他因为变成错误或偶然的外在因素导致的一般性问题，可以用针对性的代码处理。分为`运行时异常`和`编译时异常`。
> 如空指针访问，文件不存在，网络中断等

- Error和Exception都继承Throwable类
- 编译时异常发生在`javac`将java文件编译成字节码class文件，由编译器发现。
- 运行异常发生在`java.exe`执行class文件时，在程序运行过程中捕获。
- 编译时异常全部继承Exception，运行时异常先继承`RunTimeException`，再继承Exception
- 编译时异常必须处理，运行时异常很普遍，可以不做处理，全部处理影响可读性和运行效率

### 常见运行时异常

NullPointerException 空指针异常
当程序需要使用对象的时候使用null时抛出

ArithmeticException 算数异常

ArrayIndexOutOfBoundsException 数组下标越界异常

ClassCastException 类型转换异常
试图将对象强制转换为非实例子类时抛出

NumberFormatException 数字格式不正确异常
当程序试图将字符串转换为数字类型，但字符串无法转换为适当格式时抛出

### 常见编译异常

SQLException

IOException

FileNotFoundException

ClassNotFoundException

EOFException

IllegalArguementException

### 异常处理

1. try-catch-finally

异常发生时系统将异常封装成Exception对象传递给catch，程序员根据需求在catch中编写处理逻辑。
finally中的代码则是不管try是否捕获到异常都执行，通常将释放资源放在finally，finally不是必须。
1. 异常发生后直接跳过try中后续代码，执行catch块内容，之后程序`继续运行`
2. 不发生异常时顺序执行完try中代码，并跳过catch
3. 可以同时定义多个catch，父类异常在后，子类异常在前，但是try捕获到异常只会进入一个匹配的catch代码块（由于中断）
4. 可以使用try-finally组合，相当于不管是否发生异常，都必须执行某个逻辑（如写日志，释放资源）

```Java
try{
    //may have Exception
}
catch(Exception e){
    //get Exception and handle
}
catch{
    //anothor catch
}
finally{
    //always extend if Exception catched
}
```

2. throws
> 一般try-catch和throws二选一（自己处理或调用者处理）
捕获异常的方法向上抛出（throws）异常，即将异常返回给调用的方法；接收异常的方法也可以使用两种方法来处理（catch/throws）；
该异常可以向上抛出直到main方法，main方法将抛出到JVM中，JVM将直接打印异常信息并中断程序（默认的异常处理方法）。

1. 显式声明抛出异常时，表示该方法不对这些异常做任何处理，由方法的调用者处理
2. throws语句可以声明抛出异常的列表，可以是方法中产生的异常类型，也可以是其父类
3. 编译时异常必须处理，运行时异常可以不处理，默认throws方法
4. 子类重写父类方法时，子类方法抛出异常要么和父类一致，要么为父类抛出异常的子类

### 自定义异常

1. 定义类需要继承Exception或者RuntimeException
2. 一般继承RuntimeException，便于利用默认机制
3. 一般仅调用构造器创建异常信息，在业务中catch或抛出异常

【throw对比throws】
throws：异常的一种处理方法，使用在方法声明处，后面为异常的类型
throw： 手动生成异常对象并抛出，使用在方法体中，需要返回异常的对象

## 常用类

### 包装类
包装类是针对八种基本数据类型相应的引用类型

1. `Boolean、Character`直接继承Object
2. `Byte、Short、Integer、Double、Float、Long`继承Number类

【装箱和拆箱】
jdk5以前采取手动装箱/拆箱的方式，jdk5后采取自动方式。自动`装箱`底层调用valueOf()方法
> 装箱：基本类型->包装类 拆箱：包装类->基本类型

- 使用直接赋「Integer i = 1」和valueOf(1)时，在—128 ～ 127范围内，返回的总是缓存的对象（底层创建好的对象数组），多次调用返回同一个对象；范围外则返回new的对象
- 用`==`判断包装类和基本类型时，直接判断值是否相等
```Java
//其他包装类和Integer相似
//手动装箱
Integer integer = new Integer(num);
Integer integer1 = Integer.valueOf(n1);
//手动拆箱
int i = integer.intValue();

//自动装箱
Integer integer2 = num;
//自动拆箱
int n3 = integer2;
```

【包装类转String】
```Java
Integer i = 1;
//Warpper to String
String str1 = i + "";
String str2 = i.toString();
String str3 = String.valueOf(i);

//String to Warpper
String str = "123";
Integer i1 = new Integer(str);
Integer i2 = Integer.parseInt(str);
```

### String

- 字符串常量对象是用双引号包裹的字符序列
- 字符串的字符使用Unicode编码，一个字符（中/英）占两个字节
- String实现了`Serializable`接口，即可串行化：表明可以在网络中传输
- String实现了`Comparable`接口，表明可以相互比较
- String是final类，不可以被继承，代表不可变的字符序列
- String使用final char value[]数组保存数据，不能修改其value指向的地址，但是可以修改字符内容

【创建String对象】
1. 直接赋值 String s = "xxx";
- 先检查常量池是否存在指向"xxx"的数据空间，有就指向该地址，没有就创建并指向（最终指向常量池的地址）

2. 构造器创建String s = new String("hsp");
- 先在堆中创建空间，其中维护了value属性，让value属性来指向常量池。（最终引用指向堆的地址）

3. 常量相加，在池中操作；变量相加，在堆中操作。

    1. String a = "xxx" + "yyy"; => 在常量池中只存在"xxxyyy"常量，编译器会优化，避免无引用的常量（GC？）
    2. String c = a + b; => 使用StringBuilder来创建新的字符串，然后append将a和b加入，最后用StringBuildertoString()方法返回String对象给c，即先指向堆，然后指向常量池，池中有三个常量

【String常用方法】
- equals / equalsIgnoreCase -> 相等 / 忽略大小写判断相等
- indexOf / LastIndexOf -> 第一次出现的位置 / 最后一次出现的位置，找不到返回-1
- concat /replace -> 拼接字符串 / 替换字符串
- split -> 分割字符串，特殊字符需要带上转义字符'\'
- format -> 格式化字符串（类似C语言 printf的写法），使用占位符来将不同类型的数据填充到字符串中

### StringBuffer StringBuilder

【StringBuffer】
1. 父类为AbstractStringBuilder，使用`非final`的char数组存储内容，
2. 字符串内容在堆中，每次直接更新内容，不用创建对象/更换地址，效率较高
2. StringBuffer是final类，无法被继承
3. 实现了Serializable接口，可串行化
4. StringBuffer有自动扩容机制 -> 先创建扩容后大小的数组，再copy内容

常用方法
- append / delete[start, end) -> 增删
- replace[start, end) / indexOf ->该查
- insert(index, str) -> 指定位置插入

【StringBuilder】
StringBuilder提供与StringBuffer兼容的API，但是不保证同步。大多数实现中比StringBuffer快
该类被设计用作StringBuffer的简易替换，用在字符串缓冲区被耽搁线程使用时。

1. 父类为AbstractStringBuilder，使用`非final`的char数组存储内容，
2. 字符串内容在堆中，每次直接更新内容，不用创建对象/更换地址，效率较高
2. StringBuffer是final类，无法被继承
3. 实现了Serializable接口，可串行化
4. StringBuffer有自动扩容机制 -> 先创建扩容后大小的数组，再copy内容
5. StringBuilder的方法没有使用` synchronized`，即没有互斥的处理，所以单线程时才使用StringBuilder

String：不可变，效率低，复用率高（大量对String修改时，极大影响性能）
Sbuffer：可变，效率较高，线程安全
Sbuilder：可变，效率最高，线程不安全

### System类

exit：退出当前程序 -> 参数为退出状态，0为正常状态
arraycopy：复制数组元素，比较适合底层调用，一般使用Arrays.copyOf()
currentTimeMillens：返回1970-1-1到当前的毫秒数
gc：运行垃圾回收机制

### 大数处理

【BigInteger】
操作超过long范围的整数

```Java
//创建时用字符串形式包裹大数
BigInteger big = new BigInteger("999999999999999999999999999");
//用方法 + - * /， 需要两个BigInteger操作
big.add();
big.subtract();
big.multiply();
big.divide();
```

【BigDecimal】
保存精度很高的数字时
```Java
//创建时用字符串形式包裹大数
BigDecimal big = new BigDecimal("1231312.999999999999999999999999999");
//用方法 + - * /， 需要两个BigDecimal操作
big.add();
big.subtract();
big.multiply();
big.divide(); // 除法可能导致无限循环小数，抛出算数异常 -> 加入第二个参数指定精度
```

### 日期类

【第一代】
Date：精确到毫秒，表示特定瞬间
- 可序列化，可比较，可克隆
- 直接获取日期返回英文格式，根据需要转换
- 根据毫秒数计算日期

SimpleDateFormat：格式化/解析日期的类，包括格式化（日期->类），解析（文本->日期）和规范化

【第二代】JDK1.1后
Calendar
- 可序列化，可比较，可克隆
- 是一个抽象类，构造器为private，通过getInstance方法获得对象实例
- 为日历字段之间的转换以及日历字段的操作提供了一些方法

【第三代】JDK8后
Calendar的问题：
1. 可变性：日期/时间应该是不可变的
2. 偏移性：Data中年从1900开始，月份从0开始
3. 格式化：只有Date能格式化，Calendar不行
4. 安全性：Date和Calendar不是线程安全的，也不能处理闰秒

LocalDate 日期 年月日

LocalTime 时间 时分秒

LocalDateTime 日期时间 年月日时分秒

DateTimeFormatter 格式化日期时间

Instant 时间戳：类似于Date，提供了一系列和Date转换的方法

## 集合

数组的缺陷
1. 定义时必须指定长度，且指定后不可修改
2. 保存的必须为同一类型的元素
3. 数组扩容操作复杂

集合
1. 动态保存任意数量的对象，便于使用
2. 提供了一系列操作对象的方法
3. 使用集合便于管理元素


### Collection

Collection常用子类(单列集合)
- [List](#list) - Vector ArrayList LinkedList
- [Set](#set)  - TreeSet HashSet

1. Collection的实现子类可以存放多个元素，每个元素可以是Object
2. Collection的实现类，有些可以存放重复元素，有些不行
3. Collection的实现类，有些是有序集合，有些不是有序的
4. Collection接口没有直接实现类，都通过实现Set和List完成的

【迭代器 Iterator对象】
1. Iterator主要用于遍历Collectioin集合中的元素
2. 只要实现类Collection接口的类都有iterator()方法，返回一个实现了Iterator接口的对象
3. Iterator仅用于遍历，并不存放对象
4. 遍历到最后的元素时，再次遍历需要重置（再次调用col.iterator()）
```Java
Collection col = new ArrayList();
Iterator iterator = col.iterator();
while(iterator.hasNext()){
    iterator.next();
}
```

【for循环增强】

增强for底层仍然是iterator方法，可以理解为简化版本的迭代器
```Java
Collection col = new ArrayList();
for(Object obj : col){
    sout(obj);
}
```
#### List

1. List中元素有序，即添加顺序和读取顺序一致，且元素可重复
2. List中每个元素都有对应的顺序索引，可以通过整数序号取出
3. List有多个实现类，Vector ArrayList LinkedList只是常用的三个 

【ArrayList】
1. ArrayList可以多个存放`null`空元素
2. 底层使用数组来存储数据
3. ArrayList基本等同于Vector，但是ArrayList线程不安全

ArrayList源码
1. ArrayList中维护一个Object数组`elementData = {}`（被transient修饰，表示该属性不被序列化）
2. 创建对象时，无参构造的数组容量为0，第一次扩容为10；有参构造器则为指定大小
3. add操作时，先确认容量是否允许添加数据`「ensureCapacityInternal方法」`，确认后再添加元素
4. ensureCapacityInternal方法中，先确定初始容量，再确认是否需要扩容`「ensureExplicitCapacity方法」`
5. ensureExplicitCapacity方法中，先记录集合被修改的次数，再判断是否需要扩容，需要则调用grow方法
6. grow方法中，记录扩容后的值（1.5倍），若扩容后仍小于最小值10，扩容为10；扩容使用`Arrays.copyOf`

【Vector】
1. Vector中维护一个Object数组`elementData = {}`来存放数据
2. 所有方法都是用`synchronized`修饰，是线程安全的
3. Vector无参构造默认设置大小为10
4. 扩容机制：构造器第二个参数为扩容大小，若不指定则按照原大小的2倍扩容

【LinkedList】
1. LinkedList底层实现了双向链表，有双端队列特点
2. 可以添加任意元素
3. 线程不安全，没有实现同步

add() - 添加到双向链表尾部
remove() - 从头部删除第一个元素

【ArrayList vs LinkedList】

> 根据增删/查改的业务需求选择

| 容器 | 底层结构 | 增删效率 | 改查效率 |
|:---:|:---:|:---:|:---:|
|ArrayList|可变数组|较低，超过容量数组扩容| 较高|
|LinkedList|双向链表|较高，通过链表追加|较低|

#### Set
1. 无序，添加和取出顺序不同，没有索引（取出的顺序是固定的 ）
2. 不存放重复元素，最多一个null
3. 主要实现类TreeSet HashSet
4. 遍历可以使用迭代器和增强for循环，不能通过索引

【HashSet】
1. HashSet实际是HashMap，为了保证<Key, Value>的输入，定义了一个object来统一填充value
2. HashSet不保证元素是有序的，取决于根据hash值确定的索引
3. add()方法返回是否添加成功的bool值
4. new两个属性相同的对象都可以加入，两个new String()对象不可以加入

HashMap底层为`【数组+链表+红黑树】`
1. 数组（table）的大小为16
2. 链表达到一定容量时会转化为红黑树

【添加元素】
1. 添加元素时，调用「key.hashCode()」来计算hash值来转换为索引值，其中`hashcode`方法根据实际对象而定
2. 找到table表，查看索引位置是否有已存放的元素
3. 没有则直接加入，有则调用`equals`比较，相同则放弃添加，不同则加到链表尾部，其中`equals`方法根据实际对象而定
4. jdk8中，若一个链表的元素个数达到「TREEIFT_THRESHOLD（默认8）」时，尝试转换为红黑树
    1. 如果table的大小达到64，则转化
    2. 否则进行table扩容 * 2

hash值计算方法，尽量避免hash碰撞
「key == null ? 0 : (h = key.hashCode()) ^ (h >>> 16);」

扩容机制
1. 第一次扩容到16，加载因子0.75，临界值为16*0.75 = 12
2. table使用到临界值时（size==12），就会扩容到两倍（16*2 = 32， 32*0.75 = 24）

【LinkedHashSet】
1. 是HashSet的子类，底层是LinkedHashMap，底层维护一个数组+双向链表
2. LinkedHashSet根据hashCode值来决定元素的存储位置，同时使用链表维护元素的次序，看起来是有序的
3. 插入的元素有pre和next属性，按照插入的顺序形成双向链表
4. table类型：HashMap内部类Node， 存储节点类型为LinkedHashMap内部类Entry
5. 静态内部类Entry是静态内部类Node的子类
6. 扩容和添加机制与HashSet一样，主要区别是使用双向链表管理索引

【TreeSet】
1. 无参构造器创建TreeSet时，是无序集合；TreeSet提供了比较器为参数的构造器
2. 传入的Comparator被传递给了TreeMap，即实际底层为TreeMap；存放时只改变key，value始终是一个固定的静态对象
3. 根据传入的比较器判断是加入到树的哪个节点位置，若存在相等的key则不加入
4. TreeSet中判断相等时，只要Comparator返回0，则判断为相等（区别于HashSet：HashSet使用equals方法）
5. 如果创建TreeSet时没有传入比较器，则传入的元素必须实现`Comparable`接口（默认调用该接口的`CompareTo`方法）

### Map

Map常用子类(双列集合<Key, Value>)
- Map  - HashTable HashMap TreeMap
- HashTable - Properties
- HashMap   - LinkedHashMap

JDK8Map特点
1. Map中的key和value可以是任何类型的数据，会封装到HashMap$Node对象
2. Map中的key不允许重复，有相同key输入时，会覆盖原有的value；value可以重复，无影响
3. key允许存在一个null，value可以存在多个null
4. 一个Map中，key和value存在单项一对一关系
5. 一对k-v存放在一个Node中
5. 为了便于遍历，底层创建EntrySet集合，一个Entry（Map.Entry）对象就包含k，v；
6. 单独提供AbtractSet的子类KeySet来仅遍历key，以及AbstractCollection的子类Values来遍历value

【Map的遍历方式】
1. 使用keySet遍历key，再get对应的value（增强for）
2. keySet的iterator迭代器，通过next()获取下一个key
3. 直接使用Values获取value值（无key）
4. Values的迭代器（无key）
5. 通过EntrySet获取k-v，使用增强for遍历（每个对象都是entry），需要先转换为Map.Entry
6. EntrySet迭代器

#### HashMap

1. 不保证存储顺序和输入顺序一致
2. HashMap没有实现同步，线程不安全

添加和扩容机制与HashSet类似，初始化为16，阈值因子为0.75，总size达到因子*size = 12就扩容；
当链表size达到8时，先判断数组size是否大于等于64，满足则树化，否则先扩容数组

#### Hashtable

1. 存放的元素为键值对
2. 存放的key和value都不允许为null
3. 方法和HashMap基本一致
4. Hashtable是线程安全的（synchronized）

【底层机制】
1. 底层用数组`Hashtbale$Entry[]`存储数据，初始化大小为11
2. 扩容临界值 threshold 8 = 11 * 0.75， 插入时，容量 >= 临界值就会扩容
3. 扩容机制为容量「capacity << 1 + 1」，即乘以2加1

【Properties】
1. 继承了Hashtable且实现了Map接口 
2. 还用于从 .properties 文件中获取数据并加载到Properties对象
3. 通常使用properties文件作为配置文件


### 集合实现类的选择

> 根据业务特点选择

1. 判断单列或双列
2. 单列：Collection
    1. 允许重复：List
        1. 增删为主：LinkedList
        2. 改查为主：ArrayList
    2. 不允许重复
        1. 无序：HashSet
        2. 排序：TreeSet
        3. 插入和取出保持顺序：LinkedHashSet
3. 双列：Map
    1. Key无序：HashMap（jdk7:数组+链表；jdk8:数组+链表+红黑树）
    2. Key有序：TreeMap
    3. key保持顺序：LinkedHashMap
    4. 读取文件：Properties

### Collections工具类
> 常用静态方法
【List】：
    - reverse：反转顺序
    - shuffle：随机排序
    - sort(List)：按照自然顺序升序排列
    - sort(List, Comparator)： 按照比较器规则排序
    - swap(List, int, int)：交换指定位置的两个元素

【Collection】
    - max/min：返回最大/小值
    - max/min(Collection, Comparator)：根据比较器返回最大/小值
    - frequency(Collection, Object)：返回某个元素出现的次数
    - copy(List dest, List src)：将src的内容复制给dest
    - replaceAll(List list, Object oldVal, Object newVal)：将list中所有oldVal替换为newVal


### 泛型 generic

不使用泛型的问题：
1. Collection中存储的数据类型无法约束，可能导致程序抛出异常；使用泛型可以约束存储类型
2. 如果存储的数据量过大，遍历时大量类型转换会影响效率；使用泛型后遍历时可以直接读取为存储类型

```Java
//<Class>指定存储到ArrayList的元素类型，存放其他类型会报错
ArrayList<Class> list = new ArrayList<Class>();
//遍历时可以指定类型而不使用Object
for(Class c : list){
    sout(c);

}
```

1. 泛型又称参数化类型，相当于用‘变量’泛指所有类型，运行时采用传入的类型；出现于JDK5.0，解决数据类型的安全性问题
2. 在类声明/实例化时只要指定好需要的具体类型即可，泛型的具体类型在编译期间确定；泛型不指定时全部默认为Object
3. Java泛型可以保证编译时不发生警告，运行时不产生ClassCastException，代码更加简洁健壮
4. 泛型用于三个方面
    1. 类声明时通过标识表示类中某个属性的类型
    2. 方法的返回值类型
    3. 方法的参数类型
5. 传入泛型时只能是引用类型；指定泛型具体类型后，可以传入类型本身或其子类对象

```Java
//使用E来标识某个属性的类型，该类型在定义对象时指定，在编译期间确定E的类型
//允许多个泛型标识符，一般是单个大写字母
class Class<E>{
    E attr;

    Class(E a){}

    public E f(){}
}
//接口和类都可以指定泛型，且可以指定多个泛型
interface Inter<T>{}

psvm(String args[]){
    //实际开发中简写，第二个<>可以省略类型
    List<Integer> list = new ArrayList<Integer>();
    List<Integer> list1 = new ArrayList<>();
    //不指定泛型时等价于默认使用 <Object>
    List list2 = new ArrayList();
}
```

【自定义泛型】

类和接口都可以自定义泛型：
1. 普通成员（属性/方法）可以使用泛型
2. 使用泛型的数组不能初始化（由于不确定类型，不确定开辟多大的空间）
3. 静态方法中不能使用类的泛型，静态方法在类加载时定义，此时还未创建对象
4. 泛型接口的类型在继承或实现接口时确定
5. 接口的成员默认为静态成员，不能使用泛型

自定义泛型方法

1. 可以定义在普通类和泛型类中
2. 泛型方法被调用时，必须确定泛型的具体类型
3. 方法可以使用类声明的泛型，也可以使用自己声明的泛型，注意区分`泛型方法`和`使用泛型的方法`

```Java
modifier <T,R,...> type name(T attr, R r){
    
}

//调用时传入参数会自动确定类型
name("", 1);
```

【继承/通配符】
1. 泛型不能继承
2. <?>：表示支持任意泛型
3. <? extends A>：支持A类及A类的子类，约束了泛型的上限
4. <? super A>：支持A类以及A类的父类，约束了泛型的下限