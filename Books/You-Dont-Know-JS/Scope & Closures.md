# 第一部分 作用域和闭包
# 第 1 章　作用域是什么
## 1.1 编译原理

 - 分词/词法分析
 - 解析/语法分析
 - 代码生成

## 1.2 理解作用域

 - 引擎:从头到尾负责整个JavaScript程序的编译及执行过程。
 - 编译器:负责语法分析及代码生成等脏活累活
 - 作用域:负责收集并维护由所有声明的标识符（变量）组成的一系列查询，并实施一套非常严格的规则，确定当前执行的代码对这些标识符的访问权限

###LHS RHS
LHS和RHS的含义是“赋值操作的左侧或右侧”并不一定意味着就是“=赋值操作符的左侧或右侧”。赋值操作还有其他几种形式，因此在概念上最好将其理解为“赋值操作的目标是谁（LHS）”以及“谁是赋值操作的源头（RHS）”。

 - LHS失败
	 - 严格模式：ReferenceError
	 - 非严格模式：创建全局变量
 - RHS失败
	 - TypeError
		 - 函数调用
		 - 属性读取

# 第 2 章　词法作用域
## 2.2　欺骗词法
### 2.2.1　eval

 - 严格模式：新建作用域
 - 非严格模式：当前作用域

### 2.2.1　with

 - with内作LHS引用
 - var 提升至外部作用域（undefined）
 - const,let 不提升也不写入
 - 读取到属性则赋值
 - 读取不到则根据LHS写入全局或var所在的外部作用域
 - with创建了新的词法作用域

### 2.2.3 性能
JavaScript引擎会在编译阶段进行数项的性能优化。其中有些优化依赖于能够根据代码的词法进行静态分析，并预先确定所有变量和函数的定义位置，才能在执行过程中快速找到标识符。
如果引擎在代码中发现了eval(..)或with，它只能简单地假设关于标识符位置的判断都是无效的，因为无法在词法分析阶段明确知道eval(..)会接收到什么代码，这些代码会如何对作用域进行修改，也无法知道传递给with用来创建新词法作用域的对象的内容到底是什么。
最悲观的情况是如果出现了eval(..)或with，所有的优化可能都是无意义的，因此最简单的做法就是完全不做任何优化

# 第 3 章　函数作用域和块作用域
##匿名函数表达式缺点

1. 匿名函数在栈追踪中不会显示出有意义的函数名，使得调试很困难。
2. 如果没有函数名，当函数需要引用自身时只能使用已经过期的arguments.callee引用，比如在递归中。另一个函数需要引用自身的例子，是在事件触发后事件监听器需要解绑自身。
3. 匿名函数省略了对于代码可读性/可理解性很重要的函数名。一个描述性的名称可以让代码不言自明。

##IIFE
	//IIEF	
	(function(){})()
	//另一种IIFE
	(function(){ .. }())
	//去掉全局变量的IIFE
	(function IIFE( global ) {})( window );
	//解决undefined标识符的默认值被错误覆盖导致的异常的IIFE
	(function IIFE( undefined ) {
		var a;
		if (a === undefined) {
		    console.log( "Undefined is safe here!" );
		}
	})();
	//倒置代码的运行顺序，将需要运行的函数放在第二位，在IIFE执行之后当作参数传递进去。（UMD模式）
	(function IIFE( def ) {
	    def( window );
	})(function def( global ) {
	    var a = 3;
	    console.log( a ); // 3
	    console.log( global.a ); // 2
	});

##3.4　块作用域

 - catch到的变量为块级作用域有效,但静态检查不会视为块域
 - 其他变量等效于with

###3.4.3 let
let进行的声明不会在块作用域中进行提升
#### 垃圾收集

	function process(data) {}
	var someReallyBigData = { .. };
	process( someReallyBigData );
	var btn = document.getElementById( "my_button" );
	btn.addEventListener( "click", function click(evt) {
	    console.log("button clicked");
	}, false );

click函数形成了一个覆盖整个作用域的闭包，JavaScript引擎极有可能依然保存着这个结构(实测V8已优化此问题)

	function process(data) {}
	// 在这个块中定义的内容可以销毁了！
	{
	    let someReallyBigData = { .. };
	    process( someReallyBigData );
	}
	var btn = document.getElementById( "my_button" );
	btn.addEventListener( "click", function click(evt) {
	    console.log("button clicked");
	}, false );

#第 4 章　提升
var和函数声明都会提升
##4.3　函数优先
出现在后面的函数声明还是可以覆盖前面的

！避免重复声明，尤其在分支语句中

#第 5 章　作用域闭包（狭义）
	function foo() {
	    var a = 2;
	
	    function bar() { 
	        console.log( a );
	    }
	
	    return bar;
	}
	var a=3;
	var baz = foo();
	
	baz(); // 2 

无论使用何种方式对函数类型的值进行传递，当函数在别处被调用时都可以观察到闭包。
无论通过何种手段将内部函数传递到所在的词法作用域以外，它都会持有对原始定义作用域的引用，无论在何处执行这个函数都会使用闭包。

	function foo() {
	    var a = 2;
	
	    function baz() {
	        console.log( a ); // 2
	    }
	
	    bar( baz );
	}
	
	function bar(fn) {
	    fn(); // 妈妈快看呀，这就是闭包！
	}
本质上无论何时何地，如果将函数（访问它们各自的词法作用域）当作第一级的值类型并到处传递，你就会看到闭包在这些函数中的应用。在定时器、事件监听器、Ajax请求、跨窗口通信、Web Workers或者任何其他的异步（或者同步）任务中，只要使用了回调函数，实际上就是在使用闭包！
###词法作用域查找与闭包查找
	var a = 2;
	(function IIFE() {
	    console.log( a );
	})();
IIFE并不是在它本身的词法作用域以外执行的。它在定义时所在的作用域中执行（而外部作用域，也就是全局作用域也持有a）。a是通过普通的词法作用域查找而非闭包被发现的。

##5.4　循环和闭包
	for (let i=1; i<=5; i++) {
	    setTimeout( function timer() {
	        console.log( i );
	    }, i*1000 );
	}

##5.5　模块
###1.接受参数的模块

	function CoolModule(id) {
	    function identify() {
	        console.log( id );
	    }
	    return {
	        identify: identify
	    };
	}
	
	var foo1 = CoolModule( "foo 1" ); 
	var foo2 = CoolModule( "foo 2" );
	
	foo1.identify(); // "foo 1"
	foo2.identify(); // "foo 2"
###2.动态模块
	var foo = (function CoolModule(id) {
	    function change() {
	        // 修改公共API
	        publicAPI.identify = identify2;
	    }
	
	    function identify1() { 
	        console.log( id );
	    }
	
	    function identify2() {
	        console.log( id.toUpperCase() );
	    }
	
	
	    var publicAPI = { 
	        change: change,
	        identify: identify1
	    };
	
	    return publicAPI;
	})( "foo module" );
	
	foo.identify(); // foo module
	foo.change();
	foo.identify(); // FOO MODULE

###5.5.1　现代的模块机制

	var MyModules = (function Manager() {
	    var modules = {};
	
	    function define(name, deps, impl) {
	        for (var i=0; i<deps.length; i++) {
	            deps[i] = modules[deps[i]];
	        }
	        modules[name] = impl.apply( impl, deps );
	    }
	
	    function require(name) {
	        return modules[name];
	    }
	
	    return {
	        define: define,
	        require:require
	    };
	})();

###5.5.2 未来模块机制
import export

##5.6 小结
当函数可以记住并访问所在的词法作用域，即使函数是在当前词法作用域之外执行，这时就产生了闭包。

模块有两个主要特征：(1)为创建内部作用域而调用了一个包装函数；(2)包装函数的返回值必须至少包括一个对内部函数的引用，这样就会创建涵盖整个包装函数内部作用域的闭包。

##A　动态作用域
JavaScript只有词法作用域，但是this机制某种程度上很像动态作用域。
主要区别：词法作用域是在写代码或者说定义时确定的，而动态作用域是在运行时确定的。（this也是！）词法作用域关注函数在何处声明，而动态作用域关注函数从何处调用。

##C this词法

 - self:词法作用域
 - => 继承作用域
	 - 匿名
 - bind 