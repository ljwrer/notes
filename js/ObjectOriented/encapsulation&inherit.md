#JavaScript面向对象编程

 - 全局对象
 - 封装
 - 继承

#全局变量
##定义全局变量

	标准方法
	外层使用
	var test="value"定义
	
	变量定义在window对象上，可以用delete成功删除
	window.test="value"定义
	或
	(function(){
		test="value"
	})();
	
	其他
	a定义在函数内，test="value"定义在全局上
	function todo(){
		var a=test="value"
	}

 - 在程序任何地方都可以改写，引起命名冲突

#信息隐藏
##封装
构造函数和原型都会完全暴露信息

private:
在构造函数内使用var定义变量，下划线开头

protected:
用下划线开头

public：
不用下划线开头

#继承
##类继承(what the fuck?)

##组合寄生继承
私有属性和方法定义在构造函数中
公有属性和方法定义在原型中

私有属性和方法通过call父类型构造函数继承
私有属性和方法通过指定子类型原型为父类型原型的一个副本继承
##原型继承

	Object.create(proto);
	
	Function F()
	F.prototype=proto
	return new F();