#技术选型#

##模块组织##
 - 内部依赖模块  
 - 内部成员
 - 模块以文件组织 
 
##模块化##
 - 封装实现
 - 暴露接口
 - 声明依赖
	 - 模块系统

###反模式###
	/* math.js */
	function add(a,b){
		return a+b;
	}
	function sub(a,b){
		return a-b;
	}
	
	/* calculator.js */
	var action="add";
	function compute(a,b){
		switch (action){
			case "add":
				return add(a,b);
				break;
			case "sub":
				return sub(a,b);
				break;
			default:
				break;
		}
	}
 - math.js
	 1. 封装型无
	 2. 接口结构不明显
 - calculator.js
	 1. 没有依赖声明
	 2. 使用全局状态  

###对象字面量###
	/* math.js */
	var math={
		add:function(a,b){
			return a+b;
		},
		sub:function(a,b){
			return a-b;
		}
	}
	/* calculator.js */
	var calculator={
		action:"add",
		compute:function(a,b){
			switch(this.action){
				case "add":
					return math.add(a,b);
					break;
				case "sub":
					return math.sub(a,b);
					break;
				default:
					break;
			}
		}
	}
 - math.js
	 1. 结构性好
	 2. 访问没有控制
 - calculator.js
	 1. 没有依赖声明
	 2. 无法标明私有属性


###IIFE###
字值型函数表达式  
创建局部作用域，返回闭包 

	var calculator=(function(){
	var action="add";
	return {
			compute:function(a,b){
				switch(action){
					case "add":
						return math.add(a,b);
						break;
					case "sub":
						return math.sub(a,b);
						break;
					default:
						break;
				}
			}
		}
	})();

 1. 访问控制
 2. 没有依赖声明

###IIFE-review module pattern###
揭露模块模式

	var calculator=(function(m){
		var action="add";
		var compute=function(a,b){
			switch(action){
				case "add":
					return m.add(a,b);
					break;
				case "sub":
					return m.sub(a,b);
					break;
				default:
					break;
			}
		}
		return {
			compute:compute
		};
	})(math);
 1. 增删查改方便
 2. 显式依赖声明
 3. 仍然污染了全局变量
 4. 必须手动进行依赖管理


##命名空间##
	/* math.js */
	namespace("math",[],function(){
		function add(a,b){
			return a+b;
		}
		function sub(a,b){
			return a-b;
		}
		return{
			add:add,
			sub:sub
		}
	})

	/* calculator.js */
	namespace("calculator",["math"],function(m){
		var action="add";
		function compute(a,b){
			return m[action](a,b);
		}
		return{
			compute:compute
		}
	})
 1. 依赖声明
 2. 依赖注入
 3. 没有依赖管理，需手动排序

namespace：

	var namespace=(
		function(){
			//缓存所有模块
			var cache={};
			//真正返回的namespace函数
			function createModule(name,deps,definition){//模块名，依赖列表，定义
				if(arguments.length===1)return cache[name];//参数只有模块名就输出
				//必须先取得所有依赖的模块
				deps=deps.map(function(depName){
					return ns(depName);
				});//需保证前面的模块已经定义
				//初始化模块并返回
				cache[name]=definition.apply(null,deps);
				return cache[name];
			}
			return createModule
		}
	)()

##模块系统##
 - 依赖管理
	 - 加载
	 - 分析
	 - 注入
	 - 初始化
 - 决定模块写法

###Commonjs###

	/* Commonjs */
	function add(a,b){
		return a+b;
	}
	function sub(a,b){
		return a-b;
	}
	exports.add=add;
	exports.sub=sub;

	/* Commonjs */
	var math=require("./math");//依赖声明
	function Calcuator(container){
		this.left=container.querySelector(".j-left");
		this.right=container.querySelector(".j-right");
		this.add=container.querySelector(".j-add");
		this.result=container.querySelector(".j-result");
		this.add.addEventListener("click",this.compute.bind(this));
	}
	Calcuator.prototype.compute=function(){
		this.result.textContent=math.add(+this.left.value,+this.right.value)
	}
	exports.Calcuator=Calcuator;//接口暴露
优点：
 - 依赖管理成熟可靠
 - 社区活跃，规范接受度高
 - 运行时支持，模块定义非常简单
	 - module,exports全局变量
 - 文件级的模块作用域隔离
 - 可以处理循环依赖

缺点：
 - 不是标准组织的规范
 - 同步的require,没有考虑浏览器环境
	 - browserify,webpack，component打包
	 - browserify caculator.js>caculator.js>caculator-bundle.js

###AMD###
定义类似namespace  
拥有依赖管理

	/* AMD */
	define([],function(){
		function add(a,b){
			return a+b;
		}
		function sub(a,b){
			return a-b;
		}
		return{
			add:add,
			sub:sub
		};
	})
	/* AMD */
	define(["math"],function(math){//依赖声明
		var action="add";
		var compute=function(a,b){
			switch(action){
				case "add":
				return math.add(a,b);
				break;
				case "sub":
				return math.sub(a,b);
				break;
				default:
				break;
			}
		};
		return{
			compute:compute//接口管理
		}
	});

####Simplified CommonJS wrapping
函数通过tosting得到函数体，用正在表达式提取出依赖列表
	define(function(require, exports) {
	var math = require("./math");//依赖声明
	function Calcuator(container) {
		this.left = container.querySelector(".j-left");
		this.right = container.querySelector(".j-right");
		this.add = container.querySelector(".j-add");
		this.result = container.querySelector(".j-result");
		this.add.addEventListener("click", this.compute.bind(this));
	}
	Calcuator.prototype.compute = function() {
		this.result.textContent = math.add(+this.left.value, +this.right.value)
	}
	exports.Calcuator = Calcuator;//接口暴露
})

####Loader Plugins(草案)####
完整组件=结构+逻辑+样式
AMD加载其他资源

	define(["regularjs","text!path/tp/foo.html","css!path/to/style.css"],function(Regular,template){
		Component = Regular.extend({
			template:template,//字符串文本
			show:function(){},
			hide:function(){}
		})
		return Component;
	})

####requirejs####
	<script type="text/javascript" src="../js/require.js" defer async="true" data-main = "main.js"></script>	
	/* main.js */
	require.config({
		baseUrl:"../js",
		paths:{
			"calculator":"calculator",
			"math":"math"
		}
	});
	require(["calculator"],function(calculator){
		var a=2,b=5;
		alert(calculator.compute(a,b));
	});

优点：
 - 依赖管理成熟可靠
 - 社区活跃，规范接受度高
 - 转为异步IO环境打造，适合浏览器环境
 - 支持类似Commonjs的书写方式
	 - CMD延迟执行，使用时执行，库级别无法做到条件加载，使用上区别不大
 - 通过插件API可支持加载非js资源
 - 成熟的打包构建工具，并可结合插件

缺点：
 -  模块定义繁琐，需要额外嵌套
 -  只是库级别的支持，需要引入额外库
	 -  如require.js
 -  无法处理循环依赖
 -  无法实现条件加载

###ES6 module(暂未实现)###


	import{ add } from "./math";//import关键字，引入依赖
	class Calcuator(container) {//class关键字
		constructor(container){};
		compute(){
			this.result.textContent=add(+this.left.value, +this.right.value);
		}
	}
	exports.Calcuator = Calcuator;//接口暴露
	/* ES6 module */
	function add(a,b){
		return a+b;
	}
	function sub(a,b){
		return a-b;
	}
	exports{add,sub}//export关键字 接口暴露

优点：
 - 是真正的规范，未来的模块标准
 - 语言级别的关键字支持
 - 适应所有javascript运行时，包括浏览器
 - 同样可以处理循环依赖

缺点
 - 规范并未到稳定级别
 - 基本还没有浏览器支持
 - 鲜有项目使用，即时有大量的6to5transpiler

###Systemjs###
 - 支持加载AMD
 - 支持加载Commonjs
 - 支持加载ES6
 - 支持Transpiler,可支持任意资源

不同模块系统可以相互转换


