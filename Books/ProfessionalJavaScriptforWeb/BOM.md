#第8章 BOM
##8.1 window对象
在浏览器中，window 对象有双重角色，它既是通过JavaScript 访问浏览器窗口的一个接口，又是ECMAScript 规定的Global 对象。

###8.1.1 全局作用域
全局变量不能通过delete 操作符删除，而直接在window 对象上的定义的属性可以。
尝试访问未声明的变量会抛出错误，但是通过查询window 对象，可以知
道某个可能未声明的变量是否存在。

###8.1.2 窗口关系及框架（frames）

	windows.frames[index]
	window.frames[<frameName>]