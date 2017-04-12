# Google collenctions

#### 作者：杨丽

#### 邮箱：yangli@haomo-studio.com



### 集合Collections

####1.不可变集合

	不可变对象有很多优点，包括：
	
	当对象被不可信的库调用时，不可变形式是安全的；
	
	不可变对象被多个线程调用时，不存在竞态条件问题
	
	不可变集合不需要考虑变化，因此可以节省时间和空间。所有不可变的集合都比它们的可变形式有更好的内存利用率（分析和测试细节）；
	
	不可变对象因为有固定不变，可以作为常量来安全使用。

创建对象的不可变拷贝是一项很好的防御性编程技巧。Guava为所有JDK标准集合类型和Guava新集合类型都提供了简单易用的不可变版本。

JDK也提供了Collections.unmodifiableXXX方法把集合包装为不可变形式，但是相对Guava来说有些不足：

	笨重而且累赘：不能舒适地用在所有想做防御性拷贝的场景；
	
	不安全：要保证没人通过原集合的引用进行修改，返回的集合才是事实上不可变的；
	
	低效：包装过的集合仍然保有可变集合的开销，比如并发修改的检查、散列表的额外空间，等等。
	
	
#####1.创建不可变集合的三种方法:

一、使用builder创建：

Set`<String>` immutableNamedColors = ImmutableSet.`<String>`builder()
                .add("red", "green","black","white","grey")
                .build();
                
二、使用of静态方法创建：

 ImmutableSet.of("red","green","black","white","grey");
 
三、使用copyOf静态方法创建：

ImmutableSet.copyOf(new String[]{"red","green","black","white","grey"});

#####2. 使用asList()获得不可变集合的list视图
asList方法是在ImmutableCollection中定义，而所有的不可变集合都会从ImmutableCollection继承，所以所有的不可变集合都会有asList()方法返回当前不可变集合的list视图，这个视图也是不可变的。

#####3. 不可变集合的使用
不可变集合的使用和普通集合一样，只是不能使用他们的add，remove等修改集合的方法。

#####关联可变集合和不可变集合
	可变集合接口				  属于JDK还是Guava                不可变版本
	Collection				  JDK			               ImmutableCollection
	List		               JDK			               ImmutableList
	Set						     JDK			               ImmutableSet
	SortedSet/NavigableSet	  JDK	                      ImmutableSortedSet
	Map						     JDK			               ImmutableMap
	SortedMap				     JDK	                      ImmutableSortedMap
	Multiset				     Guava	                   ImmutableMultiset
	SortedMultiset			  Guava	                  ImmutableSortedMultiset
	Multimap	  			     Guava	                  ImmutableMultimap
	ListMultimap			     Guava	                  ImmutableListMultimap
	SetMultimap			     Guava	                  ImmutableSetMultimap
	BiMap					     Guava	                  ImmutableBiMap
	ClassToInstanceMap		  Guava	                ImmutableClassToInstanceMap
	Table					     Guava	                  ImmutableTable

####2.新集合类型
Guava引入了很多JDK没有的、但我们发现明显有用的新集合类型。这些新类型是为了和JDK集合框架共存，而没有往JDK集合抽象中硬塞其他概念。作为一般规则，Guava集合非常精准地遵循了JDK接口契约。

#####Multiset
Guava提供了一个新集合类型 Multiset，它可以多次添加相等的元素。

Multiset看似是一个Set，但是实质上不是一个Set，它没有继承Set接口，它继承的是Collection`<E>`接口，你可以向Multiset中添加重复的元素，Multiset会对添加的元素做一个计数。
	Multiset方法：
	
	count(E)	给定元素在Multiset中的计数
	elementSet()	Multiset中不重复元素的集合，类型为Set`<E>`
	entrySet()	和Map的entrySet类似，返回Set`<Multiset.Entry<E>>`，其中包含的Entry支持getElement()和getCount()方法
	add(E, int)	增加给定元素在Multiset中的计数
	remove(E, int)	减少给定元素在Multiset中的计数
	setCount(E, int)	设置给定元素在Multiset中的计数，不可以为负数
	size()	返回集合元素的总个数（包括重复的元素）

Multiset的实现

Guava提供了多种Multiset的实现，大致对应JDK中Map的各种实现：

	Map					对应的Multiset		是否支持null元素
	HashMap			HashMultiset		是
	TreeMap			TreeMultiset	是（如果comparator支持的话）
	LinkedHashMap		LinkedHashMultiset	是
	ConcurrentHashMap	ConcurrentHashMultiset	否
	ImmutableMap	ImmutableMultiset	否
	
	
#####SortedMultiset

SortedMultiset是Multiset 接口的变种，它支持高效地获取指定范围的子集。比方说，你可以用 latencies.subMultiset(0,BoundType.CLOSED, 100, BoundType.OPEN).size()来统计你的站点中延迟在100毫秒以内的访问，然后把这个值和latencies.size()相比，以获取这个延迟水平在总体访问中的比例。

#####Multimap
Guava的 Multimap可以很容易地把一个键映射到多个值。
	
	可以用两种方式思考Multimap的概念：”键-单个值映射”的集合：
	
	a -> 1 a -> 2 a ->4 b -> 3 c -> 5
	
	或者”键-值集合映射”的映射：
	
	a -> [1, 2, 4] b -> 3 c -> 5
	
一般很少会直接使用Multimap接口，更多时候你会用ListMultimap或SetMultimap接口，它们分别把键映射到List或Set。

#####修改Multimap

	put(K, V)	添加键到单个值的映射	
	putAll(K, Iterable`<V>`)	依次添加键到多个值的映射
	remove(K, V)	移除键到值的映射；如果有这样的键值并成功移除，返回true。
	removeAll(K)	清除键对应的所有值，返回的集合包含所有之前映射到K的值，但修改这个集合就不会影响Multimap了。	
	replaceValues(K, Iterable`<V>`)	清除键对应的所有值，并重新把key关联到Iterable中的每个元素。返回的集合包含所有之前映射到K的值。
	
#####视图
asMap为Multimap`<K, V>`提供Map`<K,Collection<V>>`形式的视图。返回的Map支持remove操作，并且会反映到底层的Multimap，但它不支持put或putAll操作。更重要的是，如果你想为Multimap中没有的键返回null，而不是一个新的、可写的空集合，你就可以使用asMap().get(key)。（你可以并且应当把asMap.get(key)返回的结果转化为适当的集合类型——如SetMultimap.asMap.get(key)的结果转为Set，ListMultimap.asMap.get(key)的结果转为List——Java类型系统不允许ListMultimap直接为asMap.get(key)返回List——译者注：也可以用Multimaps中的asMap静态方法帮你完成类型转换）

entries用Collection`<Map.Entry<K, V>>`返回Multimap中所有”键-单个值映射”——包括重复键。（对SetMultimap，返回的是Set）

keySet用Set表示Multimap中所有不同的键。

keys用Multiset表示Multimap中的所有键，每个键重复出现的次数等于它映射的值的个数。可以从这个Multiset中移除元素，但不能做添加操作；移除操作会反映到底层的Multimap。

values()用一个”扁平”的Collection`<V>`包含Multimap中的所有值


#####Multimap的实现

Multimap提供了多种形式的实现。在大多数要使用Map`<K, Collection<V>>`的地方，你都可以使用它们：

	实现	键行为类似	值行为类似
	ArrayListMultimap	HashMap	ArrayList
	HashMultimap	HashMap	HashSet
	LinkedListMultimap*	LinkedHashMap*	LinkedList*
	LinkedHashMultimap**	LinkedHashMap	LinkedHashMap
	TreeMultimap	TreeMap	TreeSet
	ImmutableListMultimap	ImmutableMap	ImmutableList
	ImmutableSetMultimap	ImmutableMap	ImmutableSet
除了两个不可变形式的实现，其他所有实现都支持null键和null值

#####BiMap
BiMap`<K, V>`是特殊的Map：

可以用 inverse()反转BiMap`<K, V>`的键值映射
保证值是唯一的，因此 values()返回Set而不是普通的Collection
在BiMap中，如果你想把键映射到已经存在的值，会抛出IllegalArgumentException异常。如果对特定值，你想要强制替换它的键，请使用 BiMap.forcePut(key, value)。

BiMap的实现

	键–值实现	值–键实现	对应的BiMap实现
	HashMap	HashMap	HashBiMap
	ImmutableMap	ImmutableMap	ImmutableBiMap
	EnumMap	EnumMap	EnumBiMap
	EnumMap	HashMap	EnumHashBiMap
	
#####Table
当你想使用多个键做索引的时候,Guava为此提供了新集合类型Table，它有两个支持所有类型的键：”行”和”列”。Table提供多种视图，以便你从各种角度使用它：

rowMap()：用Map`<R, Map<C, V>>`Table`<R, C, V>`。同样的， rowKeySet()返回”行”的集合Set`<R>`。

row(r) ：用Map`<C, V>`返回给定”行”的所有列，对这个map进行的写操作也将写入Table中。
类似的列访问方法：columnMap()、columnKeySet()、column(c)。（基于列的访问会比基于的行访问稍微低效点）

cellSet()：用元素类型为Table.Cell`<R, C, V>`的Set表现Table`<R, C, V>`。Cell类似于Map.Entry，但它是用行和列两个键区分的。

示例：

	Table`<Vertex, Vertex, Double>` weightedGraph = HashBasedTable.create();
	
	weightedGraph.put(v1, v2, 4);
	
	weightedGraph.put(v1, v3, 20);
	
	weightedGraph.put(v2, v3, 5);
	
	 
	weightedGraph.row(v1); // returns a Map mapping v2 to 4, v3 to 20
	
	weightedGraph.column(v3); // returns a Map mapping v1 to 20, v2 to 5
	
Table有如下几种实现：

HashBasedTable：本质上用HashMap`<R, HashMap<C, V>>`实现；

TreeBasedTable：本质上用TreeMap`<R, TreeMap<C,V>>`实现；

ImmutableTable：本质上用ImmutableMap`<R, ImmutableMap<C, V>>`实现；注：ImmutableTable对稀疏或密集的数据集都有优化。

ArrayTable：要求在构造时就指定行和列的大小，本质上由一个二维数组实现，以提升访问速度和密集Table的内存利用率。
	
#####ClassToInstanceMap
ClassToInstanceMap是一种特殊的Map：它的键是类型，而值是符合键所指类型的对象。

为了扩展Map接口，ClassToInstanceMap额外声明了两个方法：T getInstance(Class`<T>`) 和T putInstance(Class`<T>`, T)，从而避免强制类型转换，同时保证了类型安全。

ClassToInstanceMap有唯一的泛型参数，通常称为B，代表Map支持的所有类型的上界。例如：

	
	ClassToInstanceMap`<Number>` numberDefaults=MutableClassToInstanceMap.create();
	
	numberDefaults.putInstance(Integer.class, Integer.valueOf(0));

ClassToInstanceMap`<B>`实现了Map`<Class<? extends B>, B>`——或者换句话说，是一个映射B的子类型到对应实例的Map。

#####RangeSet

RangeSet描述了一组不相连的、非空的区间。当把一个区间添加到可变的RangeSet时，所有相连的区间会被合并，空区间会被忽略。例如：

	RangeSet`<Integer>` rangeSet = TreeRangeSet.create();
	
	rangeSet.add(Range.closed(1, 10)); // {[1,10]}
	
	rangeSet.add(Range.closedOpen(11, 15));//不相连区间:{[1,10], [11,15)}
	
	rangeSet.add(Range.closedOpen(15, 20)); //相连区间; {[1,10], [11,20)}
	
	rangeSet.add(Range.openClosed(0, 0)); //空区间; {[1,10], [11,20)}
	
	rangeSet.remove(Range.open(5, 10)); //分割[1, 10]; {[1,5], [10,10], [11,20)}
	
请注意，要合并Range.closed(1, 10)和Range.closedOpen(11, 15)这样的区间，你需要首先用Range.canonical(DiscreteDomain)对区间进行预处理

RangeSet的视图

RangeSet的实现支持非常广泛的视图：

complement()：返回RangeSet的补集视图。complement也是RangeSet类型,包含了不相连的、非空的区间。

subRangeSet(Range`<C>`)：返回RangeSet与给定Range的交集视图。这扩展了传统排序集合中的headSet、subSet和tailSet操作。

asRanges()：用Set`<Range<C>>`表现RangeSet，这样可以遍历其中的Range。

asSet(DiscreteDomain`<C>`)（仅ImmutableRangeSet支持）：用ImmutableSortedSet`<C>`表现RangeSet，以区间中所有元素的形式而不是区间本身的形式查看。（这个操作不支持DiscreteDomain 和RangeSet都没有上边界，或都没有下边界的情况）

RangeSet的查询方法

为了方便操作，RangeSet直接提供了若干查询方法，其中最突出的有:

contains(C)：RangeSet最基本的操作，判断RangeSet中是否有任何区间包含给定元素。
rangeContaining(C)：返回包含给定元素的区间；若没有这样的区间，则返回null。
encloses(Range`<C>`)：简单明了，判断RangeSet中是否有任何区间包括给定区间。
span()：返回包括RangeSet中所有区间的最小区间。


#####RangeMap
RangeMap描述了”不相交的、非空的区间”到特定值的映射。和RangeSet不同，RangeMap不会合并相邻的映射，即便相邻的区间映射到相同的值。例如：


	RangeMap`<Integer, String>` rangeMap = TreeRangeMap.create();
	
	rangeMap.put(Range.closed(1, 10), "foo"); //{[1,10] => "foo"}
	
	rangeMap.put(Range.open(3, 6), "bar"); //{[1,3] => "foo", (3,6) => "bar", [6,10] => "foo"}
	
	rangeMap.put(Range.open(10, 20), "foo"); //{[1,3] => "foo", (3,6) => "bar", [6,10] => "foo", (10,20) => "foo"}
	
	rangeMap.remove(Range.closed(5, 11)); //{[1,3] => "foo", (3,5) => "bar", (11,20) => "foo"}

#####RangeMap的视图

RangeMap提供两个视图：

asMapOfRanges()：用Map`<Range<K>, V>`表现RangeMap。这可以用来遍历RangeMap。
subRangeMap(Range`<K>`)：用RangeMap类型返回RangeMap与给定Range的交集视图。这扩展了传统的headMap、subMap和tailMap操作。

####3. 强大的集合工具类  
java.util.Collections中未包含的集合工具
工具类与特定集合接口的对应关系

	集合接口			属于JDK还是Guava	         对应的Guava工具类
	Collection			JDK	          Collections2：不要和java.util.Collections混淆
	List				JDK	                        Lists
	Set					JDK                	        Sets
	SortedSet			JDK	                         Sets
	Map					JDK                	         Maps
	SortedMap			JDK   	                      Maps
	Queue				JDK	                         Queues
	Multiset			Guava	                     Multisets
	Multimap	  		Guava	                     Multimaps
	BiMap				Guava	                     Maps
	Table				Guava	                     Tables

#####静态工厂方法
在JDK 7之前，构造新的范型集合时要讨厌地重复声明范型：
	
	List`<TypeThatsTooLongForItsOwnGood>` list = new ArrayList`<TypeThatsTooLongForItsOwnGood>`();

因此Guava提供了能够推断范型的静态工厂方法：

	List`<TypeThatsTooLongForItsOwnGood>` list = Lists.newArrayList();
	Map`<KeyType, LongishValueType>` map = Maps.newLinkedHashMap();
可以肯定的是，JDK7版本的钻石操作符(`<>`)没有这样的麻烦：

	List`<TypeThatsTooLongForItsOwnGood>` list = new ArrayList`<>`();

用工厂方法模式，可以方便地在初始化时就指定起始元素。

	Set`<Type>` copySet = Sets.newHashSet(elements);
	List`<String>` theseElements = Lists.newArrayList("alpha", "beta", "gamma");
此外，通过为工厂方法命名（Effective Java第一条），我们可以提高集合初始化大小的可读性：

	List`<Type>`exactly100 = Lists.newArrayListWithCapacity(100);
	List`<Type>` approx100 = Lists.newArrayListWithExpectedSize(100);
	Set`<Type>` approx100Set = Sets.newHashSetWithExpectedSize(100);
Guava引入的新集合类型没有暴露原始构造器，也没有在工具类中提供初始化方法。而是直接在集合类中提供了静态工厂方法，例如：

	Multiset`<String>` multiset = HashMultiset.create();
	
####Iterables

Guava提供的工具方法更偏向于接受Iterable而不是Collection类型,很多支持所有集合的操作都在Iterables类中

下面列出了一些最常用的工具方法


	concat(Iterable`<Iterable>`)	 串联多个iterables的懒视图*   concat(Iterable...)
	
	frequency(Iterable, Object)	返回对象在iterable中出现的次数 与Collections.frequency (Collection,   Object)比较；Multiset
	
	partition(Iterable, int)	把iterable按指定大小分割，得到的子集都不能进行修改操作	Lists.partition(List, int)；paddedPartition(Iterable, int)
	
	getFirst(Iterable, T default)	返回iterable的第一个元素，若iterable为空则返回默认值	与Iterable.iterator(). next()比较;FluentIterable.first()
	
	getLast(Iterable)	返回iterable的最后一个元素，若iterable为空则抛出NoSuchElementException	getLast(Iterable, T default)；
	FluentIterable.last()
	
	elementsEqual(Iterable, Iterable)	如果两个iterable中的所有元素相等且顺序一致，返回true	与List.equals(Object)比较
	
	unmodifiableIterable(Iterable)	返回iterable的不可变视图	与Collections. unmodifiableCollection(Collection)比较
	
	limit(Iterable, int)	限制iterable的元素个数限制给定值	FluentIterable.limit(int)
	
	getOnlyElement(Iterable)	获取iterable中唯一的元素，如果iterable为空或有多个元素，则快速失败	getOnlyElement(Iterable, T default)

示例：

Iterable`<Integer>` concatenated = Iterables.concat(Ints.asList(1, 2, 3),Ints.asList(4, 5, 6)); 

// concatenated包括元素 1, 2, 3, 4, 5, 6

#####与Collection方法相似的工具方法

	
		方法			                            类似的Collection方法	                  等价的FluentIterable方法
	addAll(Collection addTo,   Iterable toAdd)	 Collection.addAll(Collection)
		
	contains(Iterable, Object)	                 Collection.contains(Object)	      FluentIterable.contains(Object)
	
	removeAll(Iterable   removeFrom, Collection toRemove)	Collection.removeAll(Collection)	
	
	retainAll(Iterable   removeFrom, Collection toRetain)	Collection.retainAll(Collection)	
	
	size(Iterable)	                              Collection.size()	                    FluentIterable.size()
	
	toArray(Iterable, Class)	                  Collection.toArray(T[])	            FluentIterable.toArray(Class)
	
	isEmpty(Iterable)	                         Collection.isEmpty()	   				FluentIterable.isEmpty()
	
	get(Iterable, int)							List.get(int)							FluentIterable.get(int)
	
	toString(Iterable)							Collection.toString()					FluentIterable.toString()
	
#####FluentIterable

FluentIterable还有一些便利方法用来把自己拷贝到不可变集合:

	ImmutableSet	toImmutableSet()
	ImmutableSortedSet	toImmutableSortedSet(Comparator)
	
#####Lists
Lists为List类型的对象提供了若干工具方法。

	partition(List, int)	把List按指定大小分割
	reverse(List)	返回给定List的反转视图。注: 如果List是不可变的，考虑改用ImmutableList.reverse()。

示例：

List countUp = Ints.asList(1, 2, 3, 4, 5);

List countDown = Lists.reverse(theList); // {5, 4, 3, 2, 1}

List`<List>` parts = Lists.partition(countUp, 2);//`{{1,2}, {3,4}, {5}}`

#####静态工厂方法

Lists提供如下静态工厂方法：

	ArrayList	basic, with elements, from Iterable, with exact capacity, with expected size, from Iterator
	LinkedList	basic, from Iterable

#####Sets


用copyInto(Set)拷贝进另一个可变集合；

用immutableCopy()对自己做不可变拷贝。

	方法
	union(Set, Set)//合并集合
	intersection(Set, Set)//取交集
	difference(Set, Set) //差集
	symmetricDifference(Set,   Set)//对称差集

使用范例：


Set`<String>` wordsWithPrimeLength = ImmutableSet.of("one", "two", "three", "six", "seven", "eight");

Set`<String>` primes = ImmutableSet.of("two", "three", "five", "seven");

SetView`<String>` intersection = Sets.intersection(primes,wordsWithPrimeLength);

// intersection包含"two", "three", "seven"

#####静态工厂方法

	HashSet	basic, with elements, from Iterable, with expected size, from Iterator
	LinkedHashSet	basic, from Iterable, with expected size
	TreeSet	basic, with Comparator, from Iterable
	
#####Maps

#####difference

Maps.difference(Map, Map)用来比较两个Map以获取所有不同点。该方法返回MapDifference对象

	entriesInCommon()	两个Map中都有的映射项，包括匹配的键与值
	entriesDiffering()	键相同但是值不同值映射项。返回的Map的值类型为MapDifference.ValueDifference，以表示左右两个不同的值
	entriesOnlyOnLeft()	键只存在于左边Map的映射项
	entriesOnlyOnRight()	键只存在于右边Map的映射项

示例： 	

	Map`<String, Integer>` left = ImmutableMap.of("a", 1, "b", 2, "c", 3);
	
	Map`<String, Integer>` left = ImmutableMap.of("a", 1, "b", 2, "c", 3);
	
	MapDifference`<String, Integer>` diff = Maps.difference(left, right);
	
	diff.entriesInCommon(); // {"b" => 2}
	
	diff.entriesInCommon(); // {"b" => 2}
	
	diff.entriesOnlyOnLeft(); // {"a" => 1}
	
	diff.entriesOnlyOnRight(); // {"d" => 5}
	
	静态工厂方法

Maps提供如下静态工厂方法：

	具体实现类型	工厂方法
	HashMap	basic, from Map, with expected size
	
	LinkedHashMap	basic, from Map
	
	TreeMap	basic, from Comparator, from SortedMap
	
	EnumMap	from Class, from Map
	
	ConcurrentMap：支持所有操作	basic
	
	IdentityHashMap	basic