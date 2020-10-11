### 基本数据类型

1.  数值型  整数型（byte，short，int，long） 

   ​              浮点型（float，double）

   ​              布尔型  boolean 

   ​              字符型  char

2. 引用数据类型  

   (1)类  class  (2) 接口 interface  (3)数组 []        

   Java 5 以后引入的枚举类型也算是一种比较特殊的引用类型。

#### float f=3.4  错误 

​    3.4  默认为edouble型，转为float单精度 会有精度损失 报错 
​    可以强制类型转化 folat f=3.4f
​    编译器可以自动向上转型，如int 转成 long 系统自动转换没有问题，因为后者精度更高  long l=4

#### 位运算  左移 和右移

 2 << 3  ==2*8 

（左移 3 位相当于乘以 2 的 3 次方，右移 3 位相当于除以 2 的 3 次方）

#### short s1 = 1; s1 = s1 + 1;有错吗?short s1 = 1; s1 += 1;有错吗

对于 short s1 = 1; s1 = s1 + 1;由于 1 是 int 类型，因此 s1+1 运算结果也是 int型，需要强制转换类型才能赋值给 short 型。

而 short s1 = 1; s1 += 1;可以正确编译，因为 s1+= 1;相当于 s1 = (short(s1 + 1);其中有隐含的强制类型转换。

#### final finally finalize区别

- final可以修饰类、变量、方法，修饰类表示该类不能被继承、修饰方法表示该方法不能被重写、修饰变量表
  示该变量是一个常量不能被重新赋值。

- finally一般作用在try-catch代码块中，在处理异常的时候，通常我们将一定要执行的代码方法finally代码块
  中，表示不管是否出现异常，该代码块都会执行，一般用来存放一些关闭资源的代码。

- finalize是一个方法，属于Object类的一个方法，而Object类是所有类的父类，该方法一般由垃圾回收器来调
  用，当我们调用System.gc() 方法的时候，由垃圾回收器调用finalize()，回收垃圾，一个对象是否可回收的
  最后判断。


### 面向对象

#### 重写与重载

##### 构造器（constructor）是否可被重写（override）

构造器不能被继承，因此不能被重写，但可以被重载。

##### 重载（Overload）和重写（Override）的区别。重载的方法能否根据返回类型进行区分？

方法的重载和重写都是实现多态的方式，区别在于前者实现的是编译时的多态性，而后者实现的是运行时的多态性。

重载：发生在同一个类中，方法名相同参数列表不同（参数类型不同、个数不同、顺序不同），与方法返回值和访问修饰符无关，即重载的方法不能根据返回类型进行区分

重写：发生在父子类中，方法名、参数列表必须相同，返回值小于等于父类，抛出的异常小于等于父类，访问修饰符大于等于父类（里氏代换原则）；如果父类方法访问修饰符为private则子类中就不是重写。

#### Java 面向对象编程三大特性：封装 继承 多态

**封装**：隐藏对象的属性和实现细节，仅对外提供公共访问方式，将变化隔离，便于使用，提高复用性和安全性。

**继承**：继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承可以提高代码复用性。继承是多态的前提。

**关于继承如下 3 点请记住**：

1. 子类拥有父类非 private 的属性和方法。
2. 子类可以拥有自己属性和方法，即子类可以对父类进行扩展。
3. 子类可以用自己的方式实现父类的方法。

**多态性**：父类或接口定义的引用变量可以指向子类或具体实现类的实例对象。提高了程序的拓展性。

在Java中有两种形式可以实现多态：继承（多个子类对同一方法的重写）和接口（实现接口并覆盖接口中同一方法）。

方法重载（overload）实现的是编译时的多态性（也称为前绑定），而方法重写（override）实现的是运行时的多态性（也称为后绑定）。

一个引用变量到底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。运行时的多态是面向对象最精髓的东西，要实现多态需要做两件事：

- 方法重写（子类继承父类并重写父类中已有的或抽象的方法）；
- 对象造型（用父类型引用子类型对象，这样同样的引用调用同样的方法就会根据子类对象的不同而表现出不同的行为）。



### 反射

#### 什么是反射机制？

JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。

静态编译和动态编译

- **静态编译：**在编译时确定类型，绑定对象
- **动态编译：**运行时确定类型，绑定对象

#### 反射机制优缺点

- **优点：** 运行期类型的判断，动态加载类，提高代码灵活度。
- **缺点：** 性能瓶颈：反射相当于一系列解释操作，通知 JVM 要做的事情，性能比直接的java代码要慢很多。

#### 反射机制的应用场景有哪些？

反射是框架设计的灵魂。

在我们平时的项目开发过程中，基本上很少会直接使用到反射机制，但这不能说明反射机制没有用，实际上有很多设计、开发都与反射机制有关，例如模块化的开发，通过反射去调用对应的字节码；动态代理设计模式也采用了反射机制，还有我们日常使用的 Spring／Hibernate 等框架也大量使用到了反射机制。

举例：①我们在使用JDBC连接数据库时使用Class.forName()通过反射加载数据库的驱动程序；②Spring框架也用到很多反射机制，最经典的就是xml的配置模式。Spring 通过 XML 配置模式装载 Bean 的过程：1) 将程序内所有 XML 或 Properties 配置文件加载入内存中; 2)Java类里面解析xml或properties里面的内容，得到对应实体类的字节码字符串以及相关的属性信息; 3)使用反射机制，根据这个字符串获得某个类的Class实例; 4)动态配置实例的属性

#### Java获取反射的三种方法

1.通过new对象实现反射机制 2.通过路径实现反射机制 3.通过类名实现反射机制

```java
public class Student {
    private int id;
    String name;
    protected boolean sex;
    public float score;
}

public class Get {
    //获取反射机制三种方式
    public static void main(String[] args) throws ClassNotFoundException {
        //方式一(通过建立对象)
        Student stu = new Student();
        Class classobj1 = stu.getClass();
        System.out.println(classobj1.getName());
        //方式二（所在通过路径-相对路径）
        Class classobj2 = Class.forName("fanshe.Student");
        System.out.println(classobj2.getName());
        //方式三（通过类名）
        Class classobj3 = Student.class;
        System.out.println(classobj3.getName());
    }
}
```

### String相关

#### 字符型常量和字符串常量的区别

1. 形式上: 字符常量是单引号引起的一个字符 字符串常量是双引号引起的若干个字符
2. 含义上: 字符常量相当于一个整形值(ASCII值),可以参加表达式运算 字符串常量代表一个地址值(该字符串在内存中存放位置)
3. 占内存大小 字符常量只占一个字节 字符串常量占若干个字节(至少一个字符结束标志)

#### String 底层

String 底层就是一个 char 类型的数组，只是使用的时候开发者不需要直接操作底层数组，用更加简便的方式即可完成对字符串的使用。

#### 字符串常量池

字符串常量池位于堆内存中，专门用来存储字符串常量，可以提高内存的使用率，避免开辟多块空间存储相同的字符串。

**在创建字符串时 JVM 会首先检查字符串常量池，如果该字符串已经存在池中，则返回它的引用，如果不存在，则实例化一个字符串放到池中，并返回其引用。**

```java
String s1="hello";
String s2="hello";
System.out.println(s1==s2); //结果为 ture
```



#### String有哪些特性

- **不变性**：String 是只读字符串，是一个典型的 immutable 对象，对它进行任何操作，其实都是创建一个新的对象，再把引用指向该对象。不变模式的主要作用在于当一个对象需要被多线程共享并频繁访问时，可以保证数据的一致性。
- **常量池优化**：String 对象创建之后，会在字符串常量池中进行缓存，如果下次创建同样的对象时，会直接返回缓存的引用。
- **final**：使用 final 来定义 String 类，表示 String 类不能被继承，提高了系统的安全性。

##### 题目

##### 1.String str="i"与 String str=new String(“i”)一样吗？

不一样，因为内存的分配方式不一样。String str="i"的方式，java 虚拟机会将其分配到常量池中；而 String str=new String(“i”) 则会被分到堆内存中。

##### 2.String s = new String(“xyz”);创建了几个字符串对象

两个对象，一个是静态区的"xyz"，一个是用new创建在堆上的对象。

##### 3.在使用 HashMap 的时候，用 String 做 key 有什么好处？

HashMap 内部实现是通过 key 的 hashcode 来确定 value 的存储位置，因为字符串是不可变的，所以当创建字符串时，它的 hashcode 被缓存下来，不需要再次计算，所以相比于其他对象更快。

#### String  String Builder String Buffer三者使用的总结

1.如果要操作少量的数据用 = String

2.单线程操作字符串缓冲区 下操作大量数据 = StringBuilder

3.多线程操作字符串缓冲区 下操作大量数据 = StringBuffer

### 包装类相关

#### 自动装箱与拆箱

**装箱**：将基本类型用它们对应的引用类型包装起来；

**拆箱**：将包装类型转换为基本数据类型；

#### int 和 Integer 有什么区别

Java 是一个近乎纯洁的面向对象编程语言，但是为了编程的方便还是引入了基本数据类型，但是**为了能够将这些基本数据类型当成对象操作**，Java 为每一个基本数据类型都引入了对应的包装类型（wrapper class），int 的包装类就是 Integer，从 Java 5 开始引入了自动装箱/拆箱机制，使得二者可以相互转换。

Java 为每个原始类型提供了包装类型：

原始类型: boolean，char，byte，short，int，long，float，double

包装类型：Boolean，Character，Byte，Short，Integer，Long，Float，Double

#### Integer a= 127 与 Integer b = 127相等吗

对于对象引用类型：==比较的是对象的内存地址。
对于基本数据类型：==比较的是值。

*如果整型字面量的值在-128到127之间，那么自动装箱时不会new新的Integer对象，而是直接引用常量池中的Integer对象，超过范围 a1==b1的结果是false*

```java
public static void main(String[] args) {
    Integer a = new Integer(3);
    Integer b = 3;  // 将3自动装箱成Integer类型
    int c = 3;
    System.out.println(a == b); // false 两个引用没有引用同一对象
    System.out.println(a == c); // true a自动拆箱成int类型再和c比较
    System.out.println(b == c); // true

    Integer a1 = 128;
    Integer b1 = 128;
    System.out.println(a1 == b1); // false

    Integer a2 = 127;
    Integer b2 = 127;
    System.out.println(a2 == b2); // true
}
```

### Lmabada 表达式

#### 简介

Lambda表达式（也称**闭包**），是Java8中最受期待和欢迎的新特性之一。在Java语法层面Lambda表达式允许函数作为一个方法的参数（函数作为参数传递到方法中），或者把代码看成数据。Lambda表达式可以简化函数式接口的使用。**函数式接口就是一个只具有一个抽象方法的普通接口**，像这样的接口就可以使用Lambda表达式来简化代码的编写。

**条件**：只有函数式接口（有且只有一个抽象方法的接口）可以使用lambada表达式

#### 使用Lambda表达式的优缺点

**优点**

使用Lambda表达式可以简化接口匿名内部类的使用，可以减少类文件的生成，可能是未来编程的一种趋势。

**缺点**

使用Lambda表达式会减弱代码的可读性，而且Lambda表达式的使用局限性比较强，只能适用于接口只有一个抽象方法时使用，不宜调试。

```java
public class Demo01 {

    public static void main(String[] args) {

        // 1.传统方式 需要new接口的实现类来完成对接口的调用
        ICar car1 = new IcarImpl();
        car1.drive();

        // 2.匿名内部类使用
        ICar car2 = new ICar() {
            @Override
            public void drive() {
                System.out.println("Drive BMW");
            }
        };
        car2.drive();

        // 3.无参无返回Lambda表达式
        ICar car3 = () -> {System.out.println("Drive Audi");};
        car3.drive();

        // 4.无参无返回且只有一行实现时可以去掉{}让Lambda更简洁
        ICar car4 = () -> System.out.println("Drive Ferrari");
        car4.drive();

        // 去查看编译后的class文件 大家可以发现 使用传统方式或匿名内部类都会生成额外的class文件，而Lambda不会
    }

}

interface ICar {

    void drive();
}

class IcarImpl implements ICar {

    @Override
    public void drive() {
        System.out.println("Drive Benz");
    }
}
```

### 集合

#### ArrayList、LinkedList、Vector 的区别

|              | **ArrayList**                                                | LinkedList                 |       **Vector**       |
| ------------ | ------------------------------------------------------------ | -------------------------- | :--------------------: |
| 底层实现     | 数组                                                         | 双向链表                   |          数组          |
| 同步性及效率 | 不同步，非线程安全，效率高，支持随机访问                     | 不同步，非线程安全，效率高 | 同步，线程安全，效率低 |
| 特点         | 查询快，增删慢                                               | 查询慢，增删快             |     查询快，增删慢     |
| 默认容量     | 10                                                           | /                          |           10           |
| 扩容机制     | int newCapacity = oldCapacity + (oldCapacity >> 1)；//1.5 倍 | /                          |          2 倍          |

#### HashMap解析

 JDK1.8之前采用的是拉链法。**拉链法**：将链表和数组相结合。也就是说创建一个链表数组，数组中每一格就是一个链表。若遇到哈希冲突，则将冲突的值加到链表中即可。

jdk1.8之后采用 **数组+链表+红黑树**。在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。

####  如何实现数组和 List 之间的转换？

- 数组转 List：使用 Arrays. asList(array) 进行转换。
- List 转数组：使用 List 自带的 toArray() 方法。

#### 多线程场景下如何使用 ArrayList？

ArrayList 不是线程安全的，如果遇到多线程场景，可以通过 Collections 的 synchronizedList 方法将其转换成线程安全的容器后再使用。例如像下面这样：

```java
List<String> synchronizedList = Collections.synchronizedList(list);
synchronizedList.add("aaa");
synchronizedList.add("bbb");

for (int i = 0; i < synchronizedList.size(); i++) {
    System.out.println(synchronizedList.get(i));
}
```

#### Collection

1. List

- Arraylist： Object数组
- Vector： Object数组
- LinkedList： 双向循环链表

1. Set

- HashSet（无序，唯一）：基于 HashMap 实现的，底层采用 HashMap 来保存元素
- LinkedHashSet： LinkedHashSet 继承与 HashSet，并且其内部是通过 LinkedHashMap 来实现的。有点类似于我们之前说的LinkedHashMap 其内部是基于 Hashmap 实现一样，不过还是有一点点区别的。
- TreeSet（有序，唯一）： 红黑树(自平衡的排序二叉树。)

Map

- HashMap： JDK1.8之前HashMap由数组+链表组成的，数组是HashMap的主体，链表则是主要为了解决哈希冲突而存在的（“拉链法”解决冲突）.JDK1.8以后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间
- LinkedHashMap：LinkedHashMap 继承自 HashMap，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，LinkedHashMap 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。
- HashTable： 数组+链表组成的，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的
- TreeMap： 红黑树（自平衡的排序二叉树）

#### 哪些集合类是线程安全的？

- vector：就比arraylist多了个同步化机制（线程安全），因为效率较低，现在已经不太建议使用。在web应用中，特别是前台页面，往往效率（页面响应速度）是优先考虑的。
- statck：堆栈类，先进后出。
- hashtable：就比hashmap多了个线程安全。
- enumeration：枚举，相当于迭代器。

#### Java集合的快速失败机制 “fail-fast”？

是java集合的一种错误检测机制，当多个线程对集合进行结构上的改变的操作时，有可能会产生 fail-fast 机制。

例如：假设存在两个线程（线程1、线程2），线程1通过Iterator在遍历集合A中的元素，在某个时候线程2修改了集合A的结构（是结构上面的修改，而不是简单的修改集合元素的内容），那么这个时候程序就会抛出 ConcurrentModificationException 异常，从而产生fail-fast机制。

原因：迭代器在遍历时直接访问集合中的内容，并且在遍历过程中使用一个 modCount 变量。集合在被遍历期间如果内容发生变化，就会改变modCount的值。每当迭代器使用hashNext()/next()遍历下一个元素之前，都会检测modCount变量是否为expectedmodCount值，是的话就返回遍历；否则抛出异常，终止遍历。

解决办法：

1. 在遍历过程中，所有涉及到改变modCount值得地方全部加上synchronized。
2. 使用CopyOnWriteArrayList来替换ArrayList

#### Iterator 迭代器 （用于遍历集合元素的接口）

将容器内部的取出方式按照一个统一的规则向外提供，这个规则就是Iterator接口，使得对容器的遍历操作与其具体的底层实现相隔离，达到解耦的效果。

Iterator 的特点是只能单向遍历，但是更加安全，因为它可以确保，在当前遍历的集合元素被更改的时候，就会抛出 ConcurrentModificationException 异常。

```
public static void main(String[] args) {
    List<String> list1 = new ArrayList<>();
    list1.add("abc0");
    list1.add("abc1");
    list1.add("abc2");

    // while循环方式遍历
    Iterator it1 = list1.iterator();
    while (it1.hasNext()) {
        System.out.println(it1.next());
    }

    // for循环方式遍历
    for (Iterator it2 = list1.iterator(); it2.hasNext(); ) {
        System.out.println(it2.next());
    }

}
```



### JSON (js对象表示法)

JSON(JavaScript Object Notation JavaScript 对象表示法)是一种轻量级的数据交换格式，它基于JavaScript的一个子集，易于人的编写和阅读，也易于机器解析。 JSON采用完全独立于语言的文本格式，但是也使用了类似于C语言家族的习惯（包括C, C++, C#, Java, JavaScript, Perl, Python等）。 这些特性使JSON成为理想的数据交换语言。

```java
{
    "name":"网站",
    "money":3.22,
    "status":0,
    "valid":true,
    "address":null,
    "sites": [
        { "name":"Google", "info":[ "Android", "Google 搜索", "Google 翻译" ] },
        { "name":"Runoob", "info":[ "菜鸟教程", "菜鸟工具", "菜鸟微信" ] },
        { "name":"Taobao", "info":[ "淘宝", "网购" ] }
    ]
}

```

### Request和Response  跳转的区别

#### 1. 用法   

请求转发：request.getRequestDispatcher("URL").forward(request,response)

重定向：    response.sendRedirect("URL")   

#### 2. 跳转方式

request是在服务器端跳转，他的路径是相对于项目的路径，只能跳转到项目内的一些地址

response是在客户端跳转，他的路径是相当于服务器的根路径，可以跳转到任何地址

比如form表单action="/xxx"

request跳转到http://loalhost:8080/webName/xxx, 

而response跳转到http://localhost:8080/xxx

#### 3. 数据共享

forward发生在服务器内部，在浏览器不知情的情况下发给另外一个页面的response,同时将request也传递过去，所以request是一样的，数据共享

redirect 会首先给即将跳转的的页面发一个response,（上一个页面的request没有同时发送给服务器） 然后新页面的浏览器收到这个response后再发一个request给服务器, 然后服务器发新的response给浏览器. 这时页面收到的request是一个新从浏览器发来的，并不是上一个页面传过来的，所以数据不能共享



#### 4. 请求次数

从3可以看出request跳转时客户端只发送一次请求，response跳转时，客户端发送两次请求

#### 5. 地址改变

request跳转时发生在服务器端，浏览器被瞒住了，所以url不变，response跳转时，浏览器会会自动向新的页面发送response，所以浏览器知道url改变了，所以url会改变