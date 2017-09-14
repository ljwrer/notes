# 第一部分 类型和语法
## 第一章 类型
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

## 第二章 值
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

## 第三章 原生函数
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

### 3.1　内部属性 [[Class]]

    Object.prototype.toString.call

### 3.3　拆封
valueOf()

### 3.4　原生函数作为构造函数

    Array.apply( null, { length: 3 } )
    Error e.stack
    Object.getOwnPropertySymbols()

#### 3.4.5　原生原型
将原型作为默认值
Function.prototype 是一个空函数， RegExp.prototype 是一个“空”的正则表达式（无
任何匹配），而 Array.prototype 是一个空数组。对未赋值的变量来说，它们是很好的默
认值。