# 第一部分 类型和语法
# 第一章 类型
JavaScript 有 七 种 内 置 类 型： null、 undefined、 boolean、 number、 string、 object 和symbol，可以使用 typeof 运算符来查看。
变量没有类型，但它们持有的值有类型。类型定义了值的行为特征。
很多开发人员将 undefined 和 undeclared 混为一谈，但在 JavaScript 中它们是两码事。undefined 是值的一种。 undeclared 则表示变量还没有被声明过。
遗憾的是， JavaScript 却将它们混为一谈，在我们试图访问 "undeclared" 变量时这样报错： ReferenceError: a is not defined， 并 且 typeof 对 undefined 和 undeclared 变 量 都 返 回"undefined"。
然而，通过 typeof 的安全防范机制（阻止报错）来检查 undeclared 变量，有时是个不错的办法。
	
	// 这样会抛出错误
	if (DEBUG) {
		console.log( "Debugging is starting" );
	}
	//  - 这样是安全的
	if (typeof DEBUG !== "undefined") {
		console.log( "Debugging is starting" );
	}
	if (window.DEBUG) {
		// ..
	}

### ES6

 - symbol
 - Array.from

# 第二章 值
javaScript 中的数组是通过数字索引的一组任意类型的值。字符串和数组类似，但是它们的行为特征不同，在将字符作为数组来处理时需要特别小心。
JavaScript 中的数字包括“整数”和“浮点型”。
基本类型中定义了几个特殊的值。
null 类型只有一个值 null， undefined 类型也只有一个值 undefined。所有变量在赋值之前默认值都是 undefined。 void 运算符返回 undefined。
数 字 类 型 有 几 个 特 殊 值， 包 括 NaN（ 意 指“not a number”， 更 确 切 地 说 是“invalid number”）、 +Infinity、 -Infinity 和 -0。
简单标量基本类型值（字符串和数字等）通过值复制来赋值 / 传递，而复合值（对象等）通过引用复制来赋值 / 传递。 
JavaScript 中的引用和其他语言中的引用 / 指针不同，它们不能指向别的变量 / 引用，只能指向值。
### 字符串与字符数组
reverse

### ES6

 - 0x 0b 0o
 - Number.EPSILON
 - Number.MAX_SAFE_INTEGER
 - Number.MIN_SAFE_INTEGER
 - Number.isInteger()
 - Number.isSafeInteger()
 - Number.isNaN()
 - Object.is() 

# 第三章 原生函数
 - String()
 - Number()
 - Boolean()
 - Array()
 - Object()
 - Function()
 - RegExp()
 - Date()
 - Error()
 - Symbol()

Object.prototype.toString.call

## 3.1　内部属性 [[Class]]

    Object.prototype.toString.call

## 3.3　拆封
valueOf()

## 3.4　原生函数作为构造函数

    Array.apply( null, { length: 3 } )
    Error e.stack
    Object.getOwnPropertySymbols()

### 3.4.5　原生原型
将原型作为默认值
Function.prototype 是一个空函数， RegExp.prototype 是一个“空”的正则表达式（无
任何匹配），而 Array.prototype 是一个空数组。对未赋值的变量来说，它们是很好的默
认值。

# 第 4 章 强制类型转换
### 4.2.1 ToString
#### JSON 字符串化
(1) 字符串、数字、布尔值和 null 的 JSON.stringify(..) 规则与 ToString 基本相同。
(2) 如果传递给 JSON.stringify(..) 的对象中定义了 toJSON() 方法，那么该方法会在字符
串化前调用，以便将对象转换为安全的 JSON 值。

### 4.2.2 ToNumber
true 转换为 1， false 转换为 0。 undefined 转换为 NaN， null 转换为 0

抽象操作 ToPrimitive（参见 ES5 规范 9.1 节）会首先（通过内部操作 DefaultValue，参见 ES5 规范 8.12.8 节）检查该值是否valueOf() 方法。如果有并且返回基本类型值，就使用该值进行强制类型转换。如果没有就使用 toString()的返回值（如果存在）来进行强制类型转换。

### 4.2.3 ToBoolean
假值：

• undefined
• null
• false
• +0、 -0 和 NaN
• ""

#### 2. 假值对象（falsy object）
 document.all

## 4.3　显式强制类型转换
### 4.3.1　字符串和数字之间的显式转换
#### 操作符+  
String=>Number

#### 1. 日期显式转换为数字
```js
var timestamp = +new Date()
var timestamp = new Date().getTime();
var timestamp = Date.now();
```

#### 2. 奇特的 ~ 运算符
~x ≈ -(x+1) 
```js
var a = "Hello World";
if (~a.indexOf( "lo" )) { // true
// 找到匹配！
}
~a.indexOf( "ol" ); // 0 <-- 假值!
!~a.indexOf( "ol" ); // true
if (!~a.indexOf( "ol" )) { // true
// 没有找到匹配！
}
```

#### 3. 字位截除
~~ ≈ Math.floor

 - 只适合32位数字
 - 负数：向下取整

### 4.3.2　显式解析数字字符串
Number parseInt parseFloat
解析允许字符串中含有非数字字符，解析按从左到右的顺序，如果遇到非数字字符就停止。而转换不允许出现非数字字符，否则会失败并返回 NaN。


#### 解析非字符串
 toString->string->number

### 4.3.3　显式转换为布尔值
Boolean(..) 和 !!

## 4.4　隐式强制类型转换

### 4.4.2　字符串和数字之间的隐式强制类型转换
>根据 ES5 规范 11.6.1 节，如果某个操作数是字符串或者能够通过以下步骤转换为字符串
的话， + 将进行拼接操作。如果其中一个操作数是对象（包括数组），则首先对其调用
ToPrimitive 抽象操作（规范 9.1 节），该抽象操作再调用 [[DefaultValue]]（规范 8.12.8
节），以数字作为上下文。
如果valueOf() 操作无法得到简单基本类型值，则转而调用 toString()。
简单来说就是，如果 + 的其中一个操作数是字符串（或者通过以上步骤可以得到字符串），则执行字符串拼接；否则执行数字加法。

#### +和toString()
a + ""（隐式）和前面的 String(a)（显式）之间有一个细微的差别需要注意。根据ToPrimitive 抽象操作规则， a + "" 会对 a 调用 valueOf() 方法，然后通过 ToString 抽象操作将返回值转换为字符串。而 String(a) 则是直接调用 ToString()。
**\-**调用toNumber()

### 4.4.3　布尔值到数字的隐式强制类型转换
0,1

### 4.4.4　隐式强制类型转换为布尔值
 1. if (..) 语句中的条件判断表达式。
 2. for ( .. ; .. ; .. ) 语句中的条件判断表达式（第二个）。
 3. while (..) 和 do..while(..) 循环中的条件判断表达式。
 4. ? : 中的条件判断表达式。
 5. 逻辑运算符 ||（逻辑或）和 &&（逻辑与）左边的操作数（作为条件判断表达式）。

### 4.4.5 || 和 &&
&& 和 || 运算符的返回值并不一定是布尔类型，而是两个操作数其中一个的值。

>|| 和 && 首先会对第一个操作数（a 和 c）执行条件判断，如果其不是布尔值（如上例）就
先进行 ToBoolean 强制类型转换，然后再执行条件判断。
对于 || 来说，如果条件判断结果为 true 就返回第一个操作数（a 和 c）的值，如果为
false 就返回第二个操作数（b）的值。
&& 则相反，如果条件判断结果为 true 就返回第二个操作数（b）的值，如果为 false 就返
回第一个操作数（a 和 c）的值。
|| 和 && 返回它们其中一个操作数的值，而非条件判断的结果（其中可能涉及强制类型转
换）。 c && b 中 c 为 null，是一个假值，因此 && 表达式的结果是 null（即 c 的值），而非
条件判断的结果 false。

```js
a || b;
// 大致相当于(roughly equivalent to):
a ? a : b;
a && b;
// 大致相当于(roughly equivalent to):
a ? b : a;
```

### 4.4.6　符号的强制类型转换
ES6 允许从符号到字符串的显式强制类型转换，然而隐式强制类型转换会产生错误
```js
var s1 = Symbol( "cool" );
String( s1 ); // "Symbol(cool)"
var s2 = Symbol( "not cool" );
s2 + ""; // TypeError
```
符号不能够被强制类型转换为数字（显式和隐式都会产生错误），但可以被强制类型转换为布尔值（显式和隐式结果都是 true）。

## 4.5　宽松相等和严格相等
> == 允许在相等比较中进行强制类型转换，而 === 不允许

### 4.5.2　抽象相等
(1)如果两个值的类型相同，就仅比较它们是否相等

 - NaN 不等于 NaN
 - +0 等于 -0

(2)在比较两个对象的时候， == 和 === 的工作原理是一样的

(3)== 在比较两个不同类型的值时会发生隐式强制类型转换

#### 1. 字符串和数字之间的相等比较
(1) 如果 Type(x) 是数字， Type(y) 是字符串，则返回 x == ToNumber(y) 的结果。
(2) 如果 Type(x) 是字符串， Type(y) 是数字，则返回 ToNumber(x) == y 的结果。

#### 2. 其他类型和布尔类型之间的相等比较
(1) 如果 Type(x) 是布尔类型，则返回 ToNumber(x) == y 的结果；
(2) 如果 Type(y) 是布尔类型，则返回 x == ToNumber(y) 的结果。
无论什么情况下都不要使用 == true 和 == false。
```js
var a = '42'
// 不要这样用，条件判断不成立：
if (a == true) {
// ..
}
// 也不要这样用，条件判断不成立：
if (a === true) {
// ..
}
// 这样的显式用法没问题：
if (a) {
// ..
}
// 这样的显式用法更好：
if (!!a) {
// ..
}
// 这样的显式用法也很好：
if (Boolean( a )) {
// ..
}

```
#### 3. null 和 undefined 之间的相等比较
(1) 如果 x 为 null， y 为 undefined，则结果为 true。
(2) 如果 x 为 undefined， y 为 null，则结果为 true。
 a == null 这样的隐式强制类型转换在保证安全性的同时还能提高代码可读性

#### 4. 对象和非对象之间的相等比较
(1) 如果 Type(x) 是字符串或数字， Type(y) 是对象，则返回 x == ToPrimitive(y) 的结果；
(2) 如果 Type(x) 是对象， Type(y) 是字符串或数字，则返回 ToPromitive(x) == y 的结果。

安全运用隐式强制类型转换
我们要对 == 两边的值认真推敲，以下两个原则可以让我们有效地避免出错。
• 如果两边的值中有 true 或者 false，千万不要使用 ==。
• 如果两边的值中有 []、 "" 或者 0，尽量不要使用 ==。

>有一种情况下强制类型转换是绝对安全的，那就是 typeof 操作。 typeof 总是
返回七个字符串之一（参见第 1 章），其中没有空字符串。所以在类型检查
过程中不会发生隐式强制类型转换。 typeof x == "function" 是 100% 安全
的，和 typeof x === "function" 一样。事实上两者在规范中是一回事。所
以既不要盲目听命于代码工具每一处都用 ===，更不要对这个问题置若罔闻。
我们要对自己的代码负责。

## 4.6　抽象关系比较
(1)比较双方首先调用 ToPrimitive，如果结果出现非字符串，就根据 ToNumber 规则将双方强
制类型转换为数字来进行比较。
(2)如果比较双方都是字符串，则按字母顺序来进行比较：
(3)根据规范 a <= b 被处理为 b < a，然后将结果反转

相等比较有严格相等，关系比较却没有“严格关系比较”（strict relational comparison）。 也就是说如果要避免 a < b 中发生隐式强制类型转换，我们只能确保 a 和 b 为相同的类型，除此之外别无他法。
为了保证安全，应该对关系比较中的值进行显式强制类型转换：
```js
var a = [ 42 ];
var b = "043";
a < b; // false -- 字符串比较！
Number( a ) < Number( b ); // true -- 数字比较！
```

# 第 5 章 语法
## 5.1　语句和表达式
语句相当于句子，表达式相当于短语，运算符则相当于标点符号和连接词
### 5.1.1　语句的结果值
变量声明返回undefined
代码块 { .. } 的结果值是其最后一个语句 / 表达式的 结果

#### do 表达式
```js
var a, b;
a = do {
	if (true) {
		b = 4 + 38;
	}
};
a; // 42
```
### 5.1.2　表达式的副作用

 - 函数调用
 - 递增运算符 ++ 和递减运算符 -- 
 - delete 运算符
 - 赋值运算符
	 - 多个赋值语句串联

## 5.2　运算符优先级
&& 运算符的优先级高于 ||，而 || 的优先级又高于 ? :
### 5.2.1　短路
>对 && 和 || 来说，如果从左边的操作数能够得出结果，就可以忽略右边的操作数。我们将
这种现象称为“短路”（即执行最短路径）。

### 5.2.3　关联
 ? : 是右关联，并且它的组合方式会影响返回结果。
= 运算符是右关联

## 5.4　错误
在编译阶段发现的代码错误叫作“早期错误”（early error）。语法错误是早期错误的一种
（如 a = ,）。另外，语法正确但不符合语法规则的情况也存在。
这些错误在代码执行之前是无法用 try..catch 来捕获的，相反，它们还会导致解析 / 编译
失败。
#### 提前使用变量
ES6 规范定义了一个新概念，叫作 TDZ（Temporal Dead Zone，暂时性死区）。
TDZ 指的是由于代码中的变量还没有初始化而不能被引用的情况。
```js
{
	a = 2; // ReferenceError!
	let a;
}

{
	typeof a; // undefined
	typeof b; // ReferenceError! (TDZ)
	let b;
}

```
## 5.5　函数参数
向函数传递参数时， arguments 数组中的对应单元会和命名参数建立关联（linkage）以得
到相同的值。相反，不传递参数就不会建立关联。
但是，在严格模式中并没有建立关联这一说：
arguments 数组已经被废止（特别是在 ES6 引入剩余参数 ... 之后，

## 5.6 try..finally
finally 中的代码总是会在 try 之后执行，如果有 catch 的话则在 catch 之后执行。也可以
将 finally 中的代码看作一个回调函数，即无论出现什么情况最后一定会被调用。
>如果 try 中有 return 语句会出现什么情况呢？ return 会返回一个值，那么调用该函数并
得到返回值的代码是在 finally 之前还是之后执行呢？ 
这里 return 42 先执行，并将 foo() 函数的返回值设置为 42。然后 try 执行完毕，接着执
行 finally
try 中的 throw 也是如此
如果 finally 中抛出异常（无论是有意还是无意），函数就会在此处终止。如果此前 try 中
已经有 return 设置了返回值，则该值会被丢弃
continue 和 break 等控制语句也是如此：
finally 中的 return 会覆盖 try 和 catch 中 return 的返回值：

## 5.7 switch
 case 表达式的匹配算法与 ===相同
除简单值以外， case 中还可以出现各种表达式，它会将表达式的结果值和 true 进行比较。
因为 a == 42 的结果为 true，所以条件成立。
尽管可以使用 ==，但 switch 中 true 和 true 之间仍然是严格相等比较。即如果 case 表达
式的结果为真值，但不是严格意义上的 true，则条件不成立

# 混合环境 JavaScript
A.3　全局 DOM 变量
由于浏览器演进的历史遗留问题，在创建带有 id 属性的 DOM 元素时也会创建同名的全局变量
这也是尽量不要使用全局变量的一个原因。如果确实要用，也要确保变量名的唯一性，从
而避免与其他地方的变量产生冲突，包括 HTML 和其他第三方代码。