
# java基础 
注意:此文基于jdk版本1.8
## 八种基础数据类型
- 原始类型 (拆箱)   
```
布尔型:          boolean
字符型:          char
整数型:          byte, short, int, long
浮点数型:        float, double
```
- 基本类型的变量 都是“持有”某个数值    
## 引用类型  
- 封装类型 (装箱)  
```
布尔型:          Boolean
字符型:          Character
整数型:          Byte, Short, Integer, Long
浮点数型:        Float, Double
```
- 引用类型的变量  都是 “指向” 某个对象 而非 “持有” 某个对象。
- String 、 数组 、 集合 、 实例化的类变量都属于引用类型的变量。  
- 引用类型的变量 指向null 和“”空字符串的区别。 
- 浮点型相当运算: Math.abs(0.6)-0.6 <0.00001 即可认为是true    
## 数组  
```
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
## String 
- StringBuffer 线程安全  
- StringBuilder 线程不安全的。
## Introspector内省、reflect 反射
- Introspector（内省）方式 获取JavaBean的所有的属性、 类型 和gett/sett方法    
`BeanInfo bi  = Introspector.getBeanInfo(Person.class) ` 需要用条件判断 排除 获取的class方法   
再通过reflect（反射）方式 使用反射方式来调用获取的方法，达到动态扩展的目的  
## 枚举Enum
- 通过 enum  枚举 来定义常量。    
通过name 方法来获取 枚举定义的字符串， 不要用 .toString() 容易被重写。   
enum类的构造方法必须为 private  
## Exception 异常
catch 捕获异常 子类异常必须放在上面。    
可以 同时捕获 多个没有继承关系的异常， 多个异常用 | 隔开   (需要jdk 在1.7 以上)  
## 日志模块log4j
commons logging 日志模块 可以代替`system.out.println("输出语句"); `      
commons logging 可以定义日志的输出级别和 设置日志的格式等，比较常用。    
commons logging 如果发现有log4j 则自动使用log4j   
引入方式如下：
```
static log = logFactory.getLog(类名.class) 
```
在配置文件log4j2.xml中配置log4j的输出格式和方式即可。  

## Class实例 、 InstanceOf
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
## HashMap TreeMap
`for each`  循环 `keySet();`
`for each`  循环 `entrySet();`
treeMap  map 按照value进行排序， 如果需要倒叙 则传入 一个 comparable 接口 实现类 重写 compare 方法接口 。  
```jsp
TreeMap排序
Map  map = new TreeMap(new Comparator<Person>(){
   public  int compare(Person p1,Person p2){
  		return   p1.name.compareTo(p2.name); //按照 name 的 字母顺序   从小到大  正序排序  
        //return  -p1.name.compareTo(p2.name); //按照 name 的 字母顺序   从大到小  倒序排序
  }   
})
```
用对象作为key的 必须复写 equals 和 hashcode 方法，具体demo参考上一节。  
## Queue 队列
* 队列 先进先出 FIFO linkedList      
添加到队列尾部  `add/ offer`   
从队首获取元素并且删除   `remove/ poll`    
从队首获取元素但是不删除  `element /peek`     
避免把 null 添加到队列    
* PriorityQueue 实现一个优先队列，从队首获取元素时 获取优先级最高的元素，  
PriorityQueue 中元素来实现Comparable接口  或者 PriorityQueue 实现 Comparator接口    
* deque 是 queue 的扩展类， 是双端队列，可以向队首 或者 队尾添加元素  
调用 Last  和 First  方法   
##  stack 栈   
栈 后进先出 LIFO  
* deque  
push 压栈   
pop  出栈  
peek 取出顶元素但是元素不出栈    
##  Collections 与  Colleciton的区别  
Collections 工具类 ----> Colleciton是接口  
## I/O 输入输出流
站在 内存的角度去理解输入和输出  
读写接口
```jsp
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
```jsp
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

读取txt文件 代码如下：  

`Reader read =new FileReader("/文本路径");  `     

FileReader默认使用的是 系统编码 如果需要更改去读时的编码， 则 需要 使用InputStreamReader 传入一个 FileInputStream 和编码   

`Reader reader = new InputStreamReader(new FileInputStream("路径"),"uft-8");`   

`try(resource)保证Reader能正确关闭，try(resource){ //代码块}` 是 jdk1.7引入的。     
## 时间类Date 
Java的时间表示：  
Java.util.Date  (旧)   
可以获取年月日时分秒  
Java.sql.Time  继承自 util.date 但是去掉了日期的实现，所以 实际上是只能获取时间。  
Java.sql.timestamp 是用int 来表示 毫秒数   










