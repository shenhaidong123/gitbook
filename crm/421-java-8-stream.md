# Java8 Stream

#### 作者：杨丽

#### 邮箱：yangli@haomo-studio.com

## Stream 简介

#### Stream 是Java 8 的一大亮点，是对集合（Collection）对象功能的增强，对集合、大批量数据操作操作非常便利、高效。借助于同样新出现的 Lambda 表达式，极大的提高编程效率和程序可读性

#### 优点：代码更加简洁易读；而且使用并发模式，程序执行速度更快

Stream 的另外一大特点是，数据源本身可以是无限的

#### Java8 Lambda 表达式

由于Stream 很多地方用到了 Lambda 表达式，所以先了解一下 Lambda 表达式

Lambda表达式本质上是一个匿名方法。让我们来看下面这个例子：

```
public int add(int x, int y) {
    return x + y;
}
```

转成Lambda表达式后是这个样子：

```
(int x, int y) -> x + y;
```

参数类型也可以省略，Java编译器会根据上下文推断出来：

```
(x, y) -> x + y; //返回两数之和
```

或者

```
(x, y) -> { return x + y; } //显式指明返回值
```

可见Lambda表达式有三部分组成：参数列表，箭头（-&gt;），以及一个表达式或语句块。  
下面这个例子里的λ表达式没有参数，也没有返回值（相当于一个方法接受0个参数，返回void，其实就是Runnable里run方法的一个实现）：

```
() -> { System.out.println("Hello Lambda!"); }
```

如果只有一个参数且可以被Java推断出类型，那么参数列表的括号也可以省略：

```
c -> { return c.size(); }
```

方法引用

```
Integer::parseInt //静态方法引用
System.out::print //实例方法引用
Person::new       //构造器引用
```

下面是一组例子，教你使用方法引用代替Lambda表达式：

```
//c1 与 c2 是一样的（静态方法引用）
Comparator<Integer> c2 = (x, y) -> Integer.compare(x, y);
Comparator<Integer> c1 = Integer::compare;

//下面两句是一样的（实例方法引用1）
persons.forEach(e -> System.out.println(e));
persons.forEach(System.out::println);

//下面两句是一样的（实例方法引用2）
persons.forEach(person -> person.eat());
persons.forEach(Person::eat);

//下面两句是一样的（构造器引用）
strList.stream().map(s -> new Integer(s));
strList.stream().map(Integer::new);
```

### Stream的使用

包括三个基本步骤：  
获取一个数据源（source）→ 数据转换→执行操作获取想要的结果，每次转换原有 Stream 对象不改变，返回一个新的 Stream 对象（可以有多次转换），这就允许对其操作可以像链条一样排列，变成一个管道

### 流的操作类型分为两种：（可以理解为中间操作和结束操作）

##### Intermediate：一个流可以后面跟随零个或多个 intermediate 操作。其目的主要是打开流，做出某种程度的数据映射/过滤，然后返回一个新的流，交给下一个操作使用。这类操作都是惰性化的（lazy），就是说，仅仅调用到这类方法，并没有真正开始流的遍历。

##### Terminal：一个流只能有一个 terminal 操作，当这个操作执行后，流就被使用“光”了，无法再被操作。所以这必定是流的最后一个操作。Terminal 操作的执行，才会真正开始流的遍历，并且会生成一个结果

#### 示例：

int sum = widgets.stream\(\) .filter\(w -&gt; w.getColor\(\) == RED\)  .mapToInt\(w -&gt; w.getWeight\(\)\)  .sum\(\);

##### stream\(\) 获取widgets source，filter 和 mapToInt 为 intermediate 操作，进行数据筛选和转换，最后一个 sum\(\) 为 terminal 操作，对符合条件的widget作重量求和.

### 构造流的几种常见方法

1.Stream stream = Stream.of\("a", "b", "c"\);  
2.strArray = new String\[\] {"a", "b", "c"}; stream = Stream.of\(strArray\); stream = Arrays.stream\(strArray\);   
3.List`<String>` list = Arrays.asList\(strArray\); stream = list.stream\(\);

### 流的操作

##### 当把一个数据结构包装成 Stream 后，就要开始对里面的元素进行各类操作了。常见的操作可以归类如下:

##### Intermediate：

map \(mapToInt, flatMap 等\)、 filter、 distinct、 sorted、 peek、 limit、 skip

##### Terminal：

forEach、 forEachOrdered、 toArray、 reduce、 collect、 min、 max、 count、   
anyMatch、 allMatch、 noneMatch、 findFirst、 findAny、 iterator

##### Map, mapToInt

##### map方法将流中的元素映射成另外的值，新的值类型可以和原来的元素的类型不同。

##### mapToInt 方法将流中的元素映射成int类型的值

##### 下面的代码中将字符元素映射成它的哈希码\(ASCII值\)。

List`<Integer>` l = Stream.of\('a','b','c'\).map\( c -&gt; c.hashCode\(\)\).collect\(Collectors.toList\(\)\);System.out.println\(l\); //\[97, 98, 99\]

##### filter

##### filter返回的流中只包含满足断言\(predicate\)的数据。

List`<Integer>` l = IntStream.range\(1,10\).filter\( i -&gt; i % 2 == 0\).collect\(Collectors.toList\(\)\);System.out.println\(l\); //\[2, 4, 6, 8\]

##### sorted  - 排序

List`<String>` list = Arrays.asList\("DC","CD","AD"\);  
list.stream\(\).sorted\(\).forEach\(s-&gt;System.out.println\(s\)\);  
结果：AD CD DC

##### distinct - 清除重复数据

Stream`<String>` uniqueWordStream = Stream.of\("a", "b", "b", "c", "a", "d"\).distinct\(\);  
List`<String>` uniqueWords = uniqueWordStream.collect\(Collectors.toList\(\)\);

##### limit

##### 对一个Stream进行截断操作，获取其前N个元素，如果原Stream中包含的元素个数小于N，那就获取其所有的元素；

private class Person {  
public int no;  
private String name;   
public int age;   
public Person \(int no, String name,int age\) {

this.no = no;  
this.name = name;   
}  
public String getName\(\) {  System.out.println\(name\);  return name;  }  
public int getAge\(\) {  System.out.println\(age\);  return age;  }  
 }  
List`<Person>` persons = new ArrayList\(\);  
for \(int i = 1; i &lt;= 10; i++\) {  
Person person = new Person\(i, "name" + i,0\);  
persons.add\(person\);   
}   
List`<String>` personList2 = persons.stream\(\).map\(Person::getName\).limit\(8\). collect\(Collectors.toList\(\)\);   
System.out.println\(personList2\); }  
//name1 name2 name3 name4 name5 name6 name7 name8

##### skip

##### 返回一个丢弃原Stream的前N个元素后剩下元素组成的新Stream，如果原Stream中包含的元素个数小于N，那么返回空Stream；

List`<String>` personList2 = persons.stream\(\).map\(Person::getName\).skip\(3\). collect\(Collectors.toList\(\)\);   
System.out.println\(personList2\); }  
//name4 name5 name6 name7 name8 name9 name10

##### forEach

##### forEach 方法接收一个 Lambda 表达式，然后在 Stream 的每一个元素上执行该表达式。forEach 是 terminal 操作，因此它执行后，Stream 的元素就被“消费”掉了，无法对一个 Stream 进行两次 terminal 运算

Persons.stream\(\).forEach\(p -&gt; System.out.println\(p.getName\(\)\)\);

##### 相反，具有相似功能的 intermediate 操作 peek 可以达到上述目的

##### peek

##### 生成一个包含原Stream的所有元素的新Stream，同时会提供一个消费函数（Consumer实例），新Stream每个元素被消费的时候都会执行给定的消费函数；

peek 对每个元素执行操作并返回一个新的 Stream  
Stream.of\("one", "two", "three", "four"\)  .filter\(e -&gt; e.length\(\) &gt; 3\)  .peek\(e -&gt; System.out.println\("Filtered value: " + e\)\)  .map\(String::toUpperCase\)  .peek\(e -&gt; System.out.println\("Mapped value: " + e\)\)  .collect\(Collectors.toList\(\)\);

##### reduce

##### 这个方法的主要作用是把 Stream 元素组合起来。它提供一个起始值，然后依照运算规则\(BinaryOperator\)，和前面 Stream 的第一个、第二个、第 n 个元素组合。

字符串连接，concat = "ABCD"  
String concat = Stream.of\("A", "B", "C", "D"\).reduce\("", String::concat\);

##### Match

##### Stream 有三个 match 方法，从语义上说：

##### allMatch：Stream 中全部元素符合传入的条件，返回 true

##### anyMatch：Stream 中只要有一个元素符合传入的条件，返回 true

##### noneMatch：Stream 中没有一个元素符合传入的条件，返回 true

List persons = new ArrayList\(\);

persons.add\(new Person\(1, "name" + 1, 10\)\);

persons.add\(new Person\(2, "name" + 2, 21\)\);

persons.add\(new Person\(3, "name" + 3, 34\)\);

persons.add\(new Person\(4, "name" + 4, 6\)\);

persons.add\(new Person\(5, "name" + 5, 55\)\);

boolean isAllAdult = persons.stream\(\).allMatch\(p -&gt; p.getAge\(\) &gt; 18\);

System.out.println\("All are adult? " + isAllAdult\);

boolean isThereAnyChild = persons.stream\(\).anyMatch\(p -&gt; p.getAge\(\) &lt; 12\);

System.out.println\("Any child? " + isThereAnyChild\);  
boolean isNoneChild = persons.stream\(\).noneMatch\(p -&gt; p.getAge\(\) &lt; 1\);

System.out.println\("None child? " + isNoneChild\);  
输出结果：

All are adult? false

Any child? true

None child? true

##### collect

##### 将stream返回的元素拼成ArrayList。参数为Collectros接口

##### Collectors工具类提供toList方法，用来把元素添加到ArrayList中

使用上面的person：List`<Person>` olderThan20 =persons.stream\(\).filter\(person -&gt; person.getAge\(\) &gt; 20\).collect\(Collectors.toList\(\)\);

##### Collectors toMap方法

List`<person>` list = new ArrayList&lt;&gt;\(\);  
list.add\(new Person\(4, "name1", 6\)\);  
list.add\(new Person\(4, "name2", 6\)\);  
list.add\(new Person\(4, "name3", 6\)\);  
Map`<Integer, String>` result2 = list.stream\(\).collect\(Collectors.toMap\(x -&gt; x.getAge\(\), x -&gt; x.getName\(\)\)\);

##### Collectors joining方法

list.stream\(\).map\(p -&gt; getName\(\)\).collect\(joining\("~"\)\);

