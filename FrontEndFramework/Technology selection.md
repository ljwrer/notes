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


 


