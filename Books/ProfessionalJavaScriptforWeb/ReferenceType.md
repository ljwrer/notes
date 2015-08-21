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