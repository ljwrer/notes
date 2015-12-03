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