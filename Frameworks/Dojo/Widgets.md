#Beyond Dojo
##Base vs. Core
同步模式下直接加载dojo/_base下所有模块

core:

 - feature detection
 - deferreds and promises
 - event handling
 - data store
 - DOM manipulation
 - queries
 - DOM effects
 - window lifecycle management
 - mouse, touch and keyboard events
 - drag and drop
 - dates
 - internationalization

##Dijit: Forms, Layout and UI Goodness

 -  Dijit Themes, Buttons, and Textboxes 
 -  Layout with Dijit 

##DojoX: Dojo Extensions

 - DojoX: Data Grid
	 - dgrid
	 - GridX
 - DojoX: GFX
 - DojoX: Charting
 - DojoX: Mobile
 - dojox/lang/functional
 - dojox/widget | dojox/layout | dojox/form

##Util

 - Dojo Build System
 - Testing

---

#Charting（统计图）
[http://archive.dojotoolkit.org/nightly/dojotoolkit/dojox/charting/tests/](http://archive.dojotoolkit.org/nightly/dojotoolkit/dojox/charting/tests/)

[http://www.tuicool.com/articles/7vQzu2J](http://www.tuicool.com/articles/7vQzu2J)
##dojox/charting
使用dojox/gfx生成图表（矢量图）

##Configuring dojox/charting

	require([
	     // Require the basic 2d chart resource
	    "dojox/charting/Chart",
	    // Require the theme of our choosing
	    "dojox/charting/themes/Claro"]

    dojoConfig = {
        parseOnLoad: true, //enables declarative chart creation
        gfxRenderer: "svg,silverlight,vml" // svg is first priority
    };

##Creating a Basic Chart
[http://archive.dojotoolkit.org/nightly/dojotoolkit/dojox/charting/tests/test_themes.html](http://archive.dojotoolkit.org/nightly/dojotoolkit/dojox/charting/tests/test_themes.html)
###Declaratively
不推荐

	<div
	        data-dojo-type="dojox/charting/widget/Chart"
	        data-dojo-props="theme:dojox.charting.themes.Shrooms" id="viewsChart" style="width: 500px; height: 500px;">
	
	    <!-- Pie Chart: add the plot -->
	    <div class="plot" name="default" type="Pie" radius="200" fontColor="#000" labelOffset="0"></div>
	
	    <!-- pieData is the data source -->
	    <div class="series" name="Last Week&#x27;s Visits" array="chartData"></div>
	</div>
###Programmatically

	require(['dojox/charting/Chart', 'dojox/charting/themes/myTheme', 'dojox/charting/plot2d/Pie', 'dojo/domReady!'], function (Chart, theme, Pie) {
	        'use strict';
	        var data = [
	            {x: 1, y: 1},
	            {x: 2, y: 2},
	            {x: 3, y: 3},
	            {x: 4, y: 4}
	        ];
	        var pieChart = new Chart('chartId',{
				title:<titleName>
				//...
			});
	        pieChart.setTheme(theme);
	        pieChart.addPlot('default', {
				//Plots类型
	            type: Pie,
	            radius: 200,
	            fontColor: 'black',
	            labelOffset: 100
	        });
			//添加数据
	        pieChart.addSeries('haha', data);
	        pieChart.render();
	    });

##Chart Themes

 - dojox/charting/themes/SimpleTheme
 - dojox/charting/themes/Theme

		pieChart.setTheme(theme)

##Chart Components: Plots, Axes, Series
创建任何一个图表都需要定义 Plots, Axes 和 Series 数据，其中， Plots 是定义数据如何被显示， Axes 定义的是数据显示的精度以及一些特殊的标签， Series 是数据本身
###Plots
定义显示类型
dojox/charting/plot2d/<PlotType>

	chart.addPlot('default',{
		type：<PlotType>
		<standard plot options>
		<plot-specific options>
	})

###Axes
定义轴线

	chart.addAxis("x");
	// Ad the Y axis
	chart.addAxis("y",{
	    vertical: true // y is vertical!
		<standard axis options>
	});

###Series
定义数据

	chart.addSeries（<name>,<data array>,<options>）

##Charting Plugins
###Legend
下标
	
	var legend = new Legend({ chart: chart }, <id>/<node>)         

###Tooltip
hover显示数值，在render方法之前调用
	
	var tip = new Tooltip(chart, "default");

###MoveSlice and Magnify
hover动画，render之前调用

	var slice = new MoveSlice(pieChart, "default");
	var magnify = new Magnify(chart, "default");});

###Highlight
hover高亮

	var highlight = new Highlight(chart, "default");

---

#Advanced Charting
##Dojo Stores and Charting
###StoreSeries

	var store = new ObservableStore(new MemoryStore({
        data: {
            identifier: "id",
            label: "Users Online",
            items: data
        }
    }));
	chart.addSeries("y", new StoreSeries(store, { query: { site: 1 } }, "value"));
	store.notify({ value:, id: , site: });

###dojox/charting Events

	chart.connectToPlot("default", function(evt) {});

---

#Checkboxes
dijit/form/CheckBox  dijit/form/RadioButton  
通过模拟保留原生表单的方法并增强

	<input type="checkbox" id="dbox1" checked data-dojo-type="dijit/form/CheckBox">
	require(["dijit/form/CheckBox"], function(CheckBox) {
	    var box1 = new CheckBox({
	        id: "pbox1",
	        checked: true
	    });
	    box1.placeAt("pbox1_container", "first");
	});

###dijit/registry
registry查找widget对象

##Checkbox Values
当checkbox value值(set)设为非空非null时会将checked改为true


##Radio Buttons
dijit/form/RadioButton
可用name属性控制单一选择

##Events
##其他

 - dijit/Menu
 - dijit/CheckedMenuItem
 - dijit/form/Select
 - FilteringSelect, ComboBox and MultiSelect
 - dojox/form
 - dijit/form/_FormWidget

---

#Dialogs & Tooltips
dijit/Tooltip, dijit/Dialog, and dijit/TooltipDialog
## Tooltips

	<button id="TooltipButton" onmouseover="dijit.Tooltip.defaultPosition=['above', 'below']">Tooltip Above</button>
	<div class="dijitHidden"><span data-dojo-type="dijit/Tooltip" data-dojo-props="connectId:'TooltipButton'">I am the button</span></div>

	var tip = new Tooltip({
	    // Label - the HTML or text to be placed within the Tooltip
	    label: '&lt;div class="myTipType"&gt;This is the content of my Tooltip!&lt;/div&gt;',
	    // Delay before showing the Tooltip (in milliseconds)
	    showDelay: 250,
	    // The nodes to attach the Tooltip to
	    // Can be an array of strings or domNodes
	    connectId: ["myElement1","myElement2"]
	});

修改Tooltip.defaultPosition数组可修改全局tooltip位置

##Dialogs
	
	var myDialog = new Dialog({
	    // The dialog's title
	    title: "The Dojo Toolkit",
	    // The dialog's content
	    content: "This is the dialog content.",
	    // Hard-code the dialog width
	    style: "width:200px;"
	}).show();
	<div class="dijitHidden">
	    <!-- dialog that gets its content via ajax, uses loading message -->
	    <div data-dojo-type="dijit/Dialog" style="width:600px;" data-dojo-props="title:'Ajax Dialog',href:'dialog-ajax-content.html',loadingMessage:'Loading dialog content...'" id="ajaxDialog"></div>
	</div>

##TooltipDialog
###dijit/form/DropDownButton
	
---

#Layout with Dijit
##BorderContainer
"dijit/layout/BorderContainer"-->"dijit/layout/ContentPane"，可相互嵌套，类似swing
##Making the Tabs
dijit/layout/TabContainer-->"dijit/layout/ContentPane"
##StackContainer and Friends
TabContainer继承StackContainer  
StackContainer继承_LayoutWidget

使用StackContainer与StackController定制tab

dojox/layout/ExpandoPane可替换ContentPane

##Startup and Resize

 - startup 渲染Dom 向下传播
 - resize 重绘组件 向下传播
 -  data-dojo-props
 -  addChild

---

#Dijit Editor
dijit/Editor  dijit/_editor/plugins

extraPlugins 需要引入额外的css

---
#Form Management with dojox/form/Manager
##Setting up the Manager
使用声明式语法

	<form data-dojo-type="dojox/form/Manager" id="myForm">
###Adding in observer hooks to your form elements

 - data-dojo-observer绑定回掉函数
 - intermediateChanges 绑定change事件
 - dojox的验证与dom验证不兼容

###Setting up your observer methods
在表单内插入`<script>`

 - dojo/method dojo/on dojo/aspect
 - data-dojo-event
 - data-dojo-args
 - data-dojo-advise

###Events on your form
onReset/onSubmit

---

#Vector Graphics with Dojo's GFX
##Creating an Image using dojox/gfx

1. Create the surface (or "canvas")
2. Create the shapes (paths, lines, rectangles, text, etc.)
3. Create groups of shapes (grouping shapes together)
4. Animate shapes or groups of shapes (transform, scale, etc.)
5. Add shape events

##Creating the Surface

	gfx.createSurface("surfaceElement", 400, 400);

##Creating Shapes

	gfx.create_ShapeName_(properties)

 - applyTransform: Allows for transforming of a shape (scaling and skewing, for example)
 - getFill/setFill: Get and set fill colors
 - getStroke/setStroke: Get and set stroke colors
 - moveToBack/moveToFront: Moves shapes based on "z-indexing"(与css有区别)

##Styling Shapes

 - Filling a Shape
	 - setFill
 - Setting a Stroke on a Shape
	 - setStroke
 - Choosing a Font
	 - setFont

##Grouping Shapes Together
多个shape组成group可当作一个shape处理

 - createGroup()
 - add（`<shape>`）
 - new Moveable(group)

##Animations and Transformations
dojox/gfx/fx

 - animateFill
 - animateFont
 - animateStroke

##Rotating a Shape

 - animateTransform
	 - rotateAt
	 - rotategAt

##Scaling and Skewing

 - applyTransform 

##event

 - shape.on
 - group.on

---

#A Loading Overlay

---

#Dijit Menus
dijit/Menu-->dijit/MeunItem

##Menu Structure
dec:PopupMenuItem 当作 MenuItem 嵌套，取第一个几点的innerText作为label名

##Menu Icons
iconClass:class  内部是table布局

##Menu Variations

	//为windows添加右键菜单
	contextMenuForWindow：true
	//为节点添加右键菜单
	targetNodeIds:id/node/Array
	//使用选择器，即使菜单渲染时没有生成的节点也会有效 
	selector:selector
	//便于popup绑定	
	id

>registry.byNode  
>registry.getEnclosingWidget

##MenuBars and More
dijit/MenuBar(水平的menu)  dijit/MenuBarItem

##Menus in Widgets
dijit/Menu嵌套在dijit/form/ComboButton和dijit/form/DropDownButton内形成下拉菜单.

##Event

 - onItemHover
 - onItemUnHover
 - onItemClick
 - onOpen
 - onClose

---

#Selective with Dijit

 - dijit/form/Select
	 - 可定制样式(span模拟)
 - dijit/form/FilteringSelect
	 - 带输入框，可筛选
 - dijit/form/ComboBox

##dijit/form/Select

	<select data-dojo-type="dijit/form/Select"></select>

 - displayedValue: The value presently displayed in the field（innerText）
 - value:(prop(value))
 - onChange

##dijit/form/FilteringSelect

 - required
 - placeHolder
 - displayedValue
 - value

##dijit/form/ComboBox！
组合select与textbox
接收input事件

 - required
 - placeHolder
 - value

displayedValue=value（input标签模拟）

---

#Dijit Selects using Stores！
##Select Widgets and dojo/store

	new ComboBox({
        name: "stateSelect",
        store: stateStore,
		placeHolder: "Select a State",
        labelAttr: "name",
		//防止下拉菜单导致整个页面变大
        maxHeight: -1, // tells _HasDropDown to fit menu within viewport
    }, <id>);

##Using Stores with FilteringSelect and ComboBox

 - searchAttr(搜索匹配属性，默认"name"，与store内对应)
 - pageSize(显示数)

##Using Stores with dijit/form/Select
identities 只支持string

	select.set("value", id)
	
	//widget.set("store", newStore) startUp后无效 
	widget.setStore(newStore)

##Creating dijit/form/Select without a Store

options:

 - label
 - value
 - selected
 - disabled


 - addOption
 - removeOption
 - set("options", arrayOfObjects)
 - startup （更新后需要重复调用）

---

#Sliders with Dijit
 dijit/form/HorizontalSlider dijit/form/VerticalSlider

 - properties
	 - clickSelect
	 - disabled
	 - discreteValues
	 - intermediateChanges
	 - maximum
	 - minimum
	 - name
	 - pageIncrement
	 - showButtons
	 - value
 - methods
	 - decrement
	 - increment
	 - get
	 - set
 - events
	 - onChange

##Adding Rules and Rule Labels
dijit/form/HorizontalRule, dijit/form/HorizontalRuleLabels, dijit/form/VerticalRule,dijit/form/VerticalRuleLabels

---

#Understanding _WidgetBase
##Dijit Lifecycle

 - constructor (common to all prototypes, called when instantiated)
 - postscript (common to all prototypes built using declare)
	 - create
		 - postMixInProperties
		 - buildRendering
		 - postCreate
 - startup
	 - resize

<!-- -->
 1. postscript
 2. create
 3. postMixInProperties
 4. buildRendering
 5. postCreate
 6. startup

###postCreate()
属性定义后，插入document前执行

###startup()
所有dom插入document后执行

##Tear-down methods
一般直接调用destroyRecursive即可

 - destroyRecursive
	 - destroyDescendants
	 - destroy
		 - uninitialize
		 - destroyRendering

##Node references
domNode属性，在postCreate执行后生成，包含整个组件的节点

containerNode：may contain content or widgets defined outside of your widget definition

##Getters and Setters

	// for the field "foo" in your widget:
	
	// custom getter
	_getFooAttr: function(){ /* do something and return a value */ },
	
	//	custom setter
	_setFooAttr: function(value){ /* do something to set a value */ }
		
	// get the value of "foo":
	var value = myWidget.get("foo");
	
	// set the value of "foo":
	myWidget.set("foo", someValue);

##Owning handles

	this.own(
	    on(someDomNode, "click", lang.hitch(this, "myOnClickHandler)"),
	    aspect.after(someObject, "someFunc", lang.hitch(this, "mySomeFuncHandler)"),
	    topic.subscribe("/some/topic", function(){ ... }),
	    ...
	);

##Pre-defined Properties and Events

 - id: a unique string identifying the widget
 - lang: a rarely-used string that can override the default Dojo locale
 - dir: useful for bi-directional support
 - class: the HTML class attribute for the widget's domNode
 - style: the HTML style attribute for the widget's domNode
 - title: most commonly, the HTML title attribute for native tooltips
 - baseClass: the root CSS class of the widget
 - srcNodeRef: the original DOM node that existed before it was "widgetified", if one was provided. Note that depending on the type of widget (e.g. templated widgets), this may be unset following postCreate, as the original node is discarded.

##usage

	dojo.declare("MyWidget", dijit._WidgetBase, { ... });
	this.inherited(arguments)

---

#Creating Template-based Widgets
_TemplatedMixin and _WidgetsInTemplateMixin

###properties

 - templateString

###Overridden Methods

 - buildRendering
 - destroyRendering

##Using _TemplatedMixin
创建单独的文件夹保存模板

	define([
	    "dojo/_base/declare",
	    "dijit/_WidgetBase",
	    "dijit/_TemplatedMixin",
	    "dojo/text!./templates/SomeWidget.html"
	], function(declare, _WidgetBase, _TemplatedMixin, template) {
	
	    return declare([_WidgetBase, _TemplatedMixin], {
	        templateString: template
	    });
	
	});

##Writing Templates
只允许存在单个根节点
	<div class="${baseClass}">
	    <div class="${baseClass}Title" data-dojo-attach-point="titleNode"
	            data-dojo-attach-event="onclick:_onClick"></div>
	</div>

 - variable substitution
 - attach points
 - event attachments

###Variable Substitution
建议模板中的引用值为static，如需修改，在postCreate内调用set

	${property}
	${propertyObject.property}
	//强制避免转义
	${!property}

###Attach Points
加入widget属性,绑定对应节点，domNode默认为模板根节点

	data-dojo-attach-point

#### containerNode Attach Point
	//组件模板
	<div class="${baseClass}">
	    <div class="${baseClass}Title" data-dojo-attach-point="titleNode"
	            data-dojo-attach-event="ondijitclick:_onClick"></div>
	    <!-- And our container: -->
	    <div class="${baseClass}Container"
	            data-dojo-attach-point="containerNode"></div>
	</div>
	//组件声明式调用
	<div data-dojo-type="demo/SomeWidget"
	        data-dojo-props="title: 'Our Some Widget'">
	    <p>This is arbitrary content!</p>
	    <p>More arbitrary content!</p>
	</div>
	//被组件包裹的节点(包括其他组件)都成为了containerNode的子节点，组件会自动添加id
	<div id="demo_SomeWidget_0" class="someWidgetBase">
	    <div class="someWidgetTitle">Our Some Widget</div>
	    <div class="someWidgetContainer">
	        <p>This is arbitrary content!</p>
	        <p>More arbitrary content!</p>
	    </div>
	</div>

###Event Attachments

 - 事件处理函数的参数通常和DOM事件一致
 - dijit/_OnDijitClickMixin替换click

	data-dojo-attach-event="onmouseenter:_onHover,onmouseleave:_onUnhover,ondijitclick:_onClick"

###The _WidgetsInTemplateMixin Mixin
在模板中插入其他widget

	<div class="${baseClass}" data-dojo-attach-point="focusNode"
	        data-dojo-attach-event="ondijitclick:_onClick"
	        role="menuitem" tabIndex="-1">
		//通过myWidget.buttonWidget访问dijit/form/Button
		//即data-dojo-attach-point属于内部其他组件时，可访问内部其他组件，而不是dom节点
	    <div data-dojo-type="dijit/form/Button"
	        data-dojo-attach-point="buttonWidget">
	        My Button
	    </div>
	    <span data-dojo-attach-point="containerNode"></span>
	</div>

 - 需要手动注入子组件
 - 影响性能

---

#Dijit Themes, Buttons, and Textboxes
##Using a Dijit theme
1. 导入css
2. body设置对应class
3. 块级元素设置class只对块级元素内生效
4. 弹出层必须设置body class

##Buttons in Dijit

 - iconClass: indicates what CSS class to use (to apply an image sprite)
 - showLabel: determines whether to show any text in the button
 - title: sets the value of the HTML title attribute on the rendered DOM node of the widget
 - label: in programmatic usage, this indicates the content of the button label;declaratively, this is specified via the content (innerHTML) of the element representing the widget
 - type:在form内或dijit/form/Form内可设为reset/submit
 - 其他
	 - dijit/form/ToggleButton
	 - dijit/form/DropDownButton
	 - dijit/form/ComboButton

##The Dijit TextBox Family

 - dijit/form/TextBox
 - dijit/form/ValidationTextBox
 - dijit/form/NumberTextBox
 - dijit/form/DateTextBox
 - dijit/form/TimeTextBox
 - dijit/form/CurrencyTextBox
 - dijit/form/NumberSpinner
 - dijit/form/Textarea

##HTML5 Multi-File Uploader

 - 同样需要enctype="multipart/form-data"
 - dojox/form/uploader/FileList 显示文件列表

###Plugins

	require(["dojox/form/uploader/plugins/Flash"], ...
	require(["dojox/form/uploader/plugins/IFrame"], ...
	require(["dojox/form/uploader/plugins/HTML5"], ...

---

#Forms and Validation
dojox/validate

	require(["dojox/validate"], function(validate) {});
	validate.isInRange(test, options);
	validate.isNumberFormat(test, options);
	validate.isText(test, options);
	validate.isValidLuhn(test);

	//选项
	var test = validate.isNumberFormat(someNum, { format: "(###) ###-####" });
	var test = validate.isNumberFormat(someNum, {
	    format: ["### ##", "###-##", "## ###"]
	});
	//插件
	require(["dojox/validate/web"], function(validate) {
	    validate.isEmailAddress(someAddress);
	});

##dojox/validate with HTML-based forms
调用一些内置函数，几乎不可用
	validate.check（form,profile）

	// Since dojox/validate/check and dojox/validate/web just extend
	// dojox/validate with new methods we don't need references to them
	require([
	    "dojox/validate",
	    "dojox/validate/check",
	    "dojox/validate/web"
	], function(validate) {
	    var profile = {
	        trim: [ "field1", "field2" ],
	        required: [ "field1", "pwd", "pwd2" ],
	        constraints: {
	            field1: myValidationFunction,
	            field2: [validate.isEmailAddress, false, true]
	        },
	        confirm: {
	            pwd2: "pwd"
	        }
	    }
	
	    //    later on in the app, probably onsubmit on the form:
	    var results = validate.check(document.getElementById, profile);
	});
	
 - isSuccessful()
 - hasMissing()
 - getMissing()
 - isMissing(field)
 - hasInvalid()
 - getInvalid()
 - isInvalid(field)

##dojox/validate with Dijit-based Forms

    data-dojo-type="dijit/form/ValidationTextBox"
    data-dojo-props="validator:dojox.validate.isInRange,
        constraints:{ min:10, max:100 },
        invalidMessage:'This is not within the range of 10 to 100!'"

---

#Creating a custom widget

 - app
	 - data
	 - widget
		 - css
		 - images
		 - templates
##Setup
data.json:

	[
	    {
	        "name": "Brian Arnold",
	        "avatar": "/includes/authors/brian_arnold/avatar.jpg",
	        "bio": "Brian Arnold is a software engineer at SitePen, Inc., ..."
	    },
	    /* More authors here... */
	]

