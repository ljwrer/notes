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
	 - 添加ownerDocument属性

####6. 元素的子节点

 - childNodes
 - getElementByTagName
 - 空白节点差异

###10.1.4 Text类型

 - nodeType:1
 - nodeName:#text
 - nodeValue == data
 - parentNode.nodeType === Node.ELEMENT_NODE
 - 无子节点
 - length

方法：

 - appendData(text)
 - deleteData(offset, count)
 - insertData(offset, text)
 - replaceData(offset, count, text)
 - splitText(offset)
 - substringData(offset, count)
 - 字符串会经过 HTML编码，符号转义

####1. 创建文本节点

 - document.createTextNode
	 - @params
		 - string
		 - 转义
	 - 添加ownerDocument属性

####2. 规范化文本节点

 - normalize
	 - 合并文本节点

####3. 分割文本节点

 - spliteText
	 - @params index(number)
		 - 分割0-(index-1)
	 - return
		 - index-(length-1)

###10.1.5 Comment类型

 - nodeType:8
 - nodeName:"#comment"
 - nodeValue == data
 - parentNode：Document/Element
 - 无子节点。

 - document.createComment
	 - 需要作为html的子节点

###10.1.6 CDATASection类型

 - 继承自text类型，除spliteText
 - nodeType:4
 - nodeName："#cdata-section"
 - nodeValue：CDATA 区域中的内容
 - parentNode：Document/Element
 - 无子节点
 - 仅XML支持

 -  document.createCDataSection

###10.1.7 DocumentType类型

 - IE不支持
 - nodeType：10
 - nodeName：doctype 的名称
 - nodeValue：null
 - parentNode:Document
 - 无子节点
 - document.doctype
	 - name:html

###10.1.8 DocumentFragment类型

 - nodeType:11
 - nodeName:"#document-fragment"
 - nodeValue:null
 - parentNode:null
 - 子节点:Element/ProcessingInstruction/Comment/Text/CDATASection/EntityReference
 - 缓存节点
	 - 插入dom后清除子节点
	 - 添加子节点后，子节点从dom移除
 - document.createDocumentFragment()

###10.1.9 Attr类型
元素的 attributes 属性中的节点

 - nodeType：2
 - nodeName == name
 - nodeValue == value
 - parentNode：null
 - HTML 中无子节点
 - XML 中子节点可以是 Text 或 EntityReference
 - 特性不被认为是 DOM 文档树的一部分

 - specified：默认/指定

不建议：

 - document.createAttribute
 - setAttributeNode
 - getAttributeNode

##10.2 DOM 操作技术
###10.2.1 动态脚本

	function loadScript(url){
		var script = document.createElement("script");
		script.type = "text/javascript";
		script.src = url;
		document.body.appendChild(script);
	}

	function loadScriptString(code){
		var script = document.createElement("script");
		script.type = "text/javascript";
		try {
			script.appendChild(document.createTextNode(code));
		} catch (ex){
			//IE
			script.text = code;
		}
		document.body.appendChild(script);
	}

###10.2.2 动态样式

	function loadStyles(url) {
        var link = document.createElement("link");
        link.rel = "stylesheet";
        link.type = "text/css";
        link.href = url;
        var head = document.getElementsByTagName("head")[0];
        head.appendChild(link);
    }

	function loadStyleString(css) {
        var style = document.createElement("style");
        style.type = "text/css";
        try {
            style.appendChild(document.createTextNode(css));
        } catch (ex) {
            style.styleSheet.cssText = css;
        }
        var head = document.getElementsByTagName("head")[0];
        head.appendChild(style);
    }

styleSheet.cssText，IE中重写或为空时可能崩溃

###10.2.3 操作表格
HTML DOM方法：

table：

 - caption
 - tBodies
 - tFoot
 - tHead
 - rows
 - createTHead()
 - createTFoot()
 - createCaption()
 - deleteTHead()
 - deleteTFoot()
 - deleteCaption()
 - deleteRow(pos)
 - insertRow(pos)
 - 没有tbody..

tbody：

 - rows
 - deleteRow(pos)
 - insertRow(pos)

tr：

 - cells
 - deleteCell(pos)
 - insertCell(pos)

###10.2.4 使用NodeList

 - NodeList,NamedNodeMap,HTMLCollection皆为动态，如getElementsByTagName/getElementsByClassName的返回
 - 尽量减少整个NodeList的遍历
 - 缓存NodeList的属性
	 - var len=SomeNodeList.length

##小结
DOM 是语言中立的 API，用于访问和操作 HTML 和 XML 文档。 DOM1 级将 HTML 和 XML 文档形象地看作一个层次化的节点树，可以使用 JavaScript 来操作这个节点树，进而改变底层文档的外观和结构。

DOM 由各种节点构成，简要总结如下。

 - 最基本的节点类型是 Node，用于抽象地表示文档中一个独立的部分；所有其他类型都继承自Node。
 - Document 类型表示整个文档，是一组分层节点的根节点。在 JavaScript 中， document 对象是Document 的一个实例。使用 document 对象，有很多种方式可以查询和取得节点。
 - Element 节点表示文档中的所有 HTML 或 XML 元素，可以用来操作这些元素的内容和特性。
 - 另外还有一些节点类型，分别表示文本内容、注释、文档类型、 CDATA 区域和文档片段。

访问 DOM 的操作在多数情况下都很直观，不过在处理`<script>`和`<style>`元素时还是存在一些复杂性。由于这两个元素分别包含脚本和样式信息，因此浏览器通常会将它们与其他元素区别对待。这些区别导致了在针对这些元素使用 innerHTML 时，以及在创建新元素时的一些问题。

理解 DOM 的关键，就是理解 DOM 对性能的影响。 DOM 操作往往是 JavaScript 程序中开销最大的部分，而因访问 NodeList 导致的问题为最多。 NodeList 对象都是“动态的”，这就意味着每次访问NodeList 对象，都会运行一次查询。有鉴于此，最好的办法就是尽量减少 DOM 操作。