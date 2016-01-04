#第10章 DOM
##10.1 节点层次
document>html>head
###10.1.1 Node类型
  - nodeType:
	  - Node.ELEMENT_NODE(1)
	  - Node.ATTRIBUTE_NODE(2)
	  - Node.TEXT_NODE(3)
	  - IE9 之前的版本不会为空白符创建节点

####1.nodeName 和 nodeValue 属性
 不同nodeType意义不一致
ELEMENT_NODE：nodeName==TagName

####2. 节点关系

 -  childNodes
	 -  NodeList 对象
		 -  IE8及以下为COM对象
	 -  .length
	 -  动态
	 -   [index]/.item[index]
	
IE8+:

	var arrayOfNodes = Array.prototype.slice.call(someNode.childNodes,0);
	//兼容
	function convertToArray(nodes) {
        var array = null;
        try {
            array = Array.prototype.slice.call(nodes, 0); //针对非 IE 浏览器
        } catch (ex) {
            array = new Array();
            for (var i = 0, len = nodes.length; i < len; i++) {
                array.push(nodes[i]);
            }
        }
        return array;
    }

  - parentNode
  - previousSibling/nextSibling 
  - firstChild 和 lastChild
  - hasChildNodes()
  - ownerDocument指向表示整个文档的文档节点

####3. 操作节点

 - node.appendChild
	 - @params childNode
		 - childNode可以是文档中现有节点，相当于移动节点
		 - 任何 DOM 节点也不能同时出现在文档中的多个位置上
	 - return childNode
 - node.insertBefore
	 - @params1 childNode
	 - @prams2 siblingNode
		 - null:等价于appendChild
	 - return childNode
 - node.replaceChild
	 - @params1 insertNode
	 - @params2 replacedNode
	 - return  replacedNode
	 - 被替换的节点仍然还在文档中，但它在文档中已经没有了自己的位置
 - node.removeChild
	 - @params removedChild
	 - return removedChild
	 - 通过 removeChild()移除的节点仍然为文档所有，只不过在文档中已经没有了自己的位置。

前面介绍的四个方法操作的都是某个节点的子节点，也就是说，要使用这几个方法必须先取得父节点（使用 parentNode 属性）

####4. 其他方法
	
 -  cloneNode
	 -  @parms Boolean
		 -  true 深拷贝，复制节点及其整个子节点树，包括attr和options,不包括JavaScript 属性
		 -  false 浅拷贝，只复制节点本身
 -  normalize
	 -  删除所有子节点中的空文本节点
	 -  合并所有子节点中的相邻文本节点

###10.1.2 Document类型

	document instanceof HTMLDocument
	HTMLDocument.prototype instanceof Document

 - nodeType:9
 - nodeName:"#document"
 - nodeValue:null
 - childNodes: DocumentType、 Element、 ProcessingInstruction 或 Comment

####1. 文档的子节点

 - document.doctype
	 - 浏览器支持不一致
 - document.documentElement
 - document.body

####2. 文档信息

 - document.title
	 - 可写
 - document.URL
 - document.domain
	 - 可向下写：如www.xxx.com-->xxx.com
	 - 不同iframe之间domain设为一致可以互相访问js对象
 - document.referrer
	 - 链接到当前页面的那个页面的 URL

####3. 查找元素

 - getElementById
	 -  大小写严格匹配（IE8+）
	 -  表单元素name 特性等于指定的 ID，而且该元素在文档中位于带有给定 ID 的元素前面，那么 IE7 就会返回那个表单元素
 - getElementsByTagName
	 - @pramas 
		 - tagName html不区分大小写
		 - * IE返回注释节点
	 - return HTMLCollection
		 - 动态
		 - [index]`调用item`/item(index)/[name]`调用namedItem`
		 - length
		 - namedItem()
			 - @params name
			 - return namedItem
 -  getElementsByName
	 -  return HTMLCollection
		 -  namedItem()方法则只会取得第一项

####4. 特殊集合
HTMLCollection 对象：

 - document.anchors，包含文档中所有带 name 特性的`<a>`元素
 - document.forms，包含文档中所有的<form>元素，与 document.getElementsByTagName("form")得到的结果相同
 - document.images，包含文档中所有的<img>元素，与 document.getElementsByTagName("img")得到的结果相同
 - document.links，包含文档中所有带 href 特性的`<a>`元素
 
####5. DOM 一致性检测
不准确
	
	document.implementation.hasFeature(<feature>, <version>)

####6. 文档写入
write()、 writeln()、 open()和 close()

###10.1.3 Element类型

 - nodeType:1
 - nodeName：tagName
	 - 大写
 - nodeValue:null
 - parentNode:Document/Element
 - childNodes:Element/Text/Comment/ProcessingInstruction/CDATASection/EntityReference
 - tagName==nodeName

####1. HTML 元素

	HTML[]Element.prototype instanceof HTMLElement
	HTMLElement.prototype instanceof Element

可读写属性：

 - id
 - title
 - lang
 - dir:"ltr"/"rtl"
 - className

####2. 取得特性

 - getAttribute()
	 - params1 attrname,不区分大小写
 
区分Element.attr/Element.getAttribute

 - style
	 - getAttribute string
	 - . CSSStyleDeclaration
 - onclick
	 - getAttribute string
	 - . function

####3. 设置特性

 - setAttribute()
	 - attrname统一转换为小写形式
	 - 支持自定义属性
 - .attr
	 - 自定义的属性不会自动成为元素的特性
 - removeAttribute()

####4. attributes 属性

	Element.attributes instanceof NamedNodeMap

 - 动态
 - getNamedItem(name)：返回 nodeName 属性等于 name 的节点
 - removeNamedItem(name)：从列表中移除 nodeName 属性等于 name 的节点，返回删除的attr节点
 - setNamedItem(node)：向列表中添加节点，以节点的 nodeName 属性为索引
 - item(pos)：返回位于数字 pos 位置处的节点。

#####attributes遍历

	function outputAttributes(element){
		var pairs = new Array(),
		attrName,
		attrValue,
		i,
		len;
		for (i=0, len=element.attributes.length; i < len; i++){
		attrName = element.attributes[i].nodeName;
		attrValue = element.attributes[i].nodeValue;
		if (element.attributes[i].specified) {
		pairs.push(attrName + "=\"" + attrValue + "\"");
		}
		}
		return pairs.join(" ");
	}

####5. 创建元素

 - document.createElement
	 - @params1
		 - tagName
		 - string(IE7, 处理reset问题)

####6. 元素的子节点

 - childNodes
 - getElementByTagName

###10.1.4 Text类型

 - nodeType:1
 - nodeName:#text
 - nodeValue == data

	
