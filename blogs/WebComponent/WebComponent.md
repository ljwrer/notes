#第1章 组件种类和JS分层

##不同层的职责和API

 - 应用层：具体项目需求
 - 框架组件：drag,resize，animate，tabView
 - 框架core：$,bind,addClass
 - 浏览器底层：getElementByID,setIntervel

##通用组件和定制组件

 - 应用工程师
	 - 应用层
	 - 定制组件
 - 框架工程师
	 - 框架通用组件
	 - 框架core
 - 浏览器底层

##独立组件
不依赖于其他框架组件

 - 应用工程师
	 - 应用层
	 - 定制组件
 - 框架工程师
	 - 框架通用组件
		 - 框架core
	 - 独立组件
		 - 独立组件底层
 - 浏览器底层

##组件分类

 - 框架组件
 - 定制组件
 - 独立组件

---

#第2章 如何定义和加载组件
##2.1 组件定义和加载
css和js都有命名冲突

##2.2 CSS命名空间和JS匿名空间
###1.css以组件名开头

	.m-tabview-meun
	.m-tabview-content
 - 不美观

###2.LESS 通过子孙选择器，父类去选择

	.m-tabview menu
- 跨团队问题
- html耦合
- 子孙选择器提高权重，覆盖样式，权重恶意竞争

###3.IIFE
 - 匿名空间隔开公有私有
 - 将组件构造函数暴露在全局

##2.3 组件的依赖关系 
###模块化和requireJS
 - define
	 - 定义模块
	 - 输出字典
 - require
	 - 依赖注入
	 - main方法，程序入口

##2.4 基于require.js重写代码

---
    
#第3章 简单的弹窗组件
##3.1 制作简单的弹窗组件
###原生弹窗

 - alert
 - confirm
 - prompt

问题：

 - 阻塞进程
 - 不同浏览器样式不同
 - 不可定制样式

新版jquery支持require加载

##3.2 带关闭按钮的弹窗组件

 - 长宽硬编码
 - 位置硬编码	

---

#第4章 定制长宽和位置

 - content,handle，cfg
 - 使用对象传参数定制长宽和位置，字典格式
 - $.extend()
 - $().css()

---

#第5章 调整接口格式

 - 参数统一放入字典格式的对象
 - 设置默认值
 - $.extend()合并默认值

---

#第六章 定制标题

 - 默认字典添加title
 - 设置样式

---

#第七章 定制关闭按钮

 - 字典增加hasCloseBtn
 - 字典增加handle4CloseBtn
 - 字典增加handle4AlertBtn

---

#第8章 定制皮肤

 - 添加skinClassName
 - 利用子孙选择器换肤
 - NEC
	 - 公共样式不变
	 - 皮肤样式移除后切换

---

#第九章 定制按钮文案
修改字典即可

---

#第十章 模态
添加遮罩层

---

#第十一章 拖放

 - 引入框架组件draggable()方法
 - draggable（{handle：selector}）

---

#第十二章 自定义事件
##12.1 概述

 - 框架组件层：封装API，抽象成高可读性组件
 - BOX组件
	 - color属性
	 - open()方法
	 - 事件open,close,提升事件层级，不直接监听DOM事件，改为监听组件的自定义事件
 - 定义
	 - 基于原生事件，再封装判断条件
	 - 观察者模式
	 - 无需降到原生事件 

##12.2 自定义事件实现

 - 添加this.handlers属性记录自定义事件注册
 - on注册
 - fire依次触发
 - 内部原生事件调用fire()

---

#第十三章 连缀语法

	内部方法结尾
	return this;

---

#第14章 抽出Widget抽象类

 - utility
	 - UI无关
	 - cookie，ajax，drag，resize
 - widget
	 - UI
	 - tabview,treeview,日历，富文本编辑器，弹出层

<!-- -->

	this.handers
	on()
	fire()

---

#第15章 为Widget类设计统一生命周期

 - 添加容器属性
 - 添加UI相关接口(空方法，子类继承实现)
	 - 添加DOM节点renderUI
	 - 监听事件bindUI
		 - DOM事件
		 - 自定义事件
	 - 初始化组件属性syncUI
	 - 销毁前的处理destructor
 - 添加UI相关方法（调用内部方法）
	 - render渲染组件（可接收外部容器）
	 - destroy销毁组件

---

#第16章 实现confirm方法

 - 修改renderUI
 - 填充bindUI
 - 填充类型参数

---

#第17章 实现prompt方法

 - 添加input

#第18章 实现common方法

 - 修改renderUI
 - 判断windowType

其他：

 - MVC
 - 监听valueChange

