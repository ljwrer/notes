#布局解决方案#
---
##居中布局##
###水平居中###
宽度不定，父容器宽度不定
####inline-block+text-align####
块级元素默认宽度撑满父元素  
inline-block宽度为内容宽度  
text align:对子元素inline元素有效  

	.parent{
		text-align: center;
	}
	.child{
		text-align: left;
		display: inline-block;
	}
优点：

- 兼容性好  
- display:inline+zoom：1在IE6,7模拟inline-block   

缺点：

- child会继承text-align属性，需要单独调整

####table+margin####
display:table类似display:block,但宽度为内容宽度  
利用类似block定宽+margin:auto达到居中  

	.child{
		display: table;
		margin: 0 auto;
	}
	
优点：

 - 只需设置child元素
 - display:table IE8以上支持
 - 将结构换成table兼容IE6,7

<!-- -->

	<div class="parent">
		<table class="child">
			<tr><td>demo</td></tr>
		</table>
	</div>
####absolute+transform####
设置父元素为参照物  
子元素绝对定位元素默认宽度为内容宽度  
left:50%,参照物为父元素  
transform：translateX(-50%)参照物为元素本身

优点：

 - 子元素脱离文档流，不会对其他元素造成影响

缺点：

 - css3属性，IE6,7,8不兼容
 - 高版本浏览器需要私有属性

<!-- -->

 	.parent{
		position: relative;
	}
	.child{
		position: absolute;
		left: 50%;
		transform: translateX(-50%);				
	}
####flex+justify-content####
flex子元素为flex-item  
flex-item默认宽度为auto，浏览器计算实际宽度，一般为内容宽度
使用justify-content或者margin居中

	.parent{
		display: flex;
		justify-content:center;					
	}
或

	.parent{
		display: flex;
	}
	.child{
		*margin:  0 auto;*
	}
优点：  

 - 只需设置parent

缺点：
 
 - IE6,7,8不兼容 

---
###垂直居中###
父元素高度不定  
子元素高度不定
####table-cell+vertical-align####
vertical-align对inline,inline-block,table-cell有效

	.parent{
		display: table-cell;
		vertical-align: middle;
	}
优点：

 - 兼容性好，IE8以上
 - 只需设置父元素
 - 改成表格结构兼容IE6,7

<!-- -->
		<table>
			<tr><td class="parent">
				<div class="child">demo</div>
			</td></tr>
		</table>
####absolute+transform####
类似水平居中

	.parent{
		position: relative;
	}
	.child{
		position: absolute;
		top: 50%;
		transform: translateY(-50%);
	}
####flex+align-items####
flex-item默认align-items为stretch,撑满剩余空间，修改为center  
类似水平布局

	.parent{
		display: flex;
		align-items: center;
	}

---
###水平居中+垂直居中###
####inline-block+text-align+table-cell+vertical-align####
综合两种类inline元素居中

	.parent{
		text-align: center;
		display: table-cell;
		vertical-align: middle;
	}
	.child{
		display: inline-block;
	}  
优点：

 - 兼容性好
 - 可修改结构兼容IE6,7

<!-- -->
	.parent{
		height: 500px;
		width: 500px;
		background: deepskyblue;
		text-align: center;
		display: table-cell;
		vertical-align: middle;
	}
	.child{
		background: brown;
		display: inline-block;
	}

	<table>
		<tr><td class="parent">
				<div class="child">demo</div>
		</td></tr>
	</table>
####absolute+transform####
	.parent{
		position: relative;
	}
	.child{
		position: absolute;
		left: 50%;
		top:50%;
		transform: translate(-50%,-50%);
优点：

 - 脱离文档流，不对其他元素产生影响

缺点：

 - css3兼容性问题

####flex+justify-content+align-items####

	.parent{
		display: flex;
		justify-content: center;
		align-items: center;
	}
优点：

 - 只设置parent

缺点：

 - css3兼容性

###小结###
---
 - 掌握css属性和值的特性
 - 问题分解

---
##多列布局##
###定宽+自适应###
####float+margin####

	.left{
		width: 100px;
		float: left;
	}
	.right{
		margin-left: 110px;
	}
优点：

 - 容易理解 

缺点：

 - right清除浮动会掉下去
 - IE6 3px缩进bug
<!-- -->

	#hack：
	.left{
		margin-right:-100px;
	}
####float+margin+(fix)####
1. magrin-left为负，设置空白
2. right-fix float消除clear：both bug
3. position:relative提高层级 

<!-- -->

	.left{
		width: 100px;
		float: left;
		position: relative;
	}
	.right-fix{
		width: 100%;
		float: right;
		margin-left: -100px;
	}
	.right{
		margin-left: 110px;
	}
	<div class="left">
		<p>left</p>
	</div>
	<div class="right-fix">
		<div class="right">
			<p>right</p>
			<p>right</p>
		</div>				
	</div>


优点：

 - 兼容性极佳 

缺点：

 - 结构复杂
 - 样式复杂

####float+overflow####
overflow触发BFC模式，块级格式化文本
BFC容器内容与外界隔离

	.left{
		width: 100px;
		float: left;
		margin-right: 10px;
	}
	.right{
		overflow: hidden;
	}
优点：

 - 样式简单

缺点

 - IE6不兼容

####table####
1. table默认宽度为内容宽度，改为100%撑满父元素
2. table-cell水平排列
3. table每一列宽度之和等于整个宽度
4. table-layout：fixed加速table渲染，设为布局优先,即宽度由布局决定
5. table-cell不能设置margin

<!-- -->

	.parent{
		display: table;
		width: 100%;
		table-layout: fixed;
	}
	.left,.right{
		display: table-cell;
	}
	.left{
		width: 100px;
		padding-right: 20px;
	}
缺点：

 - 代码多

####flex####
flex：1=flex:1 1 0

	.parent{
		display: flex;
	}
	.left{
		width: 100px;
		margin-right: 10px;
	}
	.right{
		flex: 1;
	}
优点

 - 代码简单

缺点

 - 兼容性
 - 性能问题，适用于小范围布局

---
###两列定宽+自适应###
类似定宽+自适应，中间元素按照定宽元素设置即可
	
	.left,.center{
		width: 100px;
		float: left;
		margin-right: 10px;
	}
	.right{
		overflow: hidden;
	}

---
###不定宽+自适应###
left,right不耦合即可

- float+overflow 
 - IE6兼容性
 - 应用范围广
 - 内容由宽度决定:不设置宽度即可

<!-- -->

	.left{
		float: left;
		margin-right: 10px;
	}
	.right{
		overflow: hidden;
	}
	.left p{
		width: 100px;
	}

- table
 - IE6,7不支持
 - 内容宽度决定
   		- table-layout去掉,宽度不再由布局决定
   		- left宽度为0.1%

<!-- -->

	.parent{
		display: table;
		width: 100%;
	}
	.left,.right{
		display: table-cell;
	}
	.left{
		width: 0.1%;
		padding-right: 20px;
	}
	.left p{
		width: 100px;
	}
- flex
	- 兼容性
<!-- -->

	.parent{
		display: flex;
	}
	.left{
		margin-right: 20px;
	}
	.right{
		flex: 1;
	}
	.left p{
		width: 100px;
	}

---
###多列不定宽+自适应###
类似不定宽+自适应

	.left,.center{
		float: left;
		margin-right: 10px;
	}
	.right{
		overflow: hidden;
	}
	.left p,.center p{
		width: 100px;
	}

---
###等分布局###
每一列宽度一致，间距一致  
C+G=(W+G)*N   
div不设背景而div子元素设背景即可取消padding背景色 
####float####
margin-left为负可以增加宽度  
box-sizing:border-box 宽度包含padding

	.parent{
		margin-left: -20px;
	}
	.column{
		float: left;
		padding-left: 20px;
		width: 25%;
		box-sizing: border-box;
	}
优点：兼容性好，IE6,7部分兼容  
缺点：结构与样式耦合性，列数变化时需修改宽度

####table####
table默认宽度为内容宽度，且设为100%以后不能用margin增加宽度  
增加fix使用margin增加宽度  
table-layout:fixed 单元默认等分宽度

	.parent-fix{
		margin-left: -20px;
	}
	.parent{
		display: table;
		width: 100%;
		table-layout: fixed;
	}
	.column{
		display: table-cell;
		padding-left: 20px;
	}
优点：结构与样式解耦  
缺点：结构复杂

####flex####

	.parent{
		display: flex;
	}
	.column{
		flex: 1;
	}
	.column+.column{
		margin-left: 20px;
	}
优点：代码简单，结构与样式解耦  
缺点：兼容性

---
###等高布局###
####table####
table布局默认等高
####flex####
align-items默认为strech，默认等高
####float####
使用padding-box填充背景颜色  
使用margin-bottom抵消填充  
使用overflow截掉非文本部分  

	.left{
		width: 100px;
		float: left;
		margin-right: 10px;
	}
	.right{
		overflow: hidden;
	}
	.left,.right{
		padding-bottom: 9999px;
		margin-bottom: -9999px;
	}
	.parent{
		overflow: hidden;
	}
优点：兼容性好  
缺点：伪等高

---
##全屏布局##
###定宽高+自适应###
####position####
高性能，高兼容性
overflow+min-height+inner实现滚动

	html,body,.container{
		height: 100%;
		overflow: hidden;
	}
	.header{
		position: absolute;
		top:0;
		left: 0;
		right: 0;
		height: 100px;
	}
	.main{
		position: absolute;
		left: 0;
		right: 0;
		top: 100px;
		bottom: 100px;
	}
	.footer{
		position: absolute;
		left: 0;
		right: 0;
		bottom: 0;
		height: 100px;
	}
	.aside{
		width: 100px;
		position: absolute;
		left: 0;
		top: 100px;
		bottom: 100px;
	}
	.section{
		overflow: auto;
		position: absolute;
		left: 100px;
		top: 100px;
		right: 0;
		bottom: 100px;
		overflow: auto;
	}
	.inner{
		min-height: 1000px;
	}

####flex####
低性能+低兼容性
	
	html,body,.container{
		height: 100%;
		overflow: hidden;
	}
	.container{
		display: flex;
		flex-direction: column;
	}
	.header{
		height: 100px;
	}
	.main{
		flex: 1;
		display: flex;
	}
	.footer{
		height: 100px;
	}
	.aside{
		width: 100px;
	}
	.section{
		bottom: 100px;
		flex: 1;
		overflow: auto;
	}
	.inner{
		min-height: 1000px;
	}

###百分比宽高+自适应
百分比为相对定位元素的百分比  
把px高位百分比即可

###内容自适应###
flex不设宽高即可
