#布局解决方案#
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
display:table类似display:child,但宽度为内容宽度  
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
