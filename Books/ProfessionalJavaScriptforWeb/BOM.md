#第8章 BOM
##8.1 window对象
在浏览器中，window 对象有双重角色，它既是通过JavaScript 访问浏览器窗口的一个接口，又是ECMAScript 规定的Global 对象。

###8.1.1 全局作用域

 - 全局变量（var定义，[[Configurable]]false）不能通过delete 操作符删除，而直接在window 对象上的定义的属性可以。
 - IE8使用delete删除 window 属性的语句时，会抛出错误
 - 尝试访问未声明的变量会抛出错误，但是通过查询window 对象，可以知道某个可能未声明的变量是否存在。

###8.1.2 窗口关系及框架（frames）
每个窗口都有独立的window对象，并保存在frames中

	windows.frames[index]
	window.frames[<frameName>]
	//top始终指向最外层的框架
	top.frames[index]
	top.frames[<frameName>]
	//parent（父）对象始终指向当前框架的直接上层框架
	最外层
	top===parent
	parent===window
	不同的window可通过 top 和 parent 连接

###8.1.3 窗口位置
screenLeft 和 screenTop窗口相对于屏幕左边和上边的位置（Firefox：screenLeft 和 screenTop）

	var leftPos = (typeof window.screenLeft == "number") ?window.screenLeft :window.screenX;
	var topPos = (typeof window.screenTop == "number") ?window.screenTop : window.screenY;

screenTop：

 - IE:包含工具栏等高度，farmeset不忽略边距
 - 其他：窗口离屏幕顶端高度，farmeset忽略边距

moveTo(x,y) moveBy(x，y)

###8.1.4 窗口大小

 - outerWidth、outerHeight
	 - 浏览器窗口,chrome中为viewport
 - innerWidth、innerHeight
	 - viewport
 - document.documentElement.clientHeight,document.documentElement.clientWidth
	 - viewport

			var pageWidth = window.innerWidth,
		            pageHeight = window.innerHeight;
		    if (typeof pageWidth != "number") {
		        if (document.compatMode == "CSS1Compat") {
		            pageWidth = document.documentElement.clientWidth;
		            pageHeight = document.documentElement.clientHeight;
		        } else {
		            pageWidth = document.body.clientWidth;
		            pageHeight = document.body.clientHeight;
		        }
		    }

###8.1.5 导航和打开窗口
window.open()

 - top.close
 - top.opener

####弹窗屏蔽检测

	try {
        var wroxWin = window.open("http://www.wrox.com", "_blank");
        if (wroxWin == null){
            blocked = true;
        }
    } catch (ex){
        blocked = true;
    }
    if (blocked){
        alert("The popup was blocked!");
    }

###8.1.6 间歇调用和超时调用
settimeout：

	//经过time时间将fn加入任务队列,任务队列中的fn依次执行
	settimeout(<fn>,<time>)
	//return:timeoutId 
	//从队列中丢弃
	clearTimeout(timeoutId)

 - 超时调用的代码都是在全局作用域中执行的，因此函数中 this 的值在非严格模式下指向 window 对象，在严格模式下是 undefined。
 - 不建议使用interval

###8.1.7 系统对话框

 - 异步
	 - window.find()
	 - window.print()
 - 同步
	 - alert
	 - prompt
	 - confirm

##8.2 location 对象
对url操作需要编码和反编码

	window.location===document.location

 - hash
 - host
 - hostname
 - href
 - pathname
 - port
 - protocal
 - search

###8.2.1 查询字符串参数

		function getQueryStringArgs() {
	        //取得查询字符串并去掉开头的问号
	        var qs = (location.search.length > 0 ? location.search.substring(1) : ""),
	        //保存数据的对象
	                args = {},
	        //取得每一项
	                items = qs.length ? qs.split("&") : [],
	                item = null,
	                name = null,	       
	                value = null,
	        //在 for 循环中使用
	                i = 0,
	                len = items.length;
	        //逐个将每一项添加到 args 对象中
	        for (i = 0; i < len; i++) {
	            item = items[i].split("=");
	            name = decodeURIComponent(item[0]);
	            value = decodeURIComponent(item[1]);
	            if (name.length) {
	                args[name] = value;
	            }
	        }
	        return args;
	    }

###8.2.2 位置操作
 - assign()
	 - @params:url
	 -  产生history
	 -  对window.location与location.href赋值也会调用assign
	 -  修改location 对象的其他属性也可以改变当前加载的页面（？#），每次修改 location 的属性（hash 除外，也会生成history），页面都会以新 URL 重新加载。
 -  replace()
	 -  @params:url
	 -  不会产生history,后退也不会返回前一个页面
 -  reload()
	 -  @params:Boolean
		 -  true: 强制服务器重新加载
		 -  false:可能从缓存加载
	 -  reload后的代码可能不执行

##8.3 navigator 对象
navigator.userAgent

###8.3.1 检测插件

	function hasPlugin(name) {
        name = name.toLowerCase();
        for (var i = 0; i < navigator.plugins.length; i++) {
            if (navigator.plugins [i].name.toLowerCase().indexOf(name) > -1) {
                return true;
            }
        }
        return false;
    }

	function hasIEPlugin(name) {
        try {
            new ActiveXObject(name);
            return true;
        } catch (ex) {
            return false;
        }
    }

	//  因IE插件检测方法与名称的不同，故推荐针对不同的插件创建不同的检测函数
	function hasFlash() {
        var result = hasPlugin("Flash");
        if (!result) {
            result = hasIEPlugin("ShockwaveFlash.ShockwaveFlash");
        }
        return result;
    }

 - refresh()
 	- @params:Boolean
	 	- true:重新加载包含插件的所有页面
	 	- false:只更新 plugins集合，不重新加载页面 

###8.3.2 注册处理程序

 - registerContentHandler()
	 - @params MIMEType
	 - @params MIMEPageUrl
	 - @params appName

			navigator.registerContentHandler("application/rss+xml","http://www.somereader.com?feed=%s", "Some Reader"); 

 - registerProtocolHandler()
	 - @params ProtocolName
	 - @params MIMEPageUrl
	 - @params appName

			navigator.registerProtocolHandler("mailto","http://www.somemailclient.com?cmd=%s", "Some Mail Client");

##8.4 screen 对象
表明客户端的能力，其中包括浏览器窗口外部的显示器的信息，如像素宽度和高度等

##8.5 history 对象
每个浏览器窗口、每个标签页乃至每个框架，都有自己的 history 对象与特定的window 对象关联

 -  go()
	 -  @params index/String  跳转参数
		 -  Number:正前负后
		 -  String:跳转到历史记录中包含该字符串的第一个位置——可能后退，也可能前进
 -  back()
 -  forward()
 -  length 
	 -  第一个页面length为0
 -  当页面的 URL 改变时，就会生成一条历史记录，包括hash

