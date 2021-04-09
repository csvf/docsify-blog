# java8 的改变
## 一、Lambda表达式
lambda表达式提供的集合的操作方法 ：例如  分组，过滤，求和，最值，排序，去重。
### 1.分组
根据`groupingBy`指定分组字段   
//根据user对象的性别属性进行分组  
```java
//分组
Map<String,List<User>> groupBySex = userList.stream().collect(Collectors.groupingBy(User::getSex));
//遍历
for(Map.Entry(String,List<User>)  entryUser : groupBySex.entrySet()){
     String key = entryUser.getKey();
     List<User> entryUserList = entryUser.getValue(); 
}
```
### 2.过滤
通过fliter方法可以过滤某些条件   
//过滤掉公号201901的员工    
```java
List<User> userCommonList  = userList.stream().filter(a -> !Objects.equals(a.getJobNumber(),"201901").collect(Collectors.toList());
```
### 3.求和
分`基本类型`和`大数据类型`求和。   
* 基本数据类型先`mapToInt`，然后调用`sum()`方法。   
```java
int sumAge = userList.stream().mapToInt(User:getAge).sum();
```
* 大数据类型使用`reduce`调用`BigDecimal::add`方法。  
//`BigDecimal`求和  
```java
BigDecimal totalQuantity = userList.stream().map(User::getFamilyMemberQuantity).reduce(BigDecimal.ZERO,BigDecimal::add);
```
上面的求和不能过滤`BigDecimal`对象为`null`的情况，可能会报空指针，这种情况，我们可以用`filter`方法过滤，或者重写求和方法  
//重写 求和方法   
```java
package com.vvvtimes.util;
import java.math.BigDecimal;
public class BigDecimalUtils {
    public static BigDecimal ifNullSet0(BigDecimal in) {
        if (in != null) {
            return in;
        }
        return BigDecimal.ZERO;
    }
    public static BigDecimal sum(BigDecimal ...in){
        BigDecimal result = BigDecimal.ZERO;
        for (int i = 0; i < in.length; i++){
            result = result.add(ifNullSet0(in[i]));
        }
        return result;
    }
}
```
使用重写的方法:  
```java
//使用重写的方法
BigDecimal totalQuantity2 = userList.stream().map(User::getFamilyMemberQuantity).reduce(BigDecimal.ZERO, BigDecimalUtils::sum);
```
判断对象空  
`stream.filter(x -> x != null)`    
`stream.filter(Objects::nonNull)`   
判断字段空  
`stream.filter(x -> x.getDateTime()!=null)`   
### 4.求最
* 最大值`Max`    
`Date maxEntryDate = userList.stream().map(User::getEntryDate).max(Date::compareTo).get();`  
* 最小值`Min`  
`Date minEntryDate  = userList.stream().map(User::getEntryDate).min(Date::compareTo).get();`  
* 汇总求和  
`Collectors.summingInt`  
* 汇总求平均值   
`Collectors.averagingInt ` 
* 汇总所有信息包括数量、求和、平均值、最小值、最大值  
`Collectors.summarizingInt`
```java
Map<String, IntSummaryStatistics> collectGroupBySex = students.stream().collect(Collectors.groupingBy(student::getSex, Collectors.summarizingInt(student::getScore)));
System.out.println("男生："+collectGroupBySex.get("男"));
System.out.println("男生的分数和："+collectGroupBySex.get("男").getSum());
System.out.println("男生中最大分数："+collectGroupBySex.get("男").getMax());
System.out.println("男生中最小分数："+collectGroupBySex.get("男").getMin());
System.out.println("男生中平均分数："+collectGroupBySex.get("男").getAverage());
System.out.println("男生个数："+collectGroupBySex.get("男").getCount());
```
### 5. 最值对应的行记录对象  
```java
Comparator<LeasingBusinessContract> comparator = Comparator.comparing(LeasingBusinessContract::getLeaseEndDate);
LeasingBusinessContract maxObject = leasingBusinessContractList.stream().max(comparator).get();
```
### 6. List转Map
* `List`转`Map`   `toMap` 如果集合对象重复值`key` 则会报`duplicate key` 错误   
* 如果`user1 user2` 的`id` 都为`1` 用`(k1,k2) -> k1` 来设置，如果有重复`key`,则保留`key1`值，舍弃`k2`值  
`Map<Long,User> userMap = userList.stream().collect(Collectors.toMap(User::gitId,a -> a (k1,k2) ->k1));`    
* `list`转`map`的时候有时候会将`date`类型作为`key`，实际情况中使用`String`的多，我们可以将某个字段转成`String`  
`Map<String,workCenterLoadVo> workCenterMap  =list.stream().collect(Collectors.toMap(key)->DateFormatUtils.format(key.getDate(),'yyyy-MM-dd'),a->a,(k1,k2)->k1);`  
### 7. 排序
* 单字段 
`userList.sort(Comparator.comparing(User::getId));`  
* 多字段 
`userList.sort(Comparator.comparing(User::getId).thenComparing(User::getAge));`  
### 8. 去重
可通过`distinct`方法进行去重   
`List<Long> distinctIdList = idList.stream().distinct().collect(Collectors.toList());`  
### 9. 生成新List
获取list某个字段组装新list  
`List<Long> userIdList =userList.stream().map(a->a.getId()).collect(Collectors.toList());`  
### 10. 不同实体类list 拷贝
`List<TimePeriodDate> timePeriodDateList1 = calendarModelVoList.stream().map(p->{TimePeriodDate e = new TimePeriodDate(); 
e.setStartDate(p.getBegin();e.setEndDate(p.getEnd());return e;)}).collect(Collectors.toList());`  
### 11. 交集、差集、并集、去重并集 
两个集合之间关系  
```java
List<String> list1 = new ArrayList();
list1.add("1111");
list1.add("2222");
list1.add("3333");
List<String> list2 = new ArrayList();
list2.add("3333");
list2.add("4444");
list2.add("5555");
```
* 交集  
拓展：list2里面如果是对象，则需要提取每个对象的某一属性组成新的list,多个条件则为多个list  
` List<String> intersection = list1.stream().filter(item -> list2.contains(item)).collect(Collectors.toList());`  
` System.out.println("---得到交集 intersection---");`  
` intersection.parallelStream().forEach(System.out :: println);`  
* 差集(list1 - list2)   List1部分  
同上拓展  
`List<String> reduce1 = list1.stream().filter(item -> !list2.contains(item)).collect(Collectors.toList());`  
`System.out.println("---得到差集 reduce1 (list1 - list2)---");`   
`reduce1.parallelStream().forEach(System.out :: println);`  
* 差集 (list2 - list1)  List2部分  
`List<String> reduce2 = list2.stream().filter(item -> !list1.contains(item)).collect(Collectors.toList());`  
`System.out.println("---得到差集 reduce2 (list2 - list1)---");`  
`reduce2.parallelStream().forEach(System.out :: println);`  
* 并集  
`List<String> listAll = list1.parallelStream().collect(toList());`  
`List<String> listAll2 = list2.parallelStream().collect(Collectors.toList());`  
`listAll.addAll(listAll2);`  
`System.out.println("---得到并集 listAll---");`  
`listAll.parallelStream().forEach(System.out :: println);`   
* 去重并集  
`List<String> listAllDistinct = listAll.stream().distinct().collect(Collectors.toList());`   
`System.out.println("---得到去重并集 listAllDistinct---");`   
`listAllDistinct.parallelStream().forEach(System.out :: println);`   
* 集合遍历   
`System.out.println("---原来的List1---");`    
`list1.parallelStream().forEach(System.out :: println);`    
`System.out.println("---原来的List2---");`    
`list2.parallelStream().forEach(System.out :: println);`  
 一般有`filter` 操作时，不用并行流`parallelStream` ,如果用的话可能会导致`线程安全`问题。  
### 12.Lambda使用限制  
`Lambda`可以没有限制地捕获（也就是在其主体中引用）实例变量和静态变量。但局部变量必须显式声明为`final`， 或事实上是`final`。  
换句话说，`Lambda`表达式只能捕获指派给它们的局部变量一次。（注：捕获 实例变量可以被看作捕获最终局部变量`this`。）  
例如，下面的代码无法编译，因为`portNumber` 变量被赋值两次：
```java
int portNumber = 1337;
Runnable r = () -> System.out.println(portNumber); 
portNumber = 31337; //错误：Lambda表达式引用的局 部变量必须是最终的（final） 或事实上最终的
```
### 13. 为什么局部变量有这些限制  
* 实例变量和局部变量背后的实现有一 个关键不同。实例变量都存储在堆中，而局部变量则保存在栈上。  
如果Lambda可以直接访问局部变量，而且Lambda是在一个线程中使用的，则使用Lambda的线程，可能会在分配该变量的线程将这个变量收回之后，去访问该变量。   
因此，Java在访问自由局部变量时，实际上是在访问它的副本，而不是访问原始变量。如果局部变量仅仅赋值一次那就没有什么区别了——因此就有了这个限制。  
* 这一限制不鼓励你使用改变外部变量的典型命令式编程模式（这种模式会阻碍很容易做到的并行处理）。  
### 14. Lambda表达式方法引用
* 你需要使用方法引用时， **目标引用放在分隔符 :: 前， 方法的名称放在后面。**  
例如， `Apple::getWeight`就是引用了`Apple`类中定义的方法`getWeight`。请记住，不需要括号，因为 你没有实际调用这个方法  

Lambda | 等效的引用方法 
:------------ | ------------- 
`(Apple a) -> a.getWeight()` | `Apple::getWeight`
`() -> Thread.currentThread().dumpStack()` |  ` Thread.currentThread()::dumpStack`
`(str,i) -> str.substring(i)` |  `String::substring`
`(String i) -> System.out.println(s)` |  `System.out::println`

### 15. 方法引用  
* 指向静态方法的引用（例如 `Integer` 的 `parseInt` 方法，写作 `Integer::parseInt`）  
* 指向任意类型实例方法的方法引用（例如 `String` 的 `length` 方法，写作 `String::length`） 
* 指向现有对象的实例方法的引用(假设有一个局部变量 `expensiveTransaction` 用于存放 `Transaction` 类型的对象，它支持实例方法 `getValue`，那么就可以写 `expensiveTransaction::getValue`)  
* 构造方法的引用  
   对于一个现有构造函数 可以利用它的名称和关键字 `new` 来创建它的一个引用：`ClassName::new`。它的功能与指向静态方法的引用类似   
```java
Supplier<Apple> c1 = Apple::new; //构造函数引用指向默认的 Apple() 构造函数
Apple a1 = c1.get(); //产生一个新的对象
//等价于：
Supplier<Apple> c1 = () -> new Apple(); //利用默认构造函数创建 Apple 的 Lambda 表达式
Apple a1 = c1.get();
```
  如果构造函数的签名是`Apple(Integer weight)`，那么它就适合 `Function` 接口的签名，于是可以这样写  
```java
Function<Integer, Apple> c2 = Apple::new; //构造函数引用指向 Apple(Integer weight) 构造函数
Apple a2 = c2.apple(100);
//等价于：
Function<Integer, Apple> c2 = (Integer weight) -> new Apple(weight);
Apple a2 = c2.apple(100);
```
  如果一个具有两个参数的构造函数Apple(String color, Integer weight)，那么它就适合BiFunction接口的签名，于是可以这样写： 
```java 
BiFunction<Integer, Integer, Apple> c3 = Apple::new;
Apple a3 = c23.apple("green", 100);
//等价于：
BiFunction<Integer, Apple> c3 = (String color, Integer weight) -> new Apple(color, weight);
Apple a3 = c3.apple("green", 100);
```
### 16.  List 可用的 sort 方法  
Java 8的API已经为你提供了一个 `List` 可用的 `sort` 方法 `sort` 的行为被参数化了，自己定义排序参数如下:   
```java 
apples.sort(new Comparator<Apple>() {
  @Override
  public int compare(Apple o1, Apple o2) {
    return o1.getWeight().compareTo(o2.getWeight());
  }
});
```

## 二、函数式接口
`Predicate<T>   Boolean   IntPredicate  LongPredicate DoublePredicate`  
`Consumer<T>    void      IntConsumer LongConsumer DoubleConsumer`  
`Function<T,R>   T ->R    IntFunction<R>`   
`Supplier<T>     ()->T     BooleanSupplier IntSupplier LongSupplier DoubleSupplier`  


