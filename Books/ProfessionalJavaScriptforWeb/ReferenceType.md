#第五章 引用类型

引用类型的值（对象）是引用类型的一个实例  
对象是某个特定引用类型的实例。新对象是使用`new`操作符后跟一个构造函数来创建的

#5.1 Object
在通过对象字面量定义对象时，实际上不会调用 Object 构造函数

对象字面量也是向函数传递大量可选参数的首选方式

方括号语法的主要优点是可以通过变量来访问属性

#5.2 Array 类型
构造函数传递一个值创建数组,如果传递的是数值，则会按照该数值创建包含给定项数的数组；而如果传递的是其他类型的参数，则会创建包含那个值
的只有一项的数组。

IE8bug：使用数组字面量表示法不要有多余的`,`或者空白的`,`

与对象一样，在使用数组字面量表示法时，也不会调用 Array 构造函数

如果将其 length 属性设置为大于数组项数的值，则新增的每一项都会取得 undefined 值

##5.2.1 检测数组
	value instanceof Array
	单一全局环境有效
ES5(IE9+):
	
	Array.isArray() 
准确检测数组:

	Object.prototype.toString.call(a).slice(8,-1).toLowerCase()

##5.2.2 转换方法

 - toString():返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串  
 - valueOf()：同上，为了创建这个字符串会调用数组每一项的 toString()方法。   
 - toLocaleString():调用的是每一项的 toLocaleString()方法，而不是 toString()方法  
 - join:使用不同的分隔符来构建这个字符串,如果不给 join()方法传入任何值，或者给它传入 undefined，则使用逗号作为分隔
符。 IE7 及更早版本会错误的使用字符串"undefined"作为分隔符。

##5.2.3 栈方法
 - push()返回修改后数组的长度
 - pop()返回移除的项

##5.2.4 队列方法
 -  shift():它能够移除数组中的第一个项并返回该项
 -  unshift():在数组前端添加任意个项并返回新数组的长度。

同时使用 unshift()和 pop()方法，可以从相反的方向来模拟队列，即在数组的前端添加项，从数组末端移除项

##5.2.5 重排序方法
reverse()和 sort()方法的返回值是经过排序之后的数组。

 - reverse():反转数组项的顺序
 -  sort():调用每个数组项的 toString()转型方法方法,按升序排列数组项
 -  sort(func)
 -  func:比较函数接收两个参数，如果第一个参数应该位于第二个之前则返回一个负数，如果两个参数相等
则返回 0，如果第一个参数应该位于第二个之后则返回一个正数

对于数值类型或者其 valueOf()方法会返回数值类型的对象类型:

	function compare(value1, value2){
		return value2 - value1;
	}

##5.2.6 操作方法
 - concat()方法可以基于当前数组中的所有项创建一个新数组
 - slice()它能够基于当前数组中的一或多个项创建一个新数组。 
	 - slice()方法可以接受一或两个参数，即要返回项的起始和结束位置
	 - slice()方法的参数中有一个负数，则用数组长度加上该数来确定相应的位置。如果结束位置小于起始位置，则返回空数组
 - splice() 删除前两个参数之间的项，插入后面的参数，返回被删除的项组成的数组

##5.2.7 位置方法
ES5(IE9+):

 - indexOf()
 - lastIndexOf()
 - 接收两个参数：要查找的项和（可选的）表示查找起点位置的索引

##5.2.8 迭代方法
ES5(IE9+):

 - every()：对数组中的每一项运行给定函数，如果该函数对每一项都返回 true，则返回 true。
 - filter()：对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。
 - forEach()：对数组中的每一项运行给定函数。这个方法没有返回值。
 - map()：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
 - some()：对数组中的每一项运行给定函数，如果该函数对任一项返回 true，则返回 true。
 - 参数为`function(item, index, array){};`

##5.2.9 归并方法
ES5(IE9+):

 - reduce()和 reduceRight()。这两个方法都会迭代数组的所有项，然后构建一个最终返回的值
 - 这两个方法都接收两个参数：一个在每一项上调用的函数和（可选的）作为归并基础的初始值。传给 reduce()和 reduceRight()的函数接收 4 个参数：前一个值、当前值、项的索引和数组对象`function(prev, cur, index, array)`

#5.3 Date 类型

	var now = new Date();
	自动获得当前日期和时间

参数：

 - 毫秒数
 - Date.parse()
	 - 接收字符串
 - Date.UTC()
	 - 接受年月日时分秒毫秒
	 - 月从0开始计数
 - 字符串
	 - 模拟上述方法
	 - 模拟Date.UTC()时返回本地时间
<!-- -->


	var someDate = new Date(Date.parse("May 25, 2004"));
	var someDate = new Date("May 25, 2004");
	// GMT 时间 2000 年 1 月 1 日午夜零时
	var y2k = new Date(Date.UTC(2000, 0));
	// GMT 时间 2005 年 5 月 5 日下午 5:55:55
	var allFives = new Date(Date.UTC(2005, 4, 5, 17, 55, 55));

Date.now():
ES5(IE9+):返回表示调用这个方法时的日期和时间的毫秒数
低版本使用：

	var time=+new Date()
	转化为字符串

##5.3.1继承方法与日期格式化方法

toLocaleString()，toString()，toDateString()，toTimeString()，toLocaleDateString()，toLocaleTimeString()，toUTCString()
！！以上方法因浏览器差异极大基本不可用

valueOf()返回毫秒数，可用于比较时间

#5.4 RegExp类型
元字符都必须转义

	( [ { \ ^ $ | ) ? * + .]}

使用RegExp 构造函数可以动态创建正则表达式，参数均为字符串  
由于 RegExp 构造函数的模式参数是字符串，所以在某些情况下要对字符进行双重转义。所有元字符都必须双重转义，那些已经转义过的字符也是如此，例如\n（字符\在字符串中通常被转义为\\，而在正则表达式字符串中就会变成\\\\）。  

 - ES3:正则表达式字面量始终会共享同一个 RegExp 实例，而使用构造函数创建的每一个新 RegExp 实例都是一个新实例
 - ES5:，使用正则表达式字面量必须像直接调用 RegExp 构造函数一样，每次都创建新的 RegExp 实例
 - 以ES5为标准

##5.4.2 RegExp实例方法
exec():捕获组

 - 接受一个参数，即要应用模式的字符串
 - 返回包含第一个匹配项信息的数组，index 表示匹配项在字符串中的位置，而 input 表示应用正则表达式的字符串。
 - 在数组中，第一项是与整个模式匹配的字符串，其他项是与模式中的捕获组匹配的字符串（如果模式中没有捕获组，则该数组只包含一项）
 - 从外向内捕获，依次加入数组
 - 在设置全局标志的情况下，每次调用 exec()则都会在字符串中继续查找新匹配项，捕获到更新lastIndex，每次只返回一项
 - ！IE 即使在非全局模式下，lastIndex 属性每次也会变化。

test()：接受一个字符串参数

toLocaleString()和 toString()显示其字符串表示  
 valueOf()方法返回正则表达式本身

##5.4.3 RegExp构造函数属性
RegExp:

 - RegExp.$_,RegExp["$`"]),RegExp["$'"]),RegExp["$&"],RegExp["$+"]),RegExp["$*"])
 - RegExp.$1、 RegExp.$2…RegExp.$9，分别用于存储第一、第二……第九个匹配的捕获组

#5.5 Function 类型
每个函数都是 Function 类型的实例  
函数是对象，函数名是指针

##5.5.1 没有重载（深入理解）
因为函数名是指针，后面的函数定义会覆盖前面的定义

##5.5.2 函数声明与函数表达式
函数声明：函数声明提升（function declaration hoisting），代码执行前提升

##5.5.3 作为值的函数

	function createComparisonFunction(propertyName) {
		return function(object1, object2) {
			var value1 = object1[propertyName];
			var value2 = object2[propertyName];
			if (value1 < value2) {
				return -1;
			} else if (value1 > value2) {
				return 1;
			} else {
				return 0;
			}
		};
	}

##5.5.4 函数内部属性
 - arguments:callee指向拥有这个 arguments 对象的函数  
 - this引用的是函数据以执行的环境对象
 - ES5 ES3(opera9.6以下除外):caller保存着调用当前函数的函数的引用
 - 严格模式，禁用callee，caller不可写，可用带有名字的函数表达式替代

##5.5.5 函数属性和方法
 - length参数长度
 - prototype保存它们所有实例方法的真正所在
 - 在创建自定义引用类型以及实现继承时， prototype 属性的作用是极为重要的。在 ES5 中， prototype 属性是不可枚举的，因此使用 for-in 无法发现。

 - apply()和 call()，在特定的作用域中调用函数，实际上等于设置函数体内 this 对象的值
 - apply()可接收arguments对象
 - 在严格模式下，未指定环境对象而调用函数，则 this 值不会转型为 window。除非明确把函数添加到某个对象或者调用 apply()或 call()，否则 this 值将是undefined。
 - 使用 call()（或 apply()）来扩充作用域的最大好处，就是对象不需要与方法有任何耦合关系

ES5:bind():bind()创建一个函数的实例，其 this 值会被绑定到传给 bind()函数的值

toLocaleString()、toString()、valueOf()方法始终都返回函数的代码

#5.6 基本包装类型
Boolean、 Number 和String

	(1) 创建 String 类型的一个实例；
	(2) 在实例上调用指定的方法；
	(3) 销毁这个实例。

 - 不要显式地调用 Boolean、 Number 和 String 来创建基本包装类型的对象  
 - Object 构造函数也会像工厂方法一样，根据传入值的类型返回相应基本包装类型的实例  
 - 使用 new 调用基本包装类型的构造函数，与直接调用同名的转型函数是不一样的

##5.6.1 Boolean类型
Boolean 类型的实例重写了 valueOf()方法，返回基本类型值 true 或 false；重写了 toString()方法，返回字符串"true"和"false"。  
布尔表达式中的所有对象都会被转换为 true  
永远不要使用 Boolean 对象。

##5.6.2 Number类型
valueOf()方法返回对象表示的基本类型的数值，toLocaleString()和 toString()返回字符串形式的数值。  
toString()方法传递一个表示基数的参数，告诉它返回几进制数值的字符串形式

 - toFixed()方法会按照指定的小数位返回数值的字符串表示，自动舍入 `{(-0.94,-0.5],[0.5,0.94)}`IE8 BUG 0
 - toExponential()，该方法返回以指数表示法（也称 e 表示法）表示的数值的字符串形式。接收一个参数，指定输出结果中的小数位数
 - toPrecision()方法可能会返回固定大小（fixed）格式，也可能返回指数（exponential）格式；具体规则是看哪种格式最合适。这个方法接收一个参数，即表示数值的所有数字的位数（不包括指数部分）
 - 以上方法均自动舍入
 - 不建议直接实例化 Number 类型

##5.6.3 String类型
length 属性
###1. 字符方法
 - charAt()返回指定位置字符
 - charCodeAt()返回指定位置字符编码
 - ES5(IE8+) s[i]返回指定位置字符

###2. 字符串操作方法
 -  concat()一或多个字符串拼接，返回拼接得到的新字符串
 -  + 字符串拼接
 -  slice()第一个参数指定子字符串的开始位置,第二个参数指定的是子字符串最后一个字符后面的位置
 -  substr()第一个参数指定子字符串的开始位置,第二个参数指定的则是返回的字符个数
 -  substring()第一个参数指定子字符串的开始位置,第二个参数指定的是子字符串最后一个字符后面的位置
 -  在传递给这些方法的参数是负值的情况下slice()方法会将传入的负值与字符串的长度相加， substr()方法将负的第一个参数加上字符串的长度，而将负的第二个参数转换为 0。最后， substring()方法会把所有负值参数都转换为 0。
 -  传入两个参数时结果与参数位置无关

###3. 字符串位置方法
indexOf()和 lastIndexOf() 搜索给定的子字符串，然后返子字符串的位置（如果没有找到该子字符串，则返回-1）。接收可选的第二个参数，表示从字符串中的哪个位置开始搜索  
在使用第二个参数的情况下，可以通过循环调用 indexOf()或 lastIndexOf()来找到所有匹配的子字符串

	while(pos > -1){
		positions.push(pos);
		pos = stringValue.indexOf("e", pos + 1);
	}

###4. trim()方法
ES5(IE9+)删除前置及后缀的所有空格，然后返回结果

###5. 字符串大小写转换方法
toLowerCase()、 toLocaleLowerCase()、 toUpperCase()和 toLocaleUpperCase()。

###6. 字符串的模式匹配方法
 - match():本例中的 match()方法返回了一个数组；如果是调用 RegExp 对象的 exec()方法也会得到与此相同的数组：数组的第一项是与整个模式匹配的字符串，之后的每一项（如果有）保存着与正则表达式中的捕获组匹配的字符串。
 - search()返回字符串中第一个匹配项的索引；如果没有找到匹配项，则返回-1。而且， search()方法始终是从字符串开头向后查找模式。
 - replace()第一个参数可以是一个 RegExp 对象或者一个字符串（这个字符串不会被转换成正则表达式），第二个参数可以是一个字符串或者一个函数。如果第一个参数是字符串，那么只会替换第一个子字符串。要想替换所有子字符串，唯一的办法就是提供一个正则表达式，而且要指定全局（g）标志