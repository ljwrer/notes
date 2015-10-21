[http://sass-lang.com/documentation/file.SASS_REFERENCE.html](http://sass-lang.com/documentation/file.SASS_REFERENCE.html)
##变量Variables

$ 定义变量，存储css值

	$font-stack:    Helvetica, sans-serif;
	$primary-color: #333;
	
	body {
	  font: 100% $font-stack;
	  color: $primary-color;
	}

##嵌套Nesting

	nav {
	  ul {
	    margin: 0;
	    padding: 0;
	    list-style: none;
	  }

	  li { display: inline-block; }
	
	  a {
	    display: block;
	    padding: 6px 12px;
	    text-decoration: none;
	  }
	}	

##引用父选择器&

 - 引用多层嵌套的父选择器
 - 最好位于复合选择器开头
 - 可添加后缀

		a {
		  font-weight: bold;
		  text-decoration: none;
		  &:hover { text-decoration: underline; }
		  body.firefox & { font-weight: normal; }
		}

##属性嵌套
	.funky{
		font: 12px/24px "Microsoft Yahei"{
			weight:bold; 
		}
	}

##%占位符（只用于继承的样式）

	#context a%extreme {
	  color: blue;
	  font-weight: bold;
	  font-size: 2em;
	}
	
	.notice {
	  @extend %extreme;
	}

##Comments: /* */ and //

 - /* */保留
 - //不保留
 - /\*!comment\*/压缩后保留

#SassScript
##Variables变量: $

 - 变量作用域符合嵌套结构，支持局部变量
 - ！gloal定义全局变量
		
		$width: 5em !global;
 -  下划线与连字符等价

		$border-width:1px;
		border-width:$border_width;

##Data Types数据类型
###string

 - 支持`"string"`和`string`
 - 在mixin中#{$string}将忽略引号
 - 应用于复杂属性值和class时需要用#{}

###Lists
类似数组，可通过空格，逗号或小括号分隔多个值  
可使用[list函数](http://sass-lang.com/documentation/Sass/Script/Functions.html#list-functions )
###Maps
类似对象
$map: (key1: value1, key2: value2, key3: value3);  
可使用list函数和map函数
###Colors
支持所有css颜色且compressed模式下自动压缩

##Operations
###Number Operations

 - +-*/%
	 - 计算保留单位
	 - 单位一致或可转换可加减
	 - 带单位相乘会出错
 - <, >, <=, >=
 - ！=，==支持所有类型

####/当做除法

 1. 如果数值或它的任意部分是存储在一个变量中或是函数的返回值。
 2. 如果数值被圆括号包围。
 3. 如果数值是另一个数学表达式的一部分。
 4. 用#{}包住变量避免触发除法
	 - 如font: #{$font-size}/#{$line-height}

####减法、负数、-

 1. 做减法时两边留空格 5px - 3px
 2. 做负数或者取反时左边留空格  -$var
 3. 连字符内的取反带括号10px {-$var}

优先权：

 1. 标识符中连字符后不要带数字 5px-3px==5px - 3px
 2. -连接不带空格的数字当做减法 1-2==1 - 2
 3. -在数字直接量之前等价于负号 1 -2==（1，-2）
 4. -数字变量之间等价于减法，忽略空格 1 -$var==1-$var
 5. -位于值等价于负号

##颜色运算Color Operations
RGB分别运算

	color:#010203+#040506;
	01+04,02+05,03+06

 - 支持颜色函数
 - 颜色运算需透明度一致
	 - opacity（）增加
	 - transparentize（）减小 
	 - IE 滤镜ie_hex_str

##字符串运算
###+

 - 结果是否带引号由左边决定
 - 如果两个值彼此相邻，它们会被用空格连接起来
 - #{} 形式的表达式可以被用来在字符串中添加动态值
 - 空值会被视作空字符串

##布尔运算
and, or, and not 

##括号
支持括号
##函数
内置函数
##关键字参数

	p {
	  color: hsl($hue: 0, $saturation: 100%, $lightness: 50%);
	}

##插值Interpolation: #{}
插值被当做css

##&
&可赋值给变量
不存在父选择器时为null

##!default
默认值，为null时转换为默认值

#@ 规则和指令
##@import
所有引入的 SCSS 和 Sass 文件都会被合并并输出一个单一的 CSS 文件。 另外，被导入的文件中所定义的变量或 mixins 都可以在主文件中使用。通过 :load_paths 选项 或者在命令行中使用 --load-path 选项来指定额外的搜索目录。  
在少数几种情况下，它会被编译成 CSS 的 @import 规则：

 - 如果文件的扩展名是 .css。
 - 如果文件名以 http:// 开头。
 - 如果文件名是 url()。
 - 如果 @import 包含了任何媒体查询（media queries）。

		@import "foo.css";
		@import "foo" screen;
		@import "http://foo.com/bar";
		@import url(foo);
将被编译为：

		@import "foo.css";
		@import "foo" screen;
		@import "http://foo.com/bar";
		@import url(foo);

一个 @import 引入多个文件：

	@import "rounded-corners", "text-shadow";

@import可包含参数

	$family: unquote("Droid+Sans");
	@import url("http://fonts.googleapis.com/css?family=#{$family}");
	
编译为
	
	@import url("http://fonts.googleapis.com/css?family=Droid+Sans");

##片段
文件名开头加下划线，不会被编译，通过@import不加下划线引用，
在同一个目录不能同时存在带下划线和不带下划线的同名文件。 例如， _colors.scss 不能与 colors.scss 并存

###Nested @import
嵌套import将导致scss被嵌套

嵌套的上下文中使用@import不允许使用@mixin或者其他指令