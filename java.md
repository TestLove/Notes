[TOC]



# Java基础知识

## 基本类型

###  类别及其对应包装类

#### 1. byte---Byte


#### 2. char---Character
#### 3. short---Short
#### 4. int---Integer
#### 5. long---Long
#### 6. float---Float
#### 7. double---Double
#### 8. boolean---Boolean

### 装箱及拆箱

#### 缘由

#### 变化

##### JDK1.5以前

~~~java
public class WrapperClass {
	public static void main(String[] args) {
		//构造器方法定义Integer包装类a，并赋值
		Integer a=new Integer("5");
		System.out.println(a);
		
		//构造器方法定义Double包装类b，并赋值
		Double b=new Double("5.5");
		System.out.println(b);
		
		//通过包装类.xxxValue()方法，从包装类对象中获取基本类变量
		int c=a.intValue();
		System.out.println(c);
		
		double d=b.doubleValue();
		System.out.println(d);
	}
}

~~~
##### JDK1.5以后

自动装箱与自动拆箱

### 数组的定义

#### 一维数组

~~~java
int []a=new int[10];
int[]a={1,2,3};

~~~

#### 多维数组

~~~java

~~~

  



### 关键字

#### final

- 修饰的类为最终类(不可被继承)
- 修饰的方法不可被子类重写
- 修饰的属性为常量

#### static

#### finally

> finally语句块里的语句必须被执行

finally中的return会覆盖前面的return

## 面向对象

### 类



#### 抽象类

> 含有abstract修饰符的class即为抽象类，abstract类不能创建的实例对象。含有abstract方法的类必须定义为abstract class，abstract class类中的方法不必是抽象的。abstract class类中定义抽象方法必须在具体(Concrete)子类中实现，所以，不能有抽象构造方法或抽象静态方法。如果的子类没有实现抽象父类中的所有抽象方法，那么子类也必须定义为abstract类型。

接口（interface）可以说成是抽象类的一种特例，接口中的所有方法都必须是抽象的。接口中的方法定义默认为public abstract类型，接口中的成员变量类型默认为public static final。

下面比较一下两者的语法区别：

1.抽象类可以有构造方法，接口中不能有构造方法。

2.抽象类中可以有普通成员变量，接口中没有普通成员变量

3.抽象类中可以包含非抽象的普通方法，接口中的所有方法必须都是抽象的，不能有非抽象的普通方法。

4. 抽象类中的抽象方法的访问类型可以是public，protected和（默认类型,虽然

eclipse下不报错，但应该也不行），但接口中的抽象方法只能是public类型的，并且默认即为public abstract类型。

5. 抽象类中可以包含静态方法，接口中不能包含静态方法

6. 抽象类和接口中都可以包含静态成员变量，抽象类中的静态成员变量的访问类型可以任意，但接口中定义的变量只能是public static final类型，并且默认即为public static final类型。

#### 类变量与类函数

#### 实例变量实例函数

#### 继承

- 初始化顺序：
	1. 父类静态块与静态函数
	2. 子类静态块与静态函数
	3. 父类构造函数
	4. 子类构造函数
    ```Markdown
    **注意**：静态块只执行一次，按顺序执行
    
  在Java里，只有值传递，因为引用本身就是一个地址值，我们说的”传递引用“本质上也是“值传递”，只不过传递的是地址值。
    
    在方法中，改变一个对象参数的引用不会影响到原始引用。这是很自然的。
    
    举个例子，假设在函数外有 A a = new A();那么引用a指向堆中的一个对象A()。
    ```

#### 多态

##### 向上转型

把子类对象当做父类对象来用

```java
class Father{}
class Son extends(Father){}
Father f1 = new Son();// f1的内存空间实际上仍然是Son的内存空间,但引用是Father,相当于把Son当Father用

```

##### 向下转型

是向上转型的还原

```java
Son s1 = (Son) f1; //此处将f1重新交给son对象引用,原因是因为f1的内存结构就是son的内存结构
```



#### 权限关系

|              | public | protected | default | private |
| ------------ | ------ | --------- | ------- | ------- |
| 同一类       | √      | √         | √       | √       |
| 同包         | √      | √         | √       |         |
| 不同包子类   | √      | √         |         |         |
| 不同包非子类 | √      |           |         |         |

#### 内部类

> 一个类定义在另一个类的内部，这个类就是Inner Class,Inner Class的实例不能单独存在，必须依附于一个Outer Class的实例
>
> 除了能引用Outer实例外，还可以修改Outer Class的`private`字段

内部类实例化

`Outer.Inner inner = outer.new Inner()`

##### 匿名内部类

##### 静态内部类

它不再依附于`Outer`的实例，而是一个完全独立的类，因此无法引用`Outer.this`，但它可以访问`Outer`的`private`静态字段和静态方法。

### 一些工具类

#### String

> 为字符串不可变类(Strings are immutable)

```java
 /** The value is used for character storage. */
    private final char value[];
/*通过构造函数可以将字节数组,字符数组,StringBuilder,StringBuffer对象转换为string*/
public String(int[] codePoints, int offset, int count)//也可以是Unicode编码数组
    /**
     * Returns a canonical representation for the string object.
     * <p>
     * A pool of strings, initially empty, is maintained privately by the
     * class {@code String}.
     * <p>
     * When the intern method is invoked, if the pool already contains a
     * string equal to this {@code String} object as determined by
     * the {@link #equals(Object)} method, then the string from the pool is
     * returned. Otherwise, this {@code String} object is added to the
     * pool and a reference to this {@code String} object is returned.
     * <p>
     * It follows that for any two strings {@code s} and {@code t},
     * {@code s.intern() == t.intern()} is {@code true}
     * if and only if {@code s.equals(t)} is {@code true}.
     * <p>
     * All literal strings and string-valued constant expressions are
     * interned. String literals are defined in section 3.10.5 of the
     * <cite>The Java&trade; Language Specification</cite>.
     *
     * @return  a string that has the same contents as this string, but is
     *          guaranteed to be from a pool of unique strings.
     */
    public native String intern();
```

但凡要传字符参数却是int型的都是传的Unicode码点

**关于字符串的一些比较**

> - 直接使用双引号声明出来的`String`对象会直接存储在常量池中。
> - 如果不是用双引号声明的`String`对象，可以使用`String`提供的`intern`方法。intern 方法会从字符串常量池中查询当前字符串是否存在，若不存在就会将当前字符串放入常量池中,存在则把该字符串的地址返回给String对象
> - new String(b)会在常量池中创建一个b对象,在堆中创建一个String对象引用b对象的地址
>
> 问题来了,那intern还有什么用?
>
> ```java
> //考虑这样一个string
> String a=new String(1)+new String(1);
> //常量池中只有1,这个常量.而a所指向则为11,这时调用intern则会把11加入常量池(jdk1.6)或者把a所引用的对象加入常量池(jdk1.7)
> ```
>

jdk1.7的改变
- 将String常量池 从 Perm 区移动到了 Java Heap区
- String#intern 方法时，如果存在堆中的对象，会直接保存对象的引用，而不会重新创建对象。

```java
public static void main(String[] args) {
    String s = new String("1");
    s.intern();
    String s2 = "1";
    System.out.println(s == s2);//false

    String s3 = new String("1") + new String("1");
    s3.intern();
    String s4 = "11";
    System.out.println(s3 == s4);//true
}
```

String s = new String("abc")创建两个对象.

![image-20201106193406234](https://raw.githubusercontent.com/TestLove/Pictures/main/img/new%E4%B8%8E%E7%9B%B4%E6%8E%A5%E5%BC%95%E7%94%A8%E7%9A%84%E5%8C%BA%E5%88%AB.png)

```java
//那这样呢
String a=new String("emtf");
String b=new String("nhm");
String c=a+b;//创建一个对象
String d="i"+"love";
//idea真香
String a = new String("emtf");
String b = new String("nhm");
(new StringBuilder()).append(a).append(b).toString();
String d = "ilove";
```

##### 总结

1. new String(A)总会在堆中创建一个String对象,如果常量池中无A,则把A加入常量池,有则直接返回该地址给String对象
2. String s3 = new String("1") + new String("1"),只会把1加入常量池,然鹅11则不会被加入
3. 在2的情况下调intern会把11的引用加入常量池,而不是直接创一个常量对象(jdk1.7) 
4. 字符串相加,常量会自动优化,变量会创建一个StringBuilder对象进行连接

## 语法

### 关于循环的优化

1. 强度削弱
2. 删除归纳变量
3. 代码外提
### 判断

```java
switch(x)//  required: 'char, byte, short, int, Character, Byte, Short, Integer, String, or an enum'
{
default:
System.out.println("Hello");
}
```

### 细节

**子串!=子序列**

子串：  awbc.  awbcd  awbcde  ....很多个子串  但是都是连续在一起  

子序列： abc  .  abcd   abcde  ...  很多个子序列  但是子序列中的字符在字符串中不一定是连在一起的

**复制**

浅拷贝:只复制引用

- Array.copy()
- Array.copyRange()
- Array.getChar()
- System.ArrayCopy()
- clone()

深拷贝:引用和对象都被复制

# Java高级知识

## 注解

> 什么是注解
>
> Java注解又称Java标注，是JDK5.0版本开始支持加入源代码的特殊语法元数据。
>  Java语言中的类、方法、变量、参数和包等都可以被标注。和Javadoc不同，Java标注可以通过反射获取标注内容。在编译器生成类文件时，标注可以被嵌入到字节码中。Java虚拟机可以保留标注内容，在运行时可以获取到标注内容。 当然它也支持自定义Java标注。

### 自定义注解

> 可以通过元注解定义注解

#### **举个栗子**

```java
@Inherited
@Documented
@Target({ ElementType.METHOD, ElementType.TYPE })
@Retention(RetentionPolicy.RUNTIME)
public @interface SysLog {
    LogType value() default LogType.ALL;
    String prefix() default "";

}
```

#### 格式要求

**定义注解格式：**
　　public @interface 注解名 {定义体}

**注解参数的可支持数据类型：**

　1.所有基本数据类型（int,float,boolean,byte,double,char,long,short)
　2.String类型
　3.Class类型
　4.enum类型
　5.Annotation类型
　6.以上所有类型的数组

#### 元注解详解

##### **@Target**

> @Target说明了Annotation所修饰的对象范围：Annotation可被用于 packages、types（类、接口、枚举、Annotation类型）、类型成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（如循环变量、catch参数）。在Annotation类型的声明中使用了target可更加明晰其修饰的目标。

**作用：用于描述注解的使用范围（即：被描述的注解可以用在什么地方）**

**取值(ElementType)有：**

　　　　1.CONSTRUCTOR:用于描述构造器
　　　　2.FIELD:用于描述域
　　　　3.LOCAL_VARIABLE:用于描述局部变量
　　　　4.METHOD:用于描述方法
　　　　5.PACKAGE:用于描述包
　　　　6.PARAMETER:用于描述参数
　　　　7.TYPE:用于描述类、接口(包括注解类型) 或enum声明

##### @Retention

> 描述注解的生命周期，取值有：
>
>  默认RetentionPolicy.CLASS 值

1. RetentionPolicy.SOURCE   源码中保留，编译期可以处理
2. RetentionPolicy.CLASS   Class文件中保留，Class加载时可以处理
3. RetentionPolicy.RUNTIME 运行时保留，运行中可以处理

##### @Inherited

> 标记注解，使用@Inherited修饰的注解作用于一个类，则该注解将被用于该类的子类。`@Inherited`仅针对`@Target(ElementType.TYPE)`类型的`annotation`有效，并且仅针对`class`的继承，对`interface`的继承无效：

##### @Documented

> 描述注解可以文档化，是一个标记注解。
> 在生成javadoc的时候，是不包含注解的，但是如果注解被@Documented修饰，则生成的文档就包含该注解。



## 反射

> 通过`Class`实例获取`class`信息的方法称为反射
>
> 反射是为了解决在运行期，对某个实例一无所知的情况下，如何调用其方法

### 通过class获取Class实例

1. 直接通过一个`class`的静态变量`class`获取：

   ```java
   Class cls = String.class;
   ```
2. 如果我们有一个实例变量，可以通过该实例变量提供的getClass()方法获取：

	```java
	String s = "Hello";
	Class cls = s.getClass()
	```

3. 如果知道一个`class`的完整类名，可以通过静态方法`Class.forName()`获取：

   ```java
   Class cls = Class.forName("java.lang.String");
   ```

- 因为`Class`实例在JVM中是唯一的，所以，上述方法获取的`Class`实例是同一个实例。可以用`==`比较两个`Class`实例：

- 用`instanceof`不但匹配指定类型，还匹配指定类型的子类。而用`==`判断`class`实例可以精确地判断数据类型，但不能作子类型比较。

- 通常情况下，我们应该用`instanceof`判断数据类型，因为面向抽象编程的时候，我们不关心具体的子类型。只有在需要精确判断一个类型是不是某个`class`的时候，我们才使用`==`判断`class`实例。
- 注意到数组（例如`String[]`）也是一种`Class`，而且不同于`String.class,[Ljava.lang.String`。此外，JVM为每一种基本类型如int也创建了`Class`，通过`int.class`访问。

### 通过Class实例重新创建class：

```java
// 获取String的Class实例:
Class cls = String.class;
// 创建一个String实例:
String s = (String) cls.newInstance();//这种方法只能进行无参构造
```

JVM总是动态加载`class`，可以在运行期根据条件来控制加载class。

### 获取信息

#### 访问字段

```java
Field getField(name);//根据字段名获取某个public的field（包括父类）
Field getDeclaredField(name);//根据字段名获取当前类的某个field（不包括父类）
Field[] getFields();//获取所有public的field（包括父类）
Field[] getDeclaredFields();//获取当前类的所有field（不包括父类）
//Field类重写后的toString方法
public String toString() {
    int mod = getModifiers();
    return (((mod == 0) ? "" : (Modifier.toString(mod) + " "))//修饰符
         + getType().getTypeName() + " "//类型
         + getDeclaringClass().getTypeName() + "."//类名
         + getName());//名称
    }
```

#### 获取字段值

1. 先获取`Class`实例，再获取`Field`实例
2. 调用`Field.setAccessible(true)`(无论该字段是否公有,都可以获取到值)
3. 然后，用`Field.get(Object)`获取指定实例的指定字段的值

注意:无法直接获得私有变量的值

#### 设置字段值

1. 先获取`Class`实例，再获取`Field`实例
2. 调用`Field.setAccessible(true)`(无论该字段是否公有,都可以设置值)
3. 通过` Field.set(Object, Object)`实现，其中第一个`Object`参数是指定的实例，第二个`Object`参数是待修改的值

#### 访问方法

```java
Method getMethod(name, Class...)//获取某个public的Method（包括父类）
Method getDeclaredMethod(name, Class...)//获取当前类的某个Method（不包括父类）
Method[] getMethods()//获取所有public的Method（包括父类）
Method[] getDeclaredMethods()//获取当前类的所有Method（不包括父类）
```

#### 调用方法

1. 先获取`Class`实例，再获取Method实例
2. 调用`Method.setAccessible(true)`(无论该字段是否公有,都可以设置值)
3. 通过` Method.invoke(Object, args)`实现，其中第一个`Object`参数是指定的实例，第二个`args`参数是方法的参数

如果获取到的Method表示一个静态方法，调用静态方法时，由于无需指定实例对象，所以`invoke`方法传入的第一个参数永远为`null`

使用反射调用方法时，仍然遵循多态原则：即总是调用实际类型的覆写方法（如果存在）

#### 调用构造方法

```java
Constructor getConstructor(Class...)//获取某个public的Constructor；
Constructor getDeclaredConstructor(Class...)//获取某个Constructor；
Constructor[] getConstructors()//获取所有public的Constructor；
Constructor[] getDeclaredConstructors()//获取所有Constructor。
```

通过`Constructor`实例可以创建一个实例对象：`newInstance(Object... parameters)`； 通过设置`setAccessible(true)`来访问非`public`构造方法。

#### 获取继承关系

```java
Class getSuperclass()//获取父类类型；
Class[] getInterfaces()//获取当前类实现的所有接口。
```

如果是两个`Class`实例，要判断一个向上转型是否成立，可以调用`isAssignableFrom()`：

#### 使用动态代理

在运行期动态创建一个`interface`实例的方法如下：

1. 定义一个`InvocationHandler`实例，它负责实现接口的方法调用；

2. 通过

   ```
   Proxy.newProxyInstance()
   ```

   创建

   ```
   interface
   ```

   实例，它需要3个参数：

   1. 使用的`ClassLoader`，通常就是接口类的`ClassLoader`；
   2. 需要实现的接口数组，至少需要传入一个接口进去；
   3. 用来处理接口方法调用的`InvocationHandler`实例。

3. 将返回的`Object`强制转型为接口。

## 流与文件

### 流

```Markdown
输入流:
	从其中读入一个字节序列的对象
输出流:
	向其中写入一个字节序列的对象
目标:
	1. 文件
	2. 网络连接
	3. 内存块
分类:
	1. 字符流
	2. 字节流
基础:
	抽象类InputStream和OutputStream
可以用.表示当前目录，..表示上级目录。
```

#### 读写字节

`abstract int read()`:
​	读入一个字节，并返回读入的字节，或者在遇到输入源结尾时返回-1,在设计具体的的输入流类时,必须覆盖这个方法

`abstract void write(int b)`:

#### 完整的流家族

#### 组合流过滤器

#### hhh

```markdown
1. System.out与System.in
	public final static InputStream in = null;
	public final static PrintStream out = null;
```



### 文件

#### 创建文件



1. `java.io.File.createNewFile()`
2. `java.io.FileOutPutStream`,会自动创造文件,如果不存在该文件

##### File

> ```Markdown
> Instances of this class may or may not denote an actual file-system
> object such as a file or a directory.
> 
> ```
>
> 属于对于一个文件或文件夹的映射(可以实际上并不存在这个文件,通过createNewFile()创建)

有各种方法模拟人在操作系统上对文件的操作,事实上是调用`class WinNTFileSystem extends FileSystem`这个类的方法和本地方法来完成操作

- 创建文件夹,文件
- 删除文件夹,文件
- 查看设置文件属性(长度,是否可读,是否可写)

## 异常

### 要求

```markdown
1. 返回到一种安全状态，并能够让用户执行一些其他的命令；
2. 允许用户保存所有操作的结果，并以妥善的方式终止程序。
```

### 类型

```Markdown
1. 用户输入错误
2. 设备错误
3. 物理限制
4. 代码错误
```

### 异常分类

```Markdown
 **异常对象都是派生于Throwable类的一个实例**
 **如果Java中内置的异常类不能够满足需求，用户可以创建自己的异常类**
```

- 由程序错误导致的异常属于RuntimeException；

  - ·错误的类型转换。
  - ·数组访问越界。
  - ·访问null指针

- 而程序本身没有问题，但由于像I/O错误这类问题导致的异常属于其他异常。

  - ·试图在文件尾部后面读取数据。

  - ·试图打开一个不存在的文件。

  - ·试图根据给定的字符串查找Class对象，而这个字符串表示的类并不存在

1. 派生于==Error类或RuntimeException类==的所有异常称为==非受查（unchecked）异常，==
1. 所有其他的异常称为==受查（checked）异常==。

**所有错误都发生在运行时。**

![image-20201106181428821](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201106181428821.png)

### 异常声明与抛出

#### 声明

```Markdown
1. 调用一个抛出受查异常的方法，例如，FileInputStream构造器。
2. 程序运行过程中发现错误，并且利用throw语句抛出一个受查异常。//必须声明
3）程序出现错误，例如，a[–1]=0会抛出一个ArrayIndexOutOfBoundsException这样的非受查异常。
4）Java虚拟机和运行时库出现的内部错误。
```

**警告**：如果在子类中覆盖了超类的一个方法，==子类方法中声明的受查异常不能比超类方法中声明的异常更通用==（也就是说，子类方法中可以抛出更特定的异常，或者根本不抛出任何异常）

#### 抛出

![image-20200723095652535](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723095652535.png)
![image-20200723095711297](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723095711297.png)

![image-20200723095854939](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723095854939.png)

**步骤** 

```Markdown
1）找到一个合适的异常类。
2）创建这个类的一个对象。
3）将对象抛出。
```

**自定义异常**:定义一个派生于Exception的类，或者派生于Exception子类的类

![image-20200723100529439](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723100529439.png)
![image-20200723100542853](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723100542853.png)

### 异常捕获

#### **注意**

```Markdown
1. 要想捕获一个异常，必须设置try/catch语句块
2. 如果在try语句块中的任何代码抛出了一个在catch子句中说明的异常类，
	1. 程序将跳过try语句块的其余代码。
	2. 程序将执行catch子句中的处理器代码。
3. 如果在try语句块中的代码没有抛出任何异常，那么程序将跳过catch子句。
4. 如果方法中的任何代码抛出了一个在catch子句中没有声明的异常类型，那么这个方法就会立刻退出（希望调用者为这种类型的异常设计了catch子句）。
5. 同一个catch子句中可以捕获多个异常类型,只有当捕获的异常类型彼此之间不存在子类关系时才需要这个特性
6. 捕获多个异常时，异常变量隐含为final变量
7. 代码抛出了一个异常，但这个异常不是由catch子句捕获的。在这种情况下，程序将执行try语句块中的所有语句，直到有异常被抛出为止

```



#### **实例**

捕获多个异常

![image-20201106183518726](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201106183518726.png)

获取更多信息:

```java
e.getMessage()
e.getClass().getName()
```

### finally子句

#### 注意

```Markdown
1. 不管是否被捕获都会被执行
2. try语句可以只有finally子句，而没有catch子句
3. 在方法返回前，finally子句的内容将被执行。如果finally子句中也有一个return语句，这个返回值将会覆盖原始的返回值
```

### 带资源的try语句

#### 实例

![image-20200723105129430](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723105129430.png)

try块退出时，会自动调用res.close（）

![image-20200723105150394](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723105150394.png)

可以指定多个资源

![image-20200723105222474](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723105222474.png)

### 分析堆栈轨迹元素

#### 定义

**堆栈轨迹（stack trace）是一个方法调用过程的列表，它包含了程序执行过程中方法调用的特定位置**

#### 实例

![image-20200723105605483](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723105605483.png)

![image-20200723105633455](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723105633455.png)

可以调用Throwable类的printStackTrace方法访问堆栈轨迹的文本描述信息。

![image-20201106183453924](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201106183453924.png)

一种更灵活的方法是使用getStackTrace方法，它会得到StackTraceElement对象的一个数组，可以在你的程序中分析这个对象数组

![image-20200723105825938](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723105825938.png)

静态的Thread.getAllStackTrace方法，它可以产生所有线程的堆栈轨迹

![image-20201106183428764](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201106183428764.png)

不一定要通过捕获异常来生成堆栈轨迹

`Thread.dumpStack()`

### 断言

#### 格式

```java
assert 条件;
assert 条件:表达式;
/**
*这两种形式都会对条件进行检测，如果结果为false，则抛出一个*AssertionError异常。在第二种形式中，表达式将被传入*AssertionError的构造器，并转换成一个消息字符串。
*/
```

#### 意义

断言机制允许在测试期间向代码中插入一些检查语句。当代码发布时，这些插入的检测语句将会被自动地移走

### 日志

```Markdown
·可以很容易地取消全部日志记录，或者仅仅取消某个级别的日志，而且打开和关闭这个操作也很容易。
·可以很简单地禁止日志记录的输出，因此，将这些日志代码留在程序中的开销很小。
·日志记录可以被定向到不同的处理器，用于在控制台中显示，用于存储在文件中等。
·日志记录器和处理器都可以对记录进行过滤。过滤器可以根据过滤实现器制定的标准丢弃那些无用的记录项。
·日志记录可以采用不同的方式格式化，例如，纯文本或XML。·应用程序可以使用多个日志记录器，它们使用类似包名的这种具有层次结构的名字，例如，com.mycompany.myapp。
·在默认情况下，日志系统的配置由配置文件控制。如果需要的话，应用程序可以替换这个配置。
```

#### 基本日志

1. 使用全局日志记录器:global logger

`Logger.getGlobal().info("File->Open menu item selected")`;

2. 如果在适当的地方（如main开始）调用

`Logger.getGlobal().setLevel(Level.OFF);`

会取消所有的日志

#### 高级日志

1. 可以调用getLogger方法创建或获取记录器：

`private static final Logger myLogger=Logger.getLogger("com.mycompany.myapp")`

2. 与包名相比，日志记录器的层次性更强日志记录器的父与子之间将共享某些属性。例如，如果对com.mycompany日志记录器设置了日志级别，它的子记录器也会继承这个级别。

##### 使用方法

![image-20200723145503536](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723145503536.png)

![image-20200723145522550](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723145522550.png)

![image-20201106183401544](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201106183401544.png)

跟踪执行流的方法,这些调用将生成FINER级别和以字符串ENTRY和RETURN开始的日志记录

![image-20200723145652708](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723145652708.png)

![image-20201106183325659](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201106183325659.png)

一般用途 记录那些不可预料的异常调用throwing可以记录一条FINER级别的记录和一条以THROW开始的信息

![image-20200723150011998](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723150011998.png)

![image-20200723150028850](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723150028850.png)

![image-20200723150047643](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723150047643.png)

#### 本地化

#### 实例

下列代码确保将所有的消息记录到应用程序特定的文件中

![image-20200723151808735](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723151808735.png)
logging/LoggingImageViewer.java
![image-20200723151904539](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723151904539.png)
![image-20201106183645440](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201106183645440.png)
![image-20200723151935355](E:\Typora\java\java.assets\image-20200723151935355.png)
![image-20200723151949683](E:\Typora\java.assets\image-20200723151949683.png)
!![image-20201107100347765](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201107100347765.png)
![image-20200723152056584](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723152056584.png)
![image-20200723152112094](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200723152112094.png)

## 反射

## 接口

```markdown

- 接口绝不能含有实例域
- 子类权限不能更严格,所以实现时一定要写public
```

### 步骤

```markdown
1. 将类声明为实现给定的接口。
2. 对接口中的所有方法进行定义。
```

### 特性

1. 能声明接口的变量
2. 接口变量必须引用实现了接口的类对象
3. 可用  ` 对象 instanceof 接口`来检查一个对象是否实现了某些接口
4. 接口中的所有方法自动地属于==public abstract==。因此，在接口中声明方法时，不必提供关键字public。后新增静态方法和默认方法
5. 接口中所有的变量都是==public static final==
6. 接口也可以扩展 `public interface 接口1 extends 接口2`

### 解决冲突

```markdown
1. **超类优先**。如果超类提供了一个具体方法，同名而且有相同参数类型的默认方法会被忽略。
2. **接口冲突**。如果一个超接口提供了一个默认方法，另一个接口提供了一个同名而且参数类型（不论是否是默认参数）相同的方法，必须覆盖这个方法来解决冲突
```



## Java集合

```markdown
- 如果需要一个循环数组队列，就可以使用ArrayDeque类。如果需要一个链表队列，就直接使用     LinkedList类，这个类实现了Queue接口
- 集合类的基本接口是Collection接口
```

### 集合框架

#### collection接口

```java
public interface Collection<E>{
    boolean add(E element);// 用于添加元素
    Iterator<E> iterator();// 用于遍历
    int size();
    boolean isEmpty();
    boolean contains(Object obj);//如果集合中包含了一个与obj相等的对象，返回true
    boolean containsAll(Collection<？>other);//如果这个集合包含other集合中的所有元素，返回true。
    boolean addAll(Collection<？extends E>other);
    boolean remove(Object obj);
    void clear（）
}
```

一些其他方法

![image-20200724095043826](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200724095043826.png)

#### 迭代器

```java
public interface Iterator<E>{
    E next();
    boolean hasNext();
    void remove();
    default void forEachRemaining(Consumer<?suoer E>action);
}
public interface Iterable<E>{
    Iterator<E> iterator;
}
```

- “for each”循环可以与任何实现了Iterable接口的对象一起工作Collection接口扩展了Iterable接口。因此，对于标准类库中的任何集合都可以使用“for each”循环。
- Iterator接口的remove方法将会删除上次调用next方法时返回的元素想要用迭代器删除集合中的元素 ` interator.next()`然后再` interator.remove()`而且不能连续remove.

#### 集合框架中的接口

![image-20201106183756854](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201106183756854.png)

### 具体的集合

![image-20201106183904658](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201106183904658.png)

![image-20201106183829140](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201106183829140.png)

#### 链表

#### ArrayList

> 
> 1. Resizable-array implementation of the List interface.  Implements all optional list operations, and permits all elements, including **<tt>null</tt>**. 
> 2. (This class is roughly equivalent to <tt>Vector</tt>, except that it is <tt>**unsynchronized**</tt>.)
> 

**初始容量为十**

```java
/**
 * Default initial capacity.
 */
private static final int DEFAULT_CAPACITY = 10;
```

#### HashMap

> 1. This implementation provides all of the optional map operations, and permits <tt>null</tt> values and the <tt>null</tt> key
> 2. (The <tt>HashMap</tt>class is roughly equivalent to <tt>Hashtable</tt>, except that it is **unsynchronized** and **permits nulls**.) 

#### HashTable

> 1. Any non-<code>null</code> object can be used as a key or as a value.
> 2. 线程安全
> 

```markdown
If a thread-safe implementation is not needed,use HashMap
If a thread-safe highly-concurrent implementation is desired,use ConcurrentHashMap
```

#### 散列集

#### 树集

```markdown
TreeSet类与散列集十分类似，不过，它比散列集有所改进。树集是一个有序集合（sorted collection）。可以以任意顺序将元素插入到集合中。在对集合进行遍历时，每个值将自动地按照排序后的顺序呈现。
```

#### 队列

##### 双端队列

##### 优先级队列

```Markdown
- 优先级队列（priority queue）中的元素可以按照任意的顺序插入，却总是按照排序的顺序进行检索
- 优先级队列使用了一个优雅且高效的数据结构，称为堆（heap）。堆是一个可以自我调整的二叉树，对树执行添加（add）和删除（remore）操作，可以让最小的元素移动到根，而不必花费时间对元素进行排
```

### 映射

#### 基本操作

```Markdown
- 散列映射对键进行散列，树映射用键的整体顺序对元素进行排序，并将其组织成搜索树。散列或比较函数只能作用于键。与键关联的值不能进行散列或比较。
- 每当往映射中添加对象时，必须同时提供一个键。在这里，键是一个字符串，对应的值是Employee对象。要想检索一个对象，必须使用（因而，必须记住）一个键。
- 键必须是唯一的。不能对同一个键存放两个值。如果对同一个键两次调用put方法，第二个值就会取代第一个值。实际上，put将返回用这个键参数存储的上一个值
```

#### 更新

就是第一次看到word时。在这种情况下，get会返回null，因此会出现一个NullPointerException异常。

![image-20200724112229600](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200724112229600.png)

以使用getOrDefault方法：

![image-20200724112253318](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200724112253318.png)

首先调用putIfAbsent方法。只有当键原先存在时才会放入一个值。

![image-20200724112307798](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200724112307798.png)

merge方法可以简化这个常见的操作。如果键原先不存在，下面的调用：

![image-20200724112318394](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200724112318394.png)

#### 链接散列集与映射

#### 枚举集与映射

在EnumSet的API文档中，将会看到E extends Enum<E>这样奇怪的类型参数。简单地说，它的意思是“E是一个枚举类型。”所有的枚举类型都扩展于泛型Enum类。例如，Weekday扩展Enum<Weekday>

#### 标识散列映射

### 视图与包装器

##### 视图定义

```Markdown
- 返回一个实现Set接口的类对象，这个类的方法对原映射进行操作。这种集合称为视图。
```

##### 轻量级集合包装器

##### 子范围

##### 不可修改的视图

#### 算法

#### 遗留的集合

![image-20200724121512644](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200724121512644.png)

## 并发

### 中断线程

```markdown
1. 当对一个线程调用interrupt方法时，线程的中断状态将被置位。这是每一个线程都具有的boolean标志。每个线程都应该不时地检查这个标志，以判断线程是否被中断。
2. 要想弄清中断状态是否被置位，首先调用静态的Thread.currentThread方法获得当前线程，然后调用isInterrupted方法：
- 有两个非常类似的方法，interrupted和isInterrupted。Interrupted方法是一个静态方法，它检测当前的线程是否被中断。而且，调用interrupted方法会清除该线程的中断状态。另一方面，isInterrupted方法是一个实例方法，可用来检验是否有线程被中断。调用这个方法不会改变中断状态
```

### 线程状态

![image-20200724161432532](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200724161432532.png)

要确定当前状态,可调用getState方法

#### New（新创建）

当用new操作符创建一个新线程时，如`new Thread（r）`，该线程还没有开始运行。这意味着它的状态是new。

#### Runnable（可运行）

一旦调用start方法，线程处于runnable状态。一个可运行的线程可能正在运行也==可能没有运行==

#### Blocked（被阻塞)与Waiting（等待）

- 当线程处于被阻塞或等待状态时，它暂时不活动。它不运行任何代码且消耗最少的资源。直到线程调度器重新激活它

1. 当一个线程试图获取一个内部的对象锁（而不是java.util.concurrent库中的锁），而该锁被其他线程持有，则该线程进入阻塞状态
2. 当线程等待另一个线程通知调度器一个条件时，它自己进入等待状态

`被阻塞状态与等待状态是有很大不同的`

#### Timed waiting（计时等待）

有几个方法有一个超时参数。调用它们导致线程进入计时等待（timed waiting）状态。这一状态将一直保持到超时期满或者接收到适当的通知。

#### Terminated（被终止）

- 因为run方法正常退出而自然死亡。

- 因为一个没有捕获的异常终止了run方法而意外死亡。

### 线程属性

#### 线程优先级

每当线程调度器有机会选择新线程时，它首先选择具有较高优先级的线程

·`static void yield（）`导致当前执行线程处于让步状态。如果有其他的可运行线程具有至少与此线程同样高的优先级，那么这些线程接下来会被调度。注意，这是一个静态方法。

#### 守护线程

可以通过调用`void setDaemon（boolean isDaemon）true`来将线程转换为守护线程,这一方法必须在线程启动之前调用

### 同步

**“如果向一个变量写入值，而这个变量接下来可能会被另一个线程读取，或者，从一个变量读值，而这个变量可能是之前被另一个线程写入的，此时必须使用同步”**

#### 锁对象

##### 基本结构

![image-20200724171251066](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200724171251066.png)

- 这一结构确保任何时刻只有一个线程进入临界区。一旦一个线程封锁了锁对象，其他任何线程都无法通过lock语句。当其他线程调用lock时，它们被阻塞，直到第一个线程释放锁对象。
- 警告：把解锁操作括在finally子句之内是至关重要的。如果在临界区的代码抛出异常，锁必须被释放。否则，其他线程将永远阻塞

##### 其他

锁是可重入的，因为线程可以重复地获得已经持有的锁。锁保持一个持有计数（hold count）来跟踪对lock方法的嵌套调用。线程在每一次调用lock都要调用unlock来释放锁。由于这一特性，被一个锁保护的代码可以调用另一个使用相同的锁的方法。

#### 条件对象

##### 定义

通常，线程进入临界区，却发现在某一条件满足之后它才能执行。要使用一个条件对象(条件变量)来管理那些已经获得了一个锁但是却不能做有用工作的线程

##### 其他

- 注意注意死锁



#### synchronized关键字

·锁用来保护代码片段，任何时刻只能有一个线程执行被保护的代码。

·锁可以管理试图进入被保护代码段的线程。

·锁可以拥有一个或多个相关的条件对象。

·每个条件对象管理那些已经进入被保护的代码段但还不能运行的线程。

每一个对象有一个内部锁，并且该锁有一个内部条件。由锁来管理那些试图进入synchronized方法的线程，由条件来管理那些调用wait的线程。

- wait、notifyAll以及notify方法是Object类的final方法。Condition方法必须被命名为await、signalAll和signal以便它们不会与那些方法发生冲突

##### 局限

```markdown
1. 不能中断一个正在试图获得锁的线程。
2. 试图获得锁时不能设定超时。
3. 每个锁仅有单一的条件，可能是不够的
```



#### 同步阻塞

```markdown
每一个Java对象有一个锁。线程可以通过调用同步方法获得锁。还有另一种机制可以获得锁，通过进入一个同步阻塞
```



##### 格式

![image-20200725094806629](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200725094806629.png)

![image-20200725094818456](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200725094818456.png)

#### 监视器

##### 特性

```markdown
1. 监视器是只包含私有域的类。
2. 每个监视器类的对象有一个相关的锁。
3. 使用该锁对所有的方法进行加锁
4. 该锁可以有任意多个相关条件
```

#### Volatile域

volatile关键字为实例域的同步访问提供了一种免锁机制。如果声明一个域为volatile，那么编译器和虚拟机就知道该域是可能被另一个线程并发更新的。

Volatile变量不能提供原子性

#### 死锁

##### 定义

```markdown
所有线程都被阻塞。这样的状态称为死锁（deadlock）。
```



#### 读写锁

##### 步骤

![image-20200725102134766](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20200725102134766.png)

### 阻塞队列

![image-20201106174925120](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201106174925120.png)

### 线程安全的集合

```markdown
1. 饿汉式(线程安全，调用效率高，但是不能延时加载)； 
2. 懒汉式(线程安全，调用效率不高，但是能延时加载)；
3. Double CheckLock实现单例：DCL也就是双重锁判断机制（由于JVM底层模型原因，偶尔会出问题，不建议使用）； 
4. 静态内部类实现模式（线程安全，调用效率高，可以延时加载）；
5. 枚举类（线程安全，调用效率高，不能延时加载，可以天然的防止反射和反序列化调用）。
```



## 网络编程

### 端口

#### 端口分类

### 通信协议:

- 公有端口:0-1023
  - HTTP:80
  - HTTPS:443
  - FTP: 21

```bash
netstat -ano #查看所有端口
netstat -ano|findstr "5900" #查看指定端口
tasklist|findstr"8696" #查看指定端口的进程

```

#### Tcp/Ip协议簇

> 实际上是一组协议

重要:

- TCP:

  - 连接,稳定

  - 三次握手,四次挥手

    - ```
      
      ```

    - 

  - 客户端,服务端

  - 传输完成,释放连接,效率低

- UDP

出名的协议:

- TCP
- IP

### 用TCP协议通信

> 1. 服务器端运用`serversocket`监听指定端口,`serversocket.accept`接受客户端的`socket`,然后再通过流传输信息

### 用UDP协议通信

> 

## lambda语法

> 作用:简化代码,

**方法引用:**双冒号语法

![image-20201024100453176](https://raw.githubusercontent.com/TestLove/Pictures/main/img/lambda.png)

## JavaBean

> 只是一种类的规范

## Swing

JFrame 类的常用构造方法如下所示。

- JFrame()：构造一个初始时不可见的新窗体。
- JFrame==(String title)==：创建一个具有 title 指定标题的不可见新窗体。

# 辅助工具使用

## Hutool

### 概览

> ![image-20200926105148907](https://raw.githubusercontent.com/TestLove/Pictures/main/img/ilambda.png)

### 引入

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-all</artifactId>
    <version>5.4.3</version>
</dependency>
```

## [easyExcel](https://www.yuque.com/easyexcel/doc/easyexcel)

### 引入

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>easyexcel</artifactId>
    <version>2.2.6</version>
</dependency>
```

# 一些疑惑

1. [InputStream是抽象类](https://www.codenong.com/13670991/),但为什么会有其的实例例如`InputStream in= socket.getInputStream();`

   > 这里的变量"in"只保存对一个对象的引用，该对象可以满足"is a inputstream"的要求。这意味着变量"in"可以容纳扩展输入流的具体类的任何瞬间，例如audioinputstream，它满足条件"is a inputstream"。我认为这与多态性有关。
   >
   > 抽象类不能被实例化，但可以被子类化,当然,也可以通过[匿名内部类](https://www.cnblogs.com/nerxious/archive/2013/01/25/2876489.html)来达到这种目的