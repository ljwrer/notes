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
 - 社区水平层次不齐，容易踩坑  

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
 - 




