#框架选型#
##解决方案##
###库 Lib###
 - 针对特定问题的解答
 - 不控制应用程序
 - 被动的被调用

###框架###
 - inverse of control
 - 决定应用程序生命周期
 - 一般会集成大量的库

###解决方案###

 - DOM
 - Communication
 - Utility
	 - 工具库
 - Templating
 - Component
	 - 组件
 - Routing
	 - 路由
	 - 单页系统
 - Architecture

###why？###
 - 开发效率
 - 可靠性：浏览器兼容、测试覆盖
 - 更好的配套：文档/DEMO/工具
 - 设计的更好
 - 专业性

###why not？###
 - 问题过于简单
 - 备选框架质量和可控性无法保证
 - 无法满足当前业务需求
 - 团队中已有相关积累

###how###
 - 开发：基于一个外部模块系统，自由组合
 - 半开放：基于一个定制过的模块系统，内部-外部的解决方案共存
 - 大教堂：深度定制的模块系统，很少需要引入外部模块

##DOM##
 - selector
	 - 当前浏览器内置
	 - 非必需
 - Manipulation
	 - DOM操作
 - Event(dom）
	 - 简化事件绑定
	 - 事件代理
 - Animation

###职责###
 - 提供便利的DOM查询/操作/移动等操作
 - 提供事件绑定/时间代理等支持
 - 提供浏览器特性检测，UA侦测
 - 提供节点属性、样式、类名等操作
 - 所有以上操作实现目标平台的跨浏览器支持

###jQuery###
链式操作,非原生DOM节点，包装的jquery对象
 - IE6+
 - 94K
 - 最稳妥的方案

优点：

 - 社区强大，普及率高
 - 包装对象，不污染原生
 - 基本上专注于Dom

缺点：

 - 包装对象，容易混淆
 - 接口两义性
 - 社区水平参差不齐，容易踩坑  

###zepto.js###
部分兼容jquery接口，轻量级

 - 25K
 - IE10+
 - 移动端备选

优点：

 - 小，启动快
 - 接口与jquery兼容
 - 提供了简单的手势

缺点

 - 与jquery不能做到100%对应
 - 支持浏览器少，功能较弱

###mootools###
接口和内部实现优秀，严格遵守Command-Query，get和command严格分开，原生DOM对象，直接扩展了Dom原生对象，Element.prototype

 - IE6+
 - 96K
 - 最好的源码阅读学习资源

优点：

 - 概念清晰，没有包装对象
 - 接口设计优秀
 - 源码清晰易懂
	 - 适合新手学习
 - 不局限于Dom和Ajax
	 - String,Function等

缺点：

 - 扩展原生对象（致命）
 - 社区衰弱

###DOM（专业领域）###
####手势--Hammer.js####
常见手势封装，包括tap,hold,transform,swipe等等，并支持自定义扩展，接口简单
####局部滚动--iscroll.js####
移动端position:fix+overflow:scroll的救星，随着浏览器支持不再需要
####高级动画--Velocity.js####
复杂动画序列实现，不局限于DOM
####视频播放--video.js####
类似元素video标签的使用方式，对低级浏览器回退到flash播放

##Communication##
通信库

实时性：websocket

 - XmlHttpRequest
 - Form
 - JSONP
 - Socket

###职责###
 - 处理与服务器的请求与响应
 - 预处理请求数据/响应数据Error/Success的判断封装
 - 多种类型iqu，统一接口（XmlHttpRequest1/2，JSONP，Iframe）
 - 多种浏览器兼容性

类型，Url  
接口和回掉绑定  
前面三个框架已经支持
###Reqwest###
 - 支持JSONP方式
 - 返回数据已经自动处理成JSON对象

优点：

 - JSONP支持
 - 稳定/IE6+ support
 - CORS跨域
 - Promise/A支持
	 - request返回Promise对象，可以在then（）里做回掉处理

###qwest###
 - 更小的代码量
 - XmlHttpRequest2
 - CORS跨域
 - 支持高级数据类型如ArrayBuffer，Blob和FormData

###实时性要求极高（IM)###
可使用握手的websocket
####socket.io####
 - 实时性
 - 支持二进制数据流
 - 智能自动的回退自持（非二进制数据流）
 - 多种后端语言支持
	 - JAVA bug
	 - NodeJs

##Utility（Lang)##
函数工具包

 - 函数增强
 - shim/Flow Control

职责：

 - 提供JS原生不支持的功能
 - 方便门面包装，使其更易于使用
 - 异步队列/流程控制等等

###Extension(underscore)###
扩展，包装，如_filter

 - underscore
	 - 兼容IE6+
 - Lodash
 	- underscore高性能版本
 	- 方法大部分都是runtime编译出来的
 	- 兼容IE6+

###Shim###
如arrry.filter

 - 库级别的支持
 - 可达到内建函数级别的支持
 - 修改原型方法
 - 需保证实现与规范一致
 - 无法达到语言级别的支持，需预编译

---
 - es5-shim
 - es6-shim

##Templating##

###String-based###
 - 后续修改数据展现不再变化
 - data与DOM不直接相关，直接修改data不会影响到DOM
 - 除非重新render一遍，这样会导致性能低下，而且原来的DOM及绑定时间全部消失

![](http://7xkcnd.com1.z0.glb.clouddn.com/String-based.png)

	//模板
	<h3>{title}</h3>
	<ul>
	{#list users as user}
	<li>{user}</li>
	{/list}
	</ul>
	//数据
	{
		title:"hello",
		users:["Ray","Rmaiy","Rmoony"]
	}
	//展现
	<h3>hello</h3>
	<ul>
		<li>Ray</li>
		<li>Rmaiy</li>
		<li>Rmoony</li>
	</ul>

###Dom-based###
 - 遍历的语法写在属性里
 - 展现随数据变化
 - 展现为局部更新
 - 遍历DOM树进行抓取和绑定
 - model与生成的DOM直接绑定
 - 语法必须写在节点上，以便html正确识别

![](http://7xkcnd.com1.z0.glb.clouddn.com/Dom-based.png)

	<h3>{title}</h3>
	<ul>
		<li repeat='user in users'{user}</li>
	</ul>
	
	{
		title:"hello",
		users:["Ray","Rmaiy","Rmoony"]
	}
		
	<h3>hello</h3>
	<ul>
		<li>Ray</li>
		<li>Rmaiy</li>
		<li>Rmoony</li>
	</ul>

###Living Template###
 - 展现随数据变化
 - 拼接上两种技术
 - 循环遍历DOM节点
 - 通过createElemnt和setAttribute生成DOM

![](http://7xkcnd.com1.z0.glb.clouddn.com/Living%20Template.png)

	<h3>{title}</h3>
	<ul>
		{#list users as user}
		<li>{user}</li>
		{/list}
	</ul>
	
	{
		title:"hello",
		users:["Ray","Rmaiy","Rmoony"]
	}
		
	<h3>hello</h3>
	<ul>
		<li>Ray</li>
		<li>Rmaiy</li>
		<li>Rmoony</li>
	</ul>
###小结###

 - String-based
	 - 初始化时间短
	 - 不支持动态更新
	 - DOM无关，可以在服务器端运行，
	 - 语法简单，模板不一定输出html
	 - 学习成本低 
	 - 不支持SVG
	 - 安全性低，因为最终使用innerHTML
	 - 解决方案
		 - dustjs（linkin接管）
		 - hogan(mustache实现之一）
		 - dot.js(速度快，小型)
 - Dom-based
	 - 初始化长
	 - 支持动态更新
	 - DOM相关
	 - 语法需被html识别
	 - 学习成本高
	 - 支持SVG(需处理namespace)
	 - 安全性低，因为最终使用innerHTML
	 - 解决方案
		 - Angularjs
		 - Vuejs
		 - Knockout

 - Living Template
	 - 支持动态更新
	 - Parse DOM无关
	 - 语法需被html识别
	 - 可利用String-based概念
	 - 支持SVG
	 - 高安全性，完全不实用innerHTML
	 - 解决方案
		 - Regularjs
		 - Ractivejs
		 - htmlbar

##Component组件##

 - Modal
 - Slider
 - DataPicker
 - Tabs
 - Editor

职责:
 - 提供基础组件css支持
 - 提供常用组件如Slider,Modal
 - 提供声明式的调用方法（Optional)

设计风格差异
社区组件极多
###Bootstrap###

 - 最新版本3.x
 - Mobile First的流式栅格（从简设计）
 - LESS/SASS组织，可定制UI
 - IE8+(受限的)
 - 超过15个javascript组件（jQuery）
 - MIT

###Foundation###

 - 最新版本5.x
 - Mobile First的流式栅格（从简设计）
 - 基于SASS组织，可定制UI
 - IE9+(受限的)
 - 超过20个javascript组件（jQuery）
 - MIT

其他非jQuery支持:

 - Knockout
 - Angularjs
 - React

##Routing##

 - Client Side
 - Server Side

职责:

 - 监听url变化，并通知注册的模块
 - 通过javascript进行主动跳转
 - 历史管理
 - 对目标浏览器的兼容性的支持

---
 - 匹配url时通知模块
 - 多级路由
 - 不对全局进行切换

库:

 - page.js
 	- 兼容IE8+
 	- 类似Express.Router的路由规则的前端路由库
 - Director.js 
	- 兼容IE6+
	- 可以前后端使用一套规则来定义路由
	- 后端实用性不高，前端需处理大量中间状态
 - Stateman
	 - 兼容IE6+
	 - 用于处理深层复杂路由的独立路由库
 - crossroad.js
	 - 老牌Routing库，API定义较为繁琐
	 - 可用于学习路由实现

##Architecture架构##
目的：解耦

 - MVC
 - MVVM
 - MV*

职责：

 - 提供一种范式帮助（强制）开发者进行模块解耦
 - 视图与模型分离
 - 更容易进行单元测试
 - 更容易实现应用程序扩展

###MVVM###
 - Model
	 - 数据实体，比如Car,Person等等，
	 - 它们用于记录应用程序的数据
 - View
	 - 展示友好的界面，它是数据的定制反映，它包括央视结构定义以及与VM享有声明式数据、事件绑定
 - ViewModel
	 - View与Model的粘合剂，它通过绑定、事件与View交互，并可以调用Sevice处理数据持久化，当然也能通过数据绑定将Model的变动更新到View中

只需注意ViewModel以及与其他组件的绑定
UnitTest通过测试ViewModel测试解耦的View

action-->View(触发事件)-->VM（响应action,action可能是通过数据绑定的形式）-->Model(修改数据)-->VM-->View更新  
页面为局部更新

###引入Routing的MVVM

 - MV*!==SPA(单页系统)
 - 但MV*适合处理单页系统
 - Routing是MV*系统的可定位状态的信息来源
	 - 即需要通过url可以定位的状态
	 - 非临时定位

action-->View(触发事件)-->VM(通知路由改变url)-->路由模块（通知VM,url状态变化，可能通过事件绑定的方式）-->VM（通过事件绑定回调Model数据）-->Model(修改数据)-->VM-->View更新

> **单页系统的普适法则：·可定位的·应用程序状态都应该通过统一的路由系统进入，以避免·网状·的信息流**

url变化都会通过Routing线性通知VM,无需关心url来源，应用逻辑简单

url变化：

 - 地址栏输入
 - 回退
 - 前进
 - JS主动跳转
 - 链接跳转

###网站###
[http://todomvc.com/](http://todomvc.com/)

##推荐参考网站##
[https://www.javascripting.com/](https://www.javascripting.com/)
 - 排名系统优秀

[http://www.javascriptoo.com/](http://www.javascriptoo.com/)
 - 遗留很多不再维护的框架
 - 类目广泛

[http://microjs.com/](http://microjs.com/)
 - 页面简单
 - 开源项目
 - 所有库小于10kb 