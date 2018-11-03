@[toc]
# 引言
为了java面试，我刷了不少的java笔试题，这个是一个比较简短的总结笔记，希望能帮到有需要的人。这篇博文其实只写了一些，我内心还有更大的蓝图(集合啊线程那些还有一些其他的考点)，只不过最近比较忙没写~以后再更新啦。
如果您觉的有用的话，可以给我点个赞(star)！！！如果文章有错误请指正。如果您需要转载的话，请在明显位置(加大加黑哈哈)注明转载地址。GitHub地址为：[GitHub跳转](https://github.com/Silverados/java_Interview)
# 一、数据类型

## 基本数据类型

### 数值类型：
数据类型 | 封装类型 | 存储需求(字节) | 取值范围=(-2^(8*存储需求)~2^(8*存储需求)-1)
-- | -- | -- | --
byte | Byte | 1 | -2^8~2^7-1(-128~127)
short | Short | 2 | 
int | Integer | 4 | 
long | Long | 8 |
float | Float | 4 |
double | Double | 8 |

以整型int 4字节来计算，4字节也就是32位，1KB=1024B，1024/32=32，也就是1KB最多能存32个int。

**面试考点：取值范围，运算，拆装箱，比较，类型转换**


#### 赋值
需要注意的一点是一些不常见的赋值方法，这个也要结合下面类型转换来看。

首先要明确的一点是**整型默认类型为int，浮点型默认类型为double**，

然后要考虑**类型的取值范围**，

再考虑一点就是下面的表现形式。

##### 数值的表现形式
1. 进制类：例如2,8,16进制。
```java
int a = 10;
int b = 0b10;
int c = 010;
int d = 0x10;
System.out.println("a = "+a+" b = "+b+" c = "+c+" d = "+d);
//a = 10 b = 2 c = 8 d = 16
```

2. 科学计数法：要注意的是使用科学计数法则元素为double类型。
```java
 double d = 5e2;
 int i = (int) 5e2;
 System.out.println(d);//500.0
```
3. 后缀类：float->f,double->d;结合默认类型和自动类型转换考虑。
```java
float f = (float)1.1;
float ff = 1.1f;
double e = 1.1d;
```

##### 其他
偶尔有些题目还会有封装类型的直接赋值，这时候考虑第一点默认类型处理。
```java
Double d = 1;//(x)因为1默认是int，这里直接赋值是错的。
```

#### 运算and类型转换

![在这里插入图片描述](https://note.youdao.com/yws/public/resource/0ce9be643957abf2f363116671a58c86/3D23F6994BE44306A39DB05588AB77DA?ynotemdtimestamp=1541249418219)
实心箭头代表无信息丢失，虚箭头表示有精度损失

补充：5、被final修饰的变量不会自动改变类型，当2个final修饰相操作时，结果会根据左边变量的类型而转化。 

案例：
```java
final byte b = 1;
byte j = b + 1;
```


**注意：结合赋值会隐式进行强制类型转换**

案例：
```java
byte i = 1;
i = i + 1;//报错，因为运算的时候byte转换成了int类型
i = (byte)(i + 1);//可以，使用了强制类型转换
i += 1;//可以，这种结合赋值会隐式进行强制类型转换
```


**位运算里：`~n = -n - 1`**

#### 自动装拆箱and比较
自动装箱和拆箱大部分人的印象应该就是这样子的：
```java
Integer i = 1;//自动将int类型装箱为Integer
int a = new Integer(5);//自动拆箱为int
```

那么**面试的考点**应该是下面这样子的：

1. 基本型和基本型封装型进行`==`运算符的比较，基本型封装型将会自动拆箱变为基本型后再进行比较。
2. 两个Integer类型进行“==”比较，还要看Integer对象创建的方式，这里还有一个缓存池的概念。Integer缓存的是-128~127
3. 两个基本型的封装型进行equals()比较，首先equals()会比较类型，如果类型相同，则继续比较值，如果值也相同，返回true
4. 基本型封装类型调用equals(),但是参数是基本类型，这时候，先会进行自动装箱，基本型转换为其封装类型，再进行3中的比较。

```java
Integer i = 9;
int j = 9;
System.out.println(i == j);//true,i自动拆箱，然后进行比较

Integer k = 9;
Integer kk = new Integer(9);
Integer kkk = Integer.valueOf(9);
System.out.println(i == k);//true。i,k创建方式不是new，而且值在缓存池里，所以==比较对象的会是同一个地址
System.out.println(i == kk);//false。kk是new的，所以内存地址不一样。
System.out.println(i == kkk);//true。如果值在缓存池里valueOf会直接返回对应缓存，否则调用new Integer

Integer ii = 128;
Integer jj = 128;
System.out.println(ii == jj);//false.如果值在缓存池里valueOf会直接返回对应缓存，否则调用new Integer

System.out.println(i.equals(k));//方法为public boolean equals(Object obj)
System.out.println(i.equals(kk));//
```

基本类型对应的缓冲池如下：

    boolean values true and false
    all byte values
    short values between -128 and 127
    int values between -128 and 127
    char in the range \u0000 to \u007F


### boolean类型：
封装类型：Boolean,取值为true|false.

**面试考点：逻辑符的短路**

#### 逻辑符的短路：
逻辑符的短路表示按顺序执行`&&,||`时，如果前面语句满足了判断那么就不会执行后面的语句：
```java
if(true||5/0==0){
    ...
}
```
执行前半句得到true，逻辑短路不会执行后面的语句，故不会报错。

实际上位运算符`&,|`也可以表示**与,或**,代表的是一种非短路的运算。如果上述条件改为`|`那么就会报错。


### char类型：
char原本用于表示单个字符。现在可以描述一些Unicode字符。封装类型为：Character，取值范围现在是0~2^16-1.

**面试考点：封装类型，常见char对应int值**

字符 | ASCII码值
-- | --
'0'|48
'a'|97
'A'|65

注意：当赋值为整型的时候不是代表对应的值，而是ASCII值：
```java
char a = 1;
char b = '1';//a!=b
```

![在这里插入图片描述](https://note.youdao.com/yws/public/resource/0ce9be643957abf2f363116671a58c86/938FC3CC8E3C492DBEB477FD92B0A196?ynotemdtimestamp=1541249418219)

## String类型
**string的面试考点一般有 判断是否相等，equals(),不可变性，StringBuffer/StringBuilder的区别**
### 内部实现
String类被声明为 final，因此它不可被继承。
而且内部使用 char 数组存储数据，该数组被声明为 final，这意味着 value 数组初始化之后就不能再引用其它数组。并且 String 内部没有改变 value 数组的方法，因此可以保证 **String 不可变。**
```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
    
```
### 常用的String的创建
1. `String s = "java";`我们把这类创建称为**字面量** 方式，直接使用双引号将字符数组value[]括起来。
2. `String s = new String("java");`使用构造函数构造String对象。

之所以要区分这两种不同创建方式是因为他们在创建String对象的时候会有不同的表现形式。

#### 字面量创建：
使用字面量的方式创建，首先JVM会到字符串常量池中检查是否有内容为`java`的对象存在。

如果不存在，那么会先在在**堆内存**创建一个内容为`java`的String对象，然后将指向该对象的内存地址引用存入**字符串常量池中**。

如果存在则返回该引用。


如果是`String s = "ja"+"va";`这样的方式创建字符串，由于编译优化，那么在编译器会直接在字符串常量池创建内容为`java`的引用，而没有`ja,va`这两个。

但是如果是`String s1 = "ja";String s = s1 + "va";`由于无法在编译器确定s的内容，jvm会在运行时确认。而在运行时确认需遵循以下规则：只要s1是变量，不论s1指向池中的字符串对象还是堆中的字符串对象，运行期s1 + “aa”操作实际上是编译器创建了StringBuilder对象进行了append操作后通过toString()返回了一个字符串对象存在heap上。


    在JDK6.0及之前版本，字符串常量池是放在Perm Gen区(也就是方法区)中；
    在JDK7.0版本，字符串常量池被移到了堆中了。因为方法区的内存太小，只能存放1009个，而现在可以通过指定-XX:StringTableSize参数初始化字符串常量池的大小。
    
    
#### 构造方法创建：
使用构造方法创建时，不过字符串常量池是否有相应的内容的String对象引用，都会在堆上新建一个String对象。

对于上面使用new创建的字符串对象，如果想将这个对象的引用加入到字符串常量池，可以使用intern方法。

调用intern后，首先检查字符串常量池中是否有该对象的引用，如果存在，则将这个引用返回给变量，否则将引用加入并返回给变量。


#### 案例：
```java
String a = "java";
String b = "java";
String c = new String("java");
String cc = String.valueOf("java");
String ccc = new StringBuilder("java").toString();
String d = "ja"+"va";
System.out.println(a == b);//true
System.out.println(a == d);//true
System.out.println(a == c);//false
System.out.println(a == cc);//true
System.out.println(a == ccc);//false
```

### StringBuffer和StringBuiler：
1. StringBuffer线程安全，StringBuiler线程非安全。
2. StringBuiler牺牲了对方法的互斥锁，所以运行速率比StringBuffer高。
3. 都继承了AbstractStringBuilder。

# 二.修饰符
## 权限修饰符
### 基础
权限修饰符修饰类，方法，字段

权限修饰符有3种，分别是public(本包，其他包可见),protected(包内和子类可见),private(类内私有)。

权限除了上面3中，还有一种是不带修饰符的，也可以称为缺省的，表示包内可见。

按权限大小划分为**public > protected > 缺省 > private.**

### 继承
继承里，关于权限修饰符有一条限制：**子类的重载方法访问权限要等于或者大于父类的权限。**

如果子类继承接口，那么如果要**实现接口的方法，方法的修饰符一定是public**，因为接口中方法默认为public abstract.


## final修饰符
final修饰方法表示子类不能覆盖。

final修饰类表示该类不能被继承。

final修饰字段表示该字段时常量。

final修饰参数表示该参数不能被重复赋值。

被final修饰的变量不会自动改变类型，当2个final修饰相操作时，结果会根据左边变量的类型而转化。 

### blank final
空的final，即不给变量赋初值，只有在构造某个类的对象时才（必须）给它赋一次初值。
类里的不同对象可以是不同的final值，而且保留了不可变的特性。
```java
    public final String helloString = "Hello";
	public final String javaString;
	public FinalTest() {
		javaString = "java";
	}
```

## static修饰符
java类只有在第一次被使用时才会被加载，而static修饰的变量，方法，区块只会被初始化一次。即不同的实例对象如果对同一个static修饰的变量做修改，其结果是共享的。

**static修饰代表是类本身的**

使用类名可以直接访问static对象。对象实例也可以但不推荐。
```java
public class Random{
    private static int i = 5;
    public static void main(String[] args){
        Random random = new Random();
        System.out.println(Random.i++);//5
        System.out.println(random.i);//6
    }
}
```

### 多态继承
1）子类是不继承父类的static变量和方法的。因为这是属于类本身的。但是子类是可以访问的。
2）子类和父类中同名的static变量和方法都是相互独立的，并不存在任何的重写的关系。

## abstract修饰符
abstract使用在抽象类的声明，抽象方法的声明中。

**要注意的是不能和private.final结合使用**

# 三.类和接口

## 普通类
一个文件里可以有很多类，但是一定会有一个类名和文件名一样的类，只有与文件名字相同的这个普通类才可以用public来修饰。

### 外部类
外部类是相对于普通类而言的，而不是相对于内部类而言！

**外部类不能有权限修饰符，只能由final或者abstract修饰。**

例如Demo.java中：Outer为外部类
```java
public class Demo {

}

class Outer{

}
```
### 内部类
1. 内部类中声明的所有静态域都必须是final的
2. 内部类不能有static方法
```java
public class Demo {
    public static void main(String[] args){
        System.out.println("\u263A");

        class LocalInner{

        }//局部内部类

        Thread t = new Thread(new Runnable() {
            @Override
            public void run() {

            }
        });//匿名内部类

    }

    static class staticInner{

    }//静态内部类

    class Inner{

    }//普通内部类
}
```

#### 普通内部类
可以使用权限修饰符，final,abstract

#### 局部内部类
只能使用final

#### 匿名内部类
无内部类名称，无任何修饰符

#### 静态内部类
可以使用权限修饰符，final,abstract。相当于普通内部类+static。

静态内部类的对象除了没有对生成它的外围类对象的引用特权外，和其他内部类一样。

静态内部类可以有静态域和方法。

声明在接口中内部类自动成为static和public类。

## 抽象类
1. 抽象类使用abstract修饰class，子类不能实例化
2. 类中有抽象方法那么类一定要是抽象类，然而如果类是抽象类可以没有抽象方法。
3. 抽象类除了有抽象方法和子类不能实例化外和其他普通类类似。

## 接口
1. 接口使用interface修饰
2. 接口中字段自动定义为public static final，方法自动定义为public。规范中建议不书写这些多余关键字。
3. java8起接口中可以存在静态方法，和以default修饰的默认方法。

# 四、异常
## 受查异常和非受查异常
RunTimeException是非受查异常，不能try-catch但是应该尽量在编写程序中避免，其中包括空指针异常、数组下标越界异常等。

对于受查异常，使用try-catch捕捉。注意：多个catch语句应按子到父类异常抛出。

![在这里插入图片描述](https://note.youdao.com/yws/public/resource/0ce9be643957abf2f363116671a58c86/D314944E06E4498696526B6E6261940E?ynotemdtimestamp=1541249418219)
![在这里插入图片描述](https://note.youdao.com/yws/public/resource/0ce9be643957abf2f363116671a58c86/484FAC9B05374FE3AA640DE58B544E97?ynotemdtimestamp=1541249418219)
## 带资源的try
也称为ARM(Automatic Resource Management，自动资源管理)

带资源的try会在退出try语句块时自动关闭资源。而不用再另外在finally块写close()。

多个资源用`;`隔开。
**try必须和catch或者finally使用。**
```java
try(InputStream in = new InputStream()){
    
}catch(Exception e){
    
}
```

## try-catch-finally的逻辑
```java
try{
    
}catch(Exception e){
    
}finally{
    
}
```
在try语句块和catch语句块中，不过是否有return或者throw语句，finally语句一定会执行。

如果try里有return语句且执行到该语句时，首先会进入finally执行finally语句块的逻辑，如果finally里有return语句，那么会覆盖try里的return。

如果try里有异常抛出，立即跳转到catch块执行catch块逻辑，即使catch块里重新调用了throw抛出异常，仍然会执行finally语句块。执行完finally后中断当前块向上抛出异常。


## Error
Error描述的是JAVA运行时系统的内部错误和资源耗尽错误。这里主要提两个Error。

### OutOfMemoryError
当JVM在给对象分配内存时，容量不足那么会抛出这个异常。

导致OutOfMemoryError异常的**常见原因**有以下几种：

- 内存中加载的数据量过于庞大，如一次从数据库取出过多数据；
- 集合类中有对对象的引用，使用完后未清空，使得JVM不能回收；
- 代码中存在死循环或循环产生过多重复的对象实体；
- 使用的第三方软件中的BUG；
- 启动参数内存值设定的过小；

解决方式：
- 增加jvm的内存大小。使用-Xmx设置内存大小

需要重点排查以下几点：

- 检查代码中是否有死循环或递归调用。
- 检查是否有大循环重复产生新对象实体。
- 检查对数据库查询中，是否有一次获得全部数据的查询。一般来说，如果取十万条记录到内存，就可能引起内存溢出。这个问题比较隐蔽，在上线数据库中数据较少，不容易出问题，上线后，数据库中数据多了，一次查有可能引起内存溢出。因此对于数据库查询尽量采用分页的方式查询。
- 检查List、MAP等集合对象是否有使用完后，未清除的问题。List、MAP等
- 对象会始终存有对对象的引用，使得这些对象不能被GC回收。


### StackOverflowError
由于应用程序递归太深而发生堆栈溢出时引发。死递归。

解决方案：
- 修改栈大小参数：-Xss
- 检查代码中是否有死循环或递归调用。



# 集合
集合类框架
![在这里插入图片描述](https://note.youdao.com/yws/public/resource/0ce9be643957abf2f363116671a58c86/68892111BAAB4DC68A15BD637BFA5CBA?ynotemdtimestamp=1541249418219)
![image](https://note.youdao.com/yws/res/9351/596EC1952BF14985B992929D1979BEC2)
![在这里插入图片描述](https://note.youdao.com/yws/public/resource/0ce9be643957abf2f363116671a58c86/596EC1952BF14985B992929D1979BEC2?ynotemdtimestamp=1541249418219)

接口框架
![在这里插入图片描述](https://note.youdao.com/yws/public/resource/0ce9be643957abf2f363116671a58c86/8896C20746084F7CA30C3524F9DAFB07?ynotemdtimestamp=1541249418219)

![在这里插入图片描述](https://note.youdao.com/yws/public/resource/0ce9be643957abf2f363116671a58c86/3991823985A14F9EACDCC22569523D27?ynotemdtimestamp=1541249418219)
- ArrayList:一种可以动态增长和缩减的索引描述
- LinkedList:一种可以在任何位置进行高效插入和删除操作的有序序列
- ArrayDeque:一种用循环数组实现的双端队列
- HashSet:一种没有重复元素的无序集合
- TreeSet:一种有序集
- EnumSet:一种包含枚举类型值的集
- LinkedHashSet:一种可以记住元素插入次序的集合
- PriorityQueue:一种允许高效删除最小元素的集合
- HashMap:一种存储键值对的数据结构
- EnumMap:一种键值属于枚举类型的映射表
- LinkedHashMap:一种可以记住键值添加次序的映射表
- WeakHashMap:一种其值无用武之地后可以被GC回收的映射表
- IdentityHashMap:一种用==而不是equals比较键值的映射表


## ArrayList
- ArrayList基于数组实现
- 默认数组的大小为10，扩容后大小为原本的1.5倍。
- 触发条件为add,addAll()方法的调用，调用Arrays.copyOf()进行数组的扩容/缩小
- 删除元素调用 System.arraycopy()
- 可以根据索引来进行查找。
- 不是线程安全的。可以使用 Collections.synchronizedList(); 得到一个线程安全的 ArrayList。
```java
List<String> list = new ArrayList<>();
List<String> synList = Collections.synchronizedList(list);
```

## LinkedList
- LinkedList使用链表实现
- 在插入和删除上的速率很快



# 杂项

## 关键字
主要记住这两点：\
**true、false、null都不是关键字\
goto、const、是保留的关键字**

关键字列表：
```

abstract                continue           for            new        
switch                  default            if             package     
synchronized            do                 goto           private     
this                    break              double         implements   
protected               throw              byte           else       
import                  public             throws         case       
enum                    instanceof         return         transient  
catch                   extends            int            short       
try                     char               final          interface    
static                  void               class          finally   
long                    strictfp           volatile       const      
float                   native             super          while
boolean                 assert 
```
