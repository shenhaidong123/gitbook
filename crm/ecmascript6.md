# ECMAScript6

#### 作者：李静

#### 邮箱：lijing@haomo-studio.com
参考资料：http://es6.ruanyifeng.com/#README

## 1.ES6简介

ES6是2015年6月正式发布的，可用来编写复杂的大型应用程序。作为企业级的开发语言来使用。

1996年11月，NetScape公司（JavaScript创造者），将JavaScript提交给国际标准化组织ECMA。

1997年，ECMA发布262号标准文件（ECMA-262）的第一版，规定了浏览器脚本语言的标准，并将这种语言称为ECMAScript，这个版本就是1.0版。

该标准从一开始就是针对JavaScript语言制定的，但是之所以不叫JavaScript，有两个原因。一是商标，Java是Sun公司的商标，根据授权协议，只有Netscape公司可以合法地使用JavaScript这个名字，且JavaScript本身也已经被Netscape公司注册为商标。二是想体现这门语言的制定者是ECMA，不是Netscape，这样有利于保证这门语言的开放性和中立性。

因此，ECMAScript和JavaScript的关系是，前者是后者的规格，后者是前者的一种实现（另外的ECMAScript方言还有Jscript和ActionScript）。日常场合，这两个词是可以互换的。

## 2.声明变量let、const

ES6里，声明变量的方式是六种：let、const、import、class、var、function

### （1）let

	{
	  let a = 10;
	  var b = 1;
	}
	
	a // ReferenceError: a is not defined.
	b // 1

```
1.声明变量，类似 var ，只在let所在的代码块内有效。
2.无变量提升，必须先声明，后使用，否则会报错。
3.不允许重复声明。在同一代码块内，不允许用let重复声明统一变量。
```

### （2）const

```
1.声明一个只读常量，一旦声明，则不可改变。
2.一旦声明，必须立刻赋值，否则报错。所以说是声明常量。
3.作用域同let，只在其所在的代码块内有效。
4.也必须先声明，后使用。不可重复声明
5.将对象冻结，使用Object.freeze();
```
	const foo = Object.freeze({});
	
	// 常规模式时，下面一行不起作用；
	// 严格模式时，该行会报错
	foo.prop = 123;

### （3）块级作用域

```
1.防止内层变量覆盖外层变量
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = "hello world";
  }
}
f();

2.防止用来记数的循环变量泄露成全局变量。
var s = 'hello';

for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}
console.log(i);

3.ES6允许块级作用域的随意嵌套。
{{{{
  let insane = 'Hello World';
  {let insane = 'Hello World'}
}}}};

4.块级作用域的推广，极大地降低了立即执行函数表达式（IIFE）的使用。
(function () {
  var tmp = ...;
  ...
}());
// 块级作用域写法
{
  let tmp = ...;
  ...
}
```

<!--ES6在附录B里面规定，浏览器的实现可以不遵守上面的规定，有自己的行为方式。

* 允许在块级作用域内声明函数。
* 函数声明类似于var，即会提升到全局作用域或函数作用域的头部。
* 同时，函数声明还会提升到所在的块级作用域的头部。
  注意，上面三条规则只对ES6的浏览器实现有效，其他环境的实现不用遵守，还是将块级作用域的函数声明当作let处理。-->

ES6规定暂时性死区和let、const语句不出现变量提升，主要是为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为。

## 3.变量的解构赋值

### （2）数组的解构赋值
	let [foo, [[bar], baz]] = [1, [[2], 3]];
	foo // 1
	bar // 2
	baz // 3
	
	let [ , , third] = ["foo", "bar", "baz"];
	third // "baz"
	
	let [x, , y] = [1, 2, 3];
	x // 1
	y // 3
	
	let [head, ...tail] = [1, 2, 3, 4];
	head // 1
	tail // [2, 3, 4]
	
	let [x, y, ...z] = ['a'];
	x // "a"
	y // undefined
	z // []
	
	let [x, y] = [1, 2, 3];
	x // 1
	y // 2
	
	let [a, [b], d] = [1, [2, 3], 4];
	a // 1
	b // 2
	d // 4
	
```
//允许指定默认值
var [foo = true] = [];
foo // true

[x, y = 'b'] = ['a']; // x='a', y='b'
[x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```

```
如果一个数组成员不严格等于undefined，默认值是不会生效的
var [x = 1] = [undefined];
x // 1

var [x = 1] = [null];
x // null

 //惰性求值
function f() {
  console.log('aaa');
}

let [x = f()] = [1];
```


### （3）对象的解构赋值

```
var { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

var { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
```

```
var { foo: baz } = { foo: "aaa", bar: "bbb" };  
baz // "aaa"  
foo // error: foo is not defined

在这里，foo是模式，baz是变量。模式不会被赋值。
```
	var node = {
	  loc: {
	    start: {
	      line: 1,
	      column: 5
	    }
	  }
	};
	
	var { loc: { start: { line }} } = node;
	line // 1
	loc  // error: loc is undefined
	start // error: start is undefined

对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。

	对象的默认值
	var {x = 3} = {};
	x // 3
	
	var {x, y = 5} = {x: 1};
	x // 1
	y // 5
	
	var {x:y = 3} = {};
	y // 3
	
	var {x:y = 3} = {x: 5};
	y // 5
	
	var { message: msg = 'Something went wrong' } = {};
	msg // "Something went wrong"

### （4）字符串的解构赋值

	const [a, b, c, d, e] = 'hello';
	a // "h"
	b // "e"
	c // "l"
	d // "l"
	e // "o"

```
类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。

let {length : len} = 'hello';
len // 5
```

### （5）数值和布尔值的解构赋值

```
解构赋值时，如果等号右边是数值和布尔值，则会先转为对象

let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```

由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。

	let { prop: x } = undefined; // TypeError
	let { prop: y } = null; // TypeError


### （4）函数参数的解构赋值

```
function move({x = 0, y = 0} = {}) {
      return [x, y];
}
move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]


function move({x, y} = { x: 0, y: 0 }) {
      return [x, y];
}
move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, undefined]
move({}); // [undefined, undefined]
move(); // [0, 0]
```

<!--### （4）圆括号问题

ES6的规则是，只要有可能导致解构的歧义，就不得使用圆括号。  
但是，这条规则实际上不那么容易辨别，处理起来相当麻烦。因此，建议只要有可能，就不要在模式中放置圆括号。

* 变量声明语句中，不能带有圆括号。  
* 函数参数中，模式不能使用圆括号。  
* 赋值语句中，不能将整个模式，或嵌套模式中的一层，放在圆括号之中。  -->

## 4.数组

### （1）Array.from\(\)
将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象

	let arrayLike = {
	    '0': 'a',
	    '1': 'b',
	    '2': 'c',
	    length: 3
	};
	let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
	
	// NodeList对象
	let ps = document.querySelectorAll('p');
	Array.from(ps).forEach(function (p) {
	  console.log(p);
	});
	
	// arguments对象
	function foo() {
	  var args = Array.from(arguments);
	  // ...
	}
	
	//字符串
	Array.from('hello')
	// ['h', 'e', 'l', 'l', 'o']
	
	//set结构
	let namesSet = new Set(['a', 'b'])
	Array.from(namesSet) // ['a', 'b']
只要是部署了Iterator接口的数据结构，Array.from都能将其转为数组。如果一个对象没有部署这个接口，就无法转换。Array.from方法则是还支持类似数组的对象。所谓类似数组的对象，本质特征只有一点，即必须有length属性。因此，任何有length属性的对象，都可以通过Array.from方法转为数组，而此时扩展运算符就无法转换。

	Array.from({ length: 3 });
	// [ undefined, undefined, undefined ]

Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

	Array.from(arrayLike, x => x * x);
	// 等同于
	Array.from(arrayLike).map(x => x * x);
	
	Array.from([1, 2, 3], (x) => x * x)
	// [1, 4, 9]


### （2）Array.of\(\)
用于将一组值，转换为数组。Array.of(3, 11, 8) // [3,11,8]。  
Array.of基本上可以用来替代Array()或new Array()，他的目的是为了防止以下情况的发生。

	Array() // []
	Array(3) // [, , ,]
	Array(3, 11, 8) // [3, 11, 8]
### （3）copyWithin(）
作用：在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。  
注意：使用这个方法，会修改当前数组。  

#### Array.prototype.copyWithin(target, start = 0, end = this.length)

- target（必需）：从该位置开始替换数据。
- start（可选）：从该位置开始读取数据，默认为0。如果为负值，表示倒数。
- end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。

```
[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]

// 将3号位复制到0号位
[1, 2, 3, 4, 5].copyWithin(0, 3, 4)
// [4, 2, 3, 4, 5]

// -2相当于3号位，-1相当于4号位
[1, 2, 3, 4, 5].copyWithin(0, -2, -1)
// [4, 2, 3, 4, 5]

```

### （3）Array.find(）和	Array.findIndex(）
#### find
作用：找出第一个符合条件的数组成员  
参数：参数是一个回调函数  
<<<<<<< HEAD
返回值：返回符合条件的第一个该成员，无则返回undefined

```
[1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;
})
```
=======
返回值：返回符合条件的第一个该成员，无则返回undefined  
>>>>>>> 7c35a6112d76ed1accb79fc36f44822aed484a6e

	[1, 5, 10, 15].find(function(value, index, arr) {
	  return value > 9;
	}) 
	
#### findIndex
同find类似，返回值是符合条件的成员的位置。如果没有则返回-1，类似字符串的IndexOf方法。
  
同时这两种方法都可以发现NaN。

此外数组还有其他的一些方法，可以自行学习一下。
  
- Array.fill()		//使用给定值，填充一个数组。
- Array.entries()	// 遍历，对键名和键值遍历
- Array.keys()	//遍历，对键名的遍历
- Array.values()		//遍历，对键值的遍历
- Array.includes()	//表示某个数组是否包含给定的值,返回布尔值

### （3）数组空位问题

ES5中

* forEach\(\), filter\(\), every\(\) 和some\(\)都会跳过空位。
* map\(\)会跳过空位，但会保留这个值
* join\(\)和toString\(\)会将空位视为undefined，而undefined和null会被处理成空字符串。

ES6中  
ES6则是明确将空位转为undefined。  
扩展运算符（...）也会将空位转为undefined，copyWithin\(\)会连空位一起拷贝，fill\(\)会将空位视为正常的数组位置。for...of循环也会遍历空位。entries\(\)、keys\(\)、values\(\)、find\(\)和findIndex\(\)会将空位处理成undefined。

```
let arr = [, ,];    //两个空位
for (let i of arr) {
      console.log(1);
}
// 1
// 1

由于空位的处理规则非常不统一，所以建议避免出现空位。
```

## 5.函数

### （1）rest参数        "...变量名"

类似数组。所以数组特有的方法都可以用于这个变量。注意，rest参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。函数的length属性\(参数的长度\)，不包括rest参数。

	function add(...values) {
	  let sum = 0;
	
	  for (var val of values) {
	    sum += val;
	  }
	
	  return sum;
	}
	
	add(2, 5, 3) // 10

### （2）扩展运算符    "..."
console.log(...[1, 2, 3])
// 1 2 3

console.log(1, ...[2, 3, 4], 5)
// 1 2 3 4 5

[...document.querySelectorAll('div')]
// [<div>, <div>, <div>]

* 它好比rest参数的逆运算，将一个数组转为用逗号分隔的参数序列。  
* 由于扩展运算符可以展开数组，所以不再需要apply方法，将数组转为函数的参数了
* 合并数组        \[1, 2\].concat\(more\) == \[1, 2, ...more\]
* 与解构赋值结合  \( a = list\[0\], rest = list.slice\(1\) \)== \( \[a, ...rest\] = list \)
* 扩展运算符只能放在数组的租后一位。\[first, ...rest\] 
* 以将字符串转为真正的数组    \[...'hello'\] --&gt; \[ "h", "e", "l", "l", "o" \]

### （2）严格模式

只要函数参数使用了默认值、解构赋值、或者扩展运算符，那么函数内部就不能显式设定为严格模式，否则会报错。两种方法可以规避这种限制：（1）设定全局性的严格模式，这是合法的；（2）把函数包在一个无参数的立即执行函数里面。

### （2）name属性

函数的name属性，返回该函数的函数名。如果将一个匿名函数赋值给一个变量，ES5 的name属性，会返回空字符串，而 ES6 的name属性会返回实际的函数名。Function构造函数返回的函数实例，name属性的值为anonymous。bind返回的函数，name属性值会加上bound前缀。例如

```
function foo() {};
foo.bind({}).name // "bound foo"

(function(){}).bind({}).name // "bound "
```

### （2）箭头函数

```
var f = v => 5;
v : 参数  //如果没有或多个则直接写"(a,b)"
=> : 函数体
5 : 返回值 
- 如果代码多余1条语句则用大括号括起来，并用return返回 "{ return num1 + num2; }"
- 如果箭头函数直接返回一个对象，必须在对象外面加上括号。id => ({ id: id, name: "Temp" });

使用注意点
```

箭头函数有几个使用注意点。

```
（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。
（4）不可以使用yield命令，因此箭头函数不能用作Generator函数。
```

this指向的固定化，并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，导致内部的this就是外层代码块的this。正是因为它没有this，所以也就不能用作构造函数。

<!--### 【等】（2）函数绑定运算符

函数绑定运算符是并排的两个双冒号（::），双冒号左边是一个对象，右边是一个函数。该运算符会自动将左边的对象，作为上下文环境（即this对象），绑定到右边的函数上面。-->

## 6.对象

### （1）对象的简介写法

```
function f(x, y) {
      return {x, y};
}

// 等同于
function f(x, y) {
      return {x: x, y: y};
}

f(1, 2) // Object {x: 1, y: 2}

CommonJS模块输出变量，就非常合适使用简洁写法。
```

简洁写法的属性名总是字符串

### （2）属性名的表达式

ES6 允许字面量定义对象时，用方法二（表达式）作为对象的属性名，即把表达式放在方括号内。也适用于定义方法名。注意，属性名表达式如果是一个对象，默认情况下会自动将对象转为字符串\[object Object\]，这一点要特别小心。

```
let propKey = 'foo';

let obj = {
          [propKey]: true,
         ['a' + 'bc']: 123
    };
```

### （2）Object.is\(\)

Object.is\(\) 类似于严格相等 "==="

```
Object.is('foo', 'foo')//true
和===不同处
+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

### （2）Object.assign\(\)

用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target），若有同名属性，则会覆盖。如果参数不是对象，则会先转化为对象，但是"undefind"和"null"都不可转化为对象那个，若作为参数传入，会报错

```
var target = { a: 1 };

var source1 = { b: 2 };
var source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

Object.assign方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。有一些函数库提供Object.assign的定制版本（比如Lodash的\_.defaultsDeep方法），可以解决浅拷贝的问题，得到深拷贝的合并。

### （2）属性遍历

* for...in
* Object.keys\(obj\)
* Object.getOwnPropertyNames\(obj\)
* Object.getOwnPropertySymbols\(obj\)
* Reflect.ownKeys\(obj\)

### （3）Object.keys\(\)，Object.values\(\)，Object.entries\(\)
用法同数组。
## 7.Symbol

Symbol是一种数据类型，表示独一无二的值。与Undefined、Null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）同级。

Symbol值通过Symbol函数生成。这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的Symbol类型。凡是属性名属于Symbol类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。

```
let s = Symbol();
typeof s
// "symbol"
```

不能使用new，直接使用该方法可获得值，他不是对象，所以无法添加属性。他是一种类似字符串的数据类型。在使用的时候，可以接受字符串作为参数，是用来对实例的描述，可以来用进行区分。

```
var s1 = Symbol('foo');
var s2 = Symbol('bar');

s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
```

注意，Symbol函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的Symbol函数的返回值是不相等的。

	// 没有参数的情况
	var s1 = Symbol();
	var s2 = Symbol();
	
	s1 === s2 // false
	
	// 有参数的情况
	var s1 = Symbol('foo');
	var s2 = Symbol('foo');
	
	s1 === s2 // false
上面代码中，s1和s2都是Symbol函数的返回值，而且参数相同，但是它们是不相等的。

Symbol不能与其他数据类型来进行计算，会报错。另外，Symbol值也可以转为布尔值和字符串，但是不能转为数值。

```
var sym = Symbol();
Boolean(sym) // true
!sym  // false

sym.toString(); //"Symbol()";

Number(sym) // TypeError
sym + 2 // TypeError
```

<<<<<<< HEAD
Symbol值作为对象属性名时，不能用点运算符。

```
var mySymbol = Symbol();
var a = {};

a.mySymbol = 'Hello!';
a[mySymbol] // undefined
a['mySymbol'] // "Hello!"
```
=======
Symbol值作为对象属性名时，不能用点运算符。 
>>>>>>> 7c35a6112d76ed1accb79fc36f44822aed484a6e

	var mySymbol = Symbol();
	var a = {};
	
	a.mySymbol = 'Hello!';
	a[mySymbol] // undefined
	a['mySymbol'] // "Hello!" 
在对象的内部，使用Symbol值定义属性时，Symbol值必须放在方括号之中。

	let s = Symbol();
	
	let obj = {
	  [s]: function (arg) { ... }
	};
	
	obj[s](123);
	
## 8.Set和Map数据结构

### （1）Set

Set本身是一个构造函数，用来生成Set数据结构。

```
var s = new Set();
[2, 3, 5, 4, 5, 2, 2].map(x => s.add(x));
for (let i of s) {
     console.log(i);
}
// 2 3 5 4
```

* 类似于数组，但是成员的值都是唯一的，没有重复的值
* 参宿可以为数组，可以用来去重
* 两个对象总是不等的，不会被自动去掉
* NaN是等于NaN的

### （2）Set实例

操作方法

* add\(value\)：添加某个值，返回Set结构本身。
* delete\(value\)：删除某个值，返回一个布尔值，表示删除是否成功。
* has\(value\)：返回一个布尔值，表示该值是否为Set的成员。
* clear\(\)：清除所有成员，没有返回值。  

遍历方法

* keys\(\)：返回键名的遍历器
* values\(\)：返回键值的遍历器
* entries\(\)：返回键值对的遍历器
* forEach\(\)：使用回调函数遍历每个成员

### （3）WeakSet

类似Set，也是不重复的值的集合。  
var ws = new WeakSet\(\);

```
区别：
- WeakSet的成员只能是对象，而不能是其他类型的值。
- WeakSet中的对象都是弱引用，WeakSet是不可遍历的。
```

WeakSet方法：

* WeakSet.prototype.add\(value\)：向WeakSet实例添加一个新成员。
* WeakSet.prototype.delete\(value\)：清除WeakSet实例的指定成员。
* WeakSet.prototype.has\(value\)：返回一个布尔值，表示某个值是否在WeakSet实例之中。

### （4）Map

Map是一种数据结构，类似对象的"键值对"的形式，但他是"值-值",不只是字符串可以当做键，对象，数组也可以当做键。是一种更完善的Hash结构实现。

对象作为参数

```
var m = new Map();
var o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false
```

数组作为参数

```
var map = new Map([
      ['name', '张三'],
      ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"

等同于
var items = [
      ['name', '张三'],
      ['title', 'Author']
];
var map = new Map();
items.forEach(([key, value]) => map.set(key, value));
```

* 如果对同一键多次赋值，会覆盖。
* 只有对同一个对象的引用，Map结构才将其视为同一个键。比如set时，直接map.set\(\['a'\]\)和map.get\(\['a'\]\);是不同的键，因为内存地址不同。如果想引用同一个件，要复制给某个变量。
* map中（+0，-0）（NaN，NaN）为同一键值

#### map实例的属性

* map.size   //返回长度
* map.set\(key,value\)  //返回map，可采用链式操作
* map.get\(key\)    //返回对应值，若无则返回undefined
* map.has\(key\)    //返回布尔值
* map.delete\(key\)    //返回布尔值
* map.clear\(\)        //无返回值

#### map遍历（Map的遍历顺序就是插入顺序）

* keys\(\)：返回键名的遍历器。
* values\(\)：返回键值的遍历器。
* entries\(\)：返回所有成员的遍历器。
* forEach\(\)：遍历Map的所有成员。  
  \[...map\] === \[...map.entries\(\)\]

### （4）WeakMap

WeakMap结构与Map结构基本类似，唯一的区别是它只接受对象作为键名（null除外），不接受其他类型的值作为键名，而且键名所指向的对象，不计入垃圾回收机制 （同WeakSet）

## 9.class
ES6的class的绝大部分功能，ES5都可以做到。新的class只是让对象原型的写法更加清晰明了，更偏向于面向对象的写法。来替代构造函数。

	class Point {
	  constructor(x, y) {    //constructor，toString为构造方法
	    this.x = x;			//this代表实例对象
	    this.y = y;
	  }
	
	  toString() {
	    return '(' + this.x + ', ' + this.y + ')';
	  }
	}
	
	typeof Point // "function"
	
注意，

<<<<<<< HEAD
* 一个类可定义多个方法，方法之剑不需要加","，并且构造方法前面不加function。
* 类的使用方法，和构造函数是一样的，也是用new来起一个实例。
* 类上面也存在prototype属性，并且和类里面的方法是一致的。
* 如果添加新的方法，直接在prptotype上面添加。Object.assign可一次新添加多个属性。  
    Object.assign\(Point.prototype, {

  ```
  toString(){},
  toValue(){}
  ```

  }\);

* 类的内部所有定义的方法，都是不可枚举的（non-enumerable）,而构造函数是可以枚举的。  
    Object.keys\(Point.prototype\)  
    // \[\]  
    Object.getOwnPropertyNames\(Point.prototype\)

* 类的属性名可以使用表达式。

#### constructor方法

* constructor是类的默认的方法，且是必须有的，
* 如果没有显式定义，一个空的constructor方法会被默认添加。
* 使用new时，自动执行该函数。
* 默认返回实例对象（即this）,可指定
* 如想执行，必须使用new。而构造函数则不需要必须使用new

#### 类的实例对象

=======
- 一个类可定义多个方法，方法之剑不需要加","，并且构造方法前面不加function。
- 类的使用方法，和构造函数是一样的，也是用new来起一个实例。
- 类上面也存在prototype属性，并且和类里面的方法是一致的。
- 如果添加新的方法，直接在prptotype上面添加。Object.assign可一次新添加多个属性。  
	Object.assign(Point.prototype, {
	  toString(){},
	  toValue(){}
	});
- 类的内部所有定义的方法，都是不可枚举的（non-enumerable）,而构造函数是可以枚举的。    
	Object.keys(Point.prototype)   
	// []   
	Object.getOwnPropertyNames(Point.prototype)
- 类的属性名可以使用表达式。

####  constructor方法
- constructor是类的默认的方法，且是必须有的，
- 如果没有显式定义，一个空的constructor方法会被默认添加。
- 使用new时，自动执行该函数。
- 默认返回实例对象（即this）,可指定
- 如想执行，必须使用new。而构造函数则不需要必须使用new

####  类的实例对象
>>>>>>> 7c35a6112d76ed1accb79fc36f44822aed484a6e
实例的属性，除非显示定义在其本身（即直接用this定义），否则都是定义在原型上的。

	class Point {
	
	  constructor(x, y) {
	    this.x = x;
	    this.y = y;
	  }
	
	  toString() {
	    return '(' + this.x + ', ' + this.y + ')';
	  }
	
	}
	
	var point = new Point(2, 3);
	
	point.hasOwnProperty('x') // true
	point.hasOwnProperty('y') // true
	point.hasOwnProperty('toString') // false
	point.__proto__.hasOwnProperty('toString') // true

与ES5一样，类的所有实例共享一个原型对象。即p1._proto_ === p2._proto_  
所以改变通过实例的_protp_来添加方法，就是向原型添加方法，也就是所有的实例对象都会有这个方法。   
不存在变量提升，也就是说必须先定义后使用。

	{
	  let Foo = class {};
	  class Bar extends Foo {
	  }
	}   //不报错
	
####  class表达式
class可使用表达式，且name属性总是返回紧跟在class关键字后面的类名。如果没有则往前找。

	const MyClass1 = class Me {}; 
	MyClass1.name //"Me"
	
	const MyClass2 = class Me {}; 
	MyClass2.name //"MyClass2"
	
采用表达式可实现立即执行

<<<<<<< HEAD
```
let person = new class {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}('张三');

person.sayName();
```
=======
	let person = new class {
	  constructor(name) {
	    this.name = name;
	  }
	
	  sayName() {
	    console.log(this.name);
	  }
	}('张三');
	
	person.sayName(); 
>>>>>>> 7c35a6112d76ed1accb79fc36f44822aed484a6e

this指向：

	class Logger {
	  printName(name = 'there') {
	    this.print(`Hello ${name}`);
	  }
	
	  print(text) {
	    console.log(text);
	  }
	}
	
	const logger = new Logger();
	const { printName } = logger;
	printName(); // TypeError: Cannot read property 'print' of undefined
	
三种解决办法
	
	（1）在构造方法中绑定this，这样就不会找不到print方法了。
		class Logger {
		  constructor() {
		    this.printName = this.printName.bind(this);
		  }
		
		  // ...
		}
	（2）使用箭头函数。
		class Logger {
		  constructor() {
		    this.printName = (name = 'there') => {
		      this.print(`Hello ${name}`);
		    };
		  }
		
		  // ...
		}
	（3）使用Proxy，获取方法的时候，自动绑定this。
		function selfish (target) {
		  const cache = new WeakMap();
		  const handler = {
		    get (target, key) {
		      const value = Reflect.get(target, key);
		      if (typeof value !== 'function') {
		        return value;
		      }
		      if (!cache.has(value)) {
		        cache.set(value, value.bind(target));
		      }
		      return cache.get(value);
		    }
		  };
		  const proxy = new Proxy(target, handler);
		  return proxy;
		}
		
		const logger = selfish(new Logger());
		
####  extends

	class B extends A {}   //A可以是任意具有prototype属性的函数。
####  Object.getPrototypeOf()
	Object.getPrototypeOf() //从子类上获取父类，
	可以判断一个类是否继承了另一个类

####  class继承 （extends）
	class ColorPoint extends Point {
		constructor(x, y, color) {
		    super(x, y); // 必须使用super关键字，否则报错
		    this.color = color;
		}
	}
	
	class ColorPoint extends Point {}
	//相当于直接复制，是正确的，super方法被默认添加。
	
super关键字是父类的构造函数，在子类中用来初始化this，this指向子类，子类起始是没有自己的this对象的。并且必须先使用super，在定义this，否则会报错。
	
	let cp = new ColorPoint(25, 8, 'green');

	cp instanceof ColorPoint // true
	cp instanceof Point // true
	
	所以说cp既是父类Point的实例，也是子类ColorPoint的实例。
	
#### 类的prototype属性和__proto__属性
	class A {
	}
	
	class B extends A {
	}
	
	B.__proto__ === A // true
	B.prototype.__proto__ === A.prototype // true
作为一个对象，子类（B）的原型（__proto__属性）是父类（A）；  
作为一个构造函数，子类（B）的原型（prototype属性）是父类的实例。

## 9.Proxy和Reflect

## 10.Iterator和for...of循环

## 11.Generator 函数

## 12.Promise对象

## 13.异步操作和Async函数

## 14.Decorator

## 15.Module



