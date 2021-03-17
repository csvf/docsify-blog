
# java基础 
注意:此文基于jdk版本1.8
## 八种基础数据类型
- 原始类型 (拆箱)   

|说明|数据类型|默认值
:----|:----|:----
|布尔型|boolean|false
|字符型|char|'u0000'
|1/4 int整数型|byte|0
|1/2 int整数型|short|0
|默认整数型|int|0
|大整数型|long|0L
|单精度浮点数型|float|0.0f
|双精度浮点数型|double|0.0d

- 基本类型的变量 都是“持有”某个数值    
## 引用类型  
- 封装类型 (装箱)  

|说明|数据类型|默认值
:----|:----|:----
|布尔型|Boolean|null
|字符型|Character|null
|1/4 int整数型|Byte|null
|1/2 int整数型|Short|null
|默认整数型|Integer|null
|大整数型|Long|null
|单精度浮点数型|Float|null
|双精度浮点数型|Double|null

- 引用类型的变量  都是 “指向” 某个对象 而非 “持有” 某个对象。
- String 、 数组 、 集合 、 实例化的类变量都属于引用类型的变量。  
- 引用类型的变量 指向null 和“”空字符串的区别。 
- 浮点型相当运算: Math.abs(0.6)-0.6 <0.00001 即可认为是true  
## 字符串String 
String 类是被`final`修饰的类，不允许再被继承。  
String类已经重写了 `equals` 和 `hashcode`方法  字符串比较是值的比较。
定义一个字符串:
```java
String str = new String("abc"); //定义了一个对象 在jvm堆内存中
String str1 = "abc";  //定义了一个常量字符串，在jvm堆内存的常量池中
String str2 = str;
String str3 = "ab";
String str4 = str3+"c";
String str5 = "ab"+"c";
System.out.println(str == str1); 
System.out.println(str.equals(str1)); 
System.out.println(str == str2);  
System.out.println(str.equals(str2)); 
System.out.println(str1 == str4);  
System.out.println(str1.equals(str4)); 
System.out.println(str1 == str5);  
System.out.println(str1.equals(str5)); 
``` 
输出结果:  
* `false` //== 比较两边引用指向的对象是否相同，一个指向在堆内存常量池区域的常量，  
    一个指向在堆内存的new产生的对象 ，显然为false  
* `true`  //equals比较的是两个对象的值是否相同 都是abc固为true   
* `true`  //== 比较两边引用指向的对象是否相同，两个引用指向同一个对象固为true
* `true`  //equals比较的是两个对象的值是否相同 都是abc固为true 
* `false` //变量+字符串常量，是拼接再把拼接对象转化为字符串，所以两边指向的是不同的对象。
* `true`  //equals比较的是两个对象的值是否相同 都是abc固为true 
* `true`  //常量字符串+常量字符串，得到拼接后的常量字符串，  
   在常量池中已经存该常量所以不再新创建，而是复用所以为true
* `true`  //equals比较的是两个对象的值是否相同 都是abc固为true 
>变量的引用  存放在jvm的栈内存中
- StringBuffer 线程安全  
- StringBuilder 线程不安全的。  
**常量**  
常量在程序运行时是不能被修改的，在 Java 中使用 final 关键字来修饰常量，声明方式和变量类似:   
`final Integer WORK_TIME = 996;`
## java虚拟机jvm内存模型
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210317154743.png)   
绿色表示线程独享区域
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/d44aa0a1f24f962e9486d4c3c7e4b47c_format,png.png)   
-  程序计数器
每条线程都需要有一个独立的**程序计数器**，各条线程之间计数器互不影响，独立存储，我们称这类内存区域为“线程私有”的内存。  
如果线程正在执行的是一个 Java 方法，这个计数器记录的是正在执行的虚拟机字节码指令的地址；   
如果正在执行的是 Native 方法，这个计数器值则为空（Undefined）。    
此内存区域是唯一一个在 Java 虚拟机规范中没有规定任何 OutOfMemoryError 情况的区域。     
-  Java虚拟机栈
与程序计数器一样，Java 虚拟机栈（Java Virtual Machine Stacks）也是线程私有的，它的生命周期与线程相同。  
虚拟机栈描述的是 `Java 方法`执行的内存模型  
每个方法在执行的同时都会创建一个栈帧（Stack Frame，是方法运行时的基础数据结构）用于存储`局部变量表`、`操作数栈`、`动态链接`、`方法出口`等信息。  
每一个方法从调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中入栈到出栈的过程。  
虚拟机栈规定了两种异常状况：   
如果线程请求的栈深度大于虚拟机所允许的深度，将抛出 StackOverflowError 异常；  
如果虚拟机栈可以动态扩展（当前大部分的 Java 虚拟机都可动态扩展），如果扩展时无法申请到足够的内存，就会抛出 OutOfMemoryError 异常。 
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/309a94668f9db473571baf91d244f159_format,png.png)    
- 本地方法栈 
本地方法栈（Native Method Stack）与虚拟机栈所发挥的作用是非常相似的，它们之间的区别不过是虚拟机栈为虚拟机执行 Java 方法（也就是字节码）服务，  
而本地方法栈则为虚拟机使用到的 Native 方法服务。Sun HotSpot 虚拟机直接就把本地方法栈和虚拟机栈合二为一。  
与虚拟机栈一样，本地方法栈区域也会抛出 StackOverflowError 和 OutOfMemoryError 异常。
JNI 类本地方法最著名的应该是 `System.currentTimeMillis() `，  JNI使 Java 深度使用操作系统的特性功能，复用非 Java 代码。  
但是在项目过程中， 如果大量使用其他语言来实现 JNI , 就会丧失跨平台特性。  
- Java堆 
对于大多数应用来说，Java 堆（Java Heap）是 Java 虚拟机所管理的内存中最大的一块。  
Java 堆是被所有线程共享的一块内存区域，在虚拟机启动时创建。此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例都在这里分配内存。  

堆是垃圾收集器管理的主要区域，因此很多时候也被称做“GC堆”（Garbage Collected Heap）。  
从内存回收的角度来看，由于现在收集器基本都采用分代收集算法，所以 Java 堆中还可以细分为：新生代和老年代；   
再细致一点的有 Eden 空间、From Survivor 空间、To Survivor 空间等。   
从内存分配的角度来看，线程共享的 Java 堆中可能划分出多个线程私有的分配缓冲区（Thread Local Allocation Buffer,TLAB）。  

Java 堆可以处于物理上不连续的内存空间中，只要逻辑上是连续的即可，   
当前主流的虚拟机都是按照可扩展来实现的（通过 -Xmx 和 -Xms 控制）。   
如果在堆中没有内存完成实例分配，并且堆也无法再扩展时，将会抛出 OutOfMemoryError 异常。  

为了支持垃圾收集，堆被分为三个部分：  
年轻代 ： 常常又被划分为Eden区和Survivor（From Survivor To Survivor）区    
(Eden空间、From Survivor空间、To Survivor空间（空间分配比例是8：1：1）    
老年代  
永久代 （jdk 8已移除永久代，如下图）  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/_20210317162255.png)  
所有新创建的Object 都将会存储在新生代Yong Generation中。  
如果Young Generation的数据在一次或多次GC后存活下来，那么将被转移到OldGeneration。新的Object总是创建在Eden Space。  

- 方法区 

方法区（Method Area）与 Java 堆一样，是各个线程共享的内存区域，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。  
虽然Java 虚拟机规范把方法区描述为堆的一个逻辑部分，但是它却有一个别名叫做 Non-Heap（非堆），目的应该是与 Java 堆区分开来。  


Java 虚拟机规范对方法区的限制非常宽松，除了和 Java 堆一样不需要连续的内存和可以选择固定大小或者可扩展外，还可以选择不实现垃圾收集。  
垃圾收集行为在这个区域是比较少出现的，其内存回收目标主要是针对常量池的回收和对类型的卸载。  
当方法区无法满足内存分配需求时，将抛出 OutOfMemoryError 异常。  

**JDK8 之前，Hotspot 中方法区的实现是永久代（Perm），JDK8 开始使用元空间（Metaspace），  
以前永久代所有内容的字符串常量移至堆内存，其他内容移至元空间，元空间直接在本地内存分配。**

为什么要使用元空间取代永久代的实现？  
字符串存在永久代中，容易出现性能问题和内存溢出。  
类及方法的信息等比较难确定其大小，因此对于永久代的大小指定比较困难，太小容易出现永久代溢出，太大则容易导致老年代溢出。  
永久代会为 GC 带来不必要的复杂度，并且回收效率偏低。  
将 HotSpot 与 JRockit 合二为一。  
- 直接内存
直接内存（Direct Memory）并不是虚拟机运行时数据区的一部分，也不是 Java 虚拟机规范中定义的内存区域。  
本机直接内存的分配不会受到 Java 堆大小的限制，但是，既然是内存，肯定还是会受到本机总内存（包括 RAM 以及 SWAP 区或者分页文件）大小以及处理器寻址空间的限制。  
服务器管理员在配置虚拟机参数时，会根据实际内存设置 -Xmx 等参数信息，但经常忽略直接内存，使得各个内存区域总和大于物理内存限制（包括物理的和操作系统级的限制），  
从而导致动态扩展时出现 OutOfMemoryError 异常。  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/20210317161429.png)   

- volatile关键字 
volatile 可以说是 JVM 提供的最轻量级的同步机制，当一个变量定义为volatile之后，它将具备两种特性：  
    - 保证此变量对所有线程的可见性。而普通变量不能做到这一点，普通变量的值在线程间传递均需要通过主内存来完成。  
注意，volatile 虽然保证了可见性，但是 Java 里面的运算并非原子操作，导致 volatile 变量的运算在并发下一样是不安全的。  
而 synchronized 关键字则是由“一个变量在同一个时刻只允许一条线程对其进行 lock 操作”这条规则获得线程安全的。  
    - 禁止指令重排序优化。  普通的变量仅仅会保证在该方法的执行过程中所有依赖赋值结果的地方都能获取到正确的结果，  
而不能保证变量赋值操作的顺序与程序代码中的执行顺序一致。  

## 数组Array
```java
数组定义:
一维 String[]  st = new String[4];
二维 String[][] erst = new String[4][5];
多维 String[][][] thst =new String[3][4][5];
```
`Arrays.toString(st)` //快速打印数组的内容  
`Arrays.deeptoString(erst)` //快速打印二维数组的内容  
`Arrays.sort(st) ` //数组从小到大排序  
`for each 循环  for(String s : st ){} `  //单纯循环数组内的元素而不需要索引时，采用此方式更快。  
## 抽象类、接口
* 含有抽象方法的类 就是抽象类。     
* 抽象类中可以含有普通的方法。      
* 抽象方法只定义方法，没有具体实现。    
* 抽象类中可以定义变量，接口中不允许定义变量。  
* 接口中定义的变量都是 静态常量  static final int =10 必须有初始值    
* 接口中可以定义一个默认方法  用default修饰 ，接口中定义的该方法，在实现接口的类中不需要强制重写    
* 静态方法 无法访问 this 及实例字段，静态方法，静态字段 都是通过类名.的方式访问，不需要实例化  

## 反射reflect、内省Introspector
- Introspector（内省）方式 获取JavaBean的所有的属性、 类型 和gett/sett方法    
`BeanInfo bi  = Introspector.getBeanInfo(Person.class) ` 需要用条件判断 排除 获取的class方法   
再通过reflect（反射）方式 使用反射方式来调用获取的方法，达到动态扩展的目的  
## 枚举Enum
- 通过 enum  枚举 来定义常量。    
通过name 方法来获取 枚举定义的字符串， 不要用 .toString() 容易被重写。   
enum类的构造方法必须为 private  
## 异常Exception
catch 捕获异常 子类异常必须放在上面。    
可以 同时捕获 多个没有继承关系的异常， 多个异常用 | 隔开   (需要jdk 在1.7 以上)  
## 日志模块log4j
commons logging 日志模块 可以代替`system.out.println("输出语句"); `      
commons logging 可以定义日志的输出级别和 设置日志的格式等，比较常用。    
commons logging 如果发现有log4j 则自动使用log4j   
引入方式如下：
```java
static log = logFactory.getLog(类名.class) 
```
在配置文件log4j2.xml中配置log4j的输出格式和方式即可。  

## 类Class 实例InstanceOf
获取一个类的Class 实例的三种方式
```java
方式一:
Class clazz1 = String.class
方式二:
String st="ss";
Class  clazz2 = st.getClass();
方式三:
Class  clazz3 = Class.forName("java.lang.String"); 
```
JVM 内部为每个类创建的Class实例 都是唯一， 所以以上三种方式获取的Class都是指向同一个对象  
所以  `clazz1 = =clazz2  clazz2 = =clazz3`  判断都为真。  

>Class实例 和 instanceOf的区别?   
Class用来精确判断一个类， instanceOf 来判断实例  可能是实例本身类 ，也可能是其父类。

## 重写 equals hashcode方法 
!> 一个类复写了 equals 方法 则必须复写 hashcode方法。 
- equals比较 方法  可以借助  `Objects.equlas(a,b)`方法来判断 防止空指针。   // JDK1.7以上引入的。
```java
重写equals()方法的 例子： 
public boolean equals(Object o ){
  if(this= =o) return true;
    if(o intanceof Person){
    Person p = (Person)o;
    return  Objects.equals(this.name,o.name) && this.age==p.age;
  }
  return false;
}
```
- 重写 hashcode 可以借助 Objects.hash();
```java
重写 hashcode例子
public int hashcode(){
  return Objects.hash(this.name,this.age);
}
```
## 集合
###HashMap TreeMap
`for each`  循环 `keySet();`
`for each`  循环 `entrySet();`
treeMap  map 按照value进行排序， 如果需要倒叙 则传入 一个 comparable 接口 实现类 重写 compare 方法接口 。  
```java
TreeMap排序
Map  map = new TreeMap(new Comparator<Person>(){
   public  int compare(Person p1,Person p2){
  		return   p1.name.compareTo(p2.name); //按照 name 的 字母顺序   从小到大  正序排序  
        //return  -p1.name.compareTo(p2.name); //按照 name 的 字母顺序   从大到小  倒序排序
  }   
})
```
用对象作为key的 必须复写 equals 和 hashcode 方法，具体demo参考上一节。  

for循环三种方式 
`for i` 、 `增强for` 、 `for each`


## 队列Queue 
* 队列 先进先出 FIFO linkedList      
添加到队列尾部  `add/ offer`   
从队首获取元素并且删除   `remove/ poll`    
从队首获取元素但是不删除  `element /peek`     
避免把 null 添加到队列    
* PriorityQueue 实现一个优先队列，从队首获取元素时 获取优先级最高的元素，  
PriorityQueue 中元素来实现Comparable接口  或者 PriorityQueue 实现 Comparator接口    
* deque 是 queue 的扩展类， 是双端队列，可以向队首 或者 队尾添加元素  
调用 Last  和 First  方法   
##  栈stack  
栈 后进先出 LIFO  
* deque  
    push 压栈   
    pop  出栈  
peek 取出顶元素但是元素不出栈    
##  Collections 与  Colleciton的区别  
Collections 工具类 ----> Colleciton是接口  
## 输入输出流I/O 
站在 内存的角度去理解输入和输出  
读写接口
```java
     inputStream / outputStream    处理字节流   最小单位是byte   读取文本 或者 音频视频 word等。
     Reader      /  Writer         处理字符流   最小单位是Char   只能读取文本 txt 等，效率较高。
```
`File` 既可以是文件也可以是目录     
`File.isFile()` 是不是文件    
`File.isDirectory();` 是不是目录    
实例化一个File对象 不会对磁盘产生任何操作，只有当使用实例化对象的方法时才会与磁盘产生交互，如果不存在 抛出异常。  
`getPath()`  获取路径   
`getAbsolutePath()` 绝对路径    
`getCanonicalPath()` 规范路径   
`inputStream` `FileInputStream`  jdk1.7 新特性 支持try后跟() 用括号来管理释放资源。  
```java
例如:
try( 
	InputStream ins = new FileInputStream(source);
	OutputStream outs = new FileOutputStream(target);	
    ){
	    byte[] buf = new byte[1024];
        int i;
        while((i=ins.read(buf))!=-1){
	      outs.write(buf,0,i);
		}
  	  }
```
Filter 模式  `FilterInputStream` 可以提供各种输入输出流程的扩展功能   
`InputStream ins =  new  GzipInputStream(new BufferInputStrea(new FileInputStream(source)))`   
从工程根目录读取文件 `当前类的类名.class.getReasourceAsStream("文件名");`  没有/    
* 从classpath  （src文件下）路径读取文件：  
1、 以/ 开头  
2、 InputStream ins = 当前类的类名.class.getReasourceAsStream("/路径");   
3、 要判断文件是否存在，如不存在则 ins=null;    
读取properties文件 代码如下：     
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/_20210315212218.png)
读取txt文件 代码如下：  
![](https://cdn.jsdelivr.net/gh/csvf/imagehost/imgs/_20210315212235.png)
`Reader read =new FileReader("/文本路径");  `     

FileReader默认使用的是 系统编码 如果需要更改去读时的编码， 则 需要 使用InputStreamReader 传入一个 FileInputStream 和编码   

`Reader reader = new InputStreamReader(new FileInputStream("路径"),"uft-8");`   

`try(resource)保证Reader能正确关闭，try(resource){ //代码块}` 是 jdk1.7引入的。     
## 时间类Date localDate
Java的时间表示：  
Java.util.Date  (旧)   
可以获取年月日时分秒  
Java.sql.Time  继承自 util.date 但是去掉了日期的实现，所以 实际上是只能获取时间。  
Java.sql.timestamp 是用int 来表示 毫秒数

说明 | 数据库类型 | java旧的时间处理类 | 新的时间处理类
:------------ | ------------- | ------------- | -------------
日期+时间 | DATATIME | java.util.date | LocalDateTime
日期 | DATE | java.util.date | LocalDate
时间 | TIME | java.sql.Time | LocalTime
日期和时间 _ long类型毫秒数 | TIMESTAMP | java.sql.TimeStamp | LocalDateTime

java.time.Date (新)   ----jdk8引入。  
LocalDate 日期  
获取当前日期 `LocalDate localDate = LocalDate.now();`  输出结果 `2020-03-28`  
LocalTime 时间   
获取当前时间`LocalTime localTime = LocalTime.now();`  输出结果 `16:58:20.799`    
LocalDateTime 日期+时间   
获取当前时间+时间 ` LocalDateTime dateTime = LocalDateTime.now();` 输出结果 `2020-03-28T16:58:20.799`  
自定义格式化输出:  
```java 
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");  
String dateStr = formatter2.format(dateTime);
System.out.println(dateStr);
//打印输出:  2020-03-28 16:58:20
```
ZoneDateTime 时区   
LocalDate 可以使用 `plusDays minusHours()`等方法 对日期和时间进行加减 等操作  

## 单元测试Junit  
@Test  
使用assert断言：    
      断言相等 `assertEquals(100,x)`  100表示期望的结果， x表示测试的方法及传入的参数  
      断言数组相等：`assertArrayEquals({1,2,3},x)`  
      断言浮点数相等： `assertEquals(3.1414,x,0.0001)`  //有精度损失，所以增加一个0.0001的误差值  
      断言为true或者false  `assertTrue(x>0)`  `assertFalse(x<0)`   
      断言为Null    `assertNull(x)`    
      
 @Before  @After  注解可以在执行@test测试方法之前 初始化资源方法， 在测试之后释放资源方法    
（基于实例层面 ，变量定义成成员变量 ，每次单元测试都是执行不同的实例）  

 @BeforeClass  @AfterClass 注解可以在执行@test测试方法之前 初始化资源方法， 在测试之后释放资源方法    
（基于类层面 ，变量定义成静态成员变量 ，多次单元测试请求的都是一个）  
## 正则表达式
`String st ="abc";`   
`st.mathes("\\w+");`    
正则表达式的开始位置为`^`  结束位置为 `$`  java中可以省略。  

`.`匹配任意一个字符  
`\w` 匹配 字符 _  数字  
`\s` 空格或者tab键  
`\d` 数字  
`\D` 匹配非数字  大写的都是与小写的相反  

`*` 匹配任意个字符   
`+` 匹配至少一个字符   
`？`匹配0或者1个字符  
`{n}` 匹配n个  
`{n,m}`匹配n到m个字符  
`{n,}`匹配至少n个字符  
 
`^` 匹配字符串开头  
`$` 匹配字符串结尾  

`[abc]`  中括号内的任意一个字符   
`[a-f0-8]` a到f或者0到8 任意一个字符    
`[^0-9]` 匹配不在0-9范围内的任意字符   
`abc|java|ss`  匹配|分割的其中的任意一个  

利用正则表达式提取字符串的字串  
```java
Pattern p =  Pattern.compile("规则")
Matcher matcher = p.matcher("字符串")
if(matcher.matches()){
    String whole= matcher.group(0); //提取整个字符串
     String f= matcher.group(1); //提取匹配到的第一个字符串
}
```
默认为贪婪匹配 ，即尽可能的多匹配   
例如 用group分组匹配  1230000   `"^(\d+)(0*)$"`  匹配到是  `"1230000"`   ""  而想要的结果是 `"123"`  `"0000"`   
则正则表达式改为  `"^(\d+?)(0*)$"`  即可改为非贪婪模式 匹配到 `"123"`  `"0000"` 利用 分组匹配即可分别获取到。  
`split("正则表达式")` 对字符串进行分割   利用正则表达式 以  `空格` `，` `;` 等信息进行字符串分割。  
`find` 用正则的方式搜索字符串。  搜索到想要的字符  
`replaceAll("正则表达式","") `方式来进行替换字串。  例如：以空格分割的一串文字都 加上`<b>单字</b>` 加粗  

  












