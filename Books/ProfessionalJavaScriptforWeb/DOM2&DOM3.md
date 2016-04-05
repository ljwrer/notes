#第12章 DOM2 和 DOM3
>*：IE

---

#12.1 DOM 变化

	 document.implementation.hasFeature("Core", "2.0");
##12.1.1 针对XML命名空间的变化
xml命名空间：

	<xhtml:html xmlns:xhtml="http://www.w3.org/1999/xhtml">
	    <xhtml:head>
	        <xhtml:title>Example XHTML page</xhtml:title>
	    </xhtml:head>
	    <xhtml:body xhtml:class="home">
	        Hello world!
	    </xhtml:body>
	</xhtml:html>	

带命名空间的svg:

	<html xmlns="http://www.w3.org/1999/xhtml">
	    <head>
	        <title>Example XHTML page</title>
	    </head>
	    <body>
	        <svg xmlns="http://www.w3.org/2000/svg" version="1.1"
	             viewBox="0 0 100 100" style="width:100%; height:100%">
	            <rect x="0" y="0" width="100" height="100" style="fill:red"/>
	        </svg>
	    </body>
	</html>

>DOM2 级核心通过为大多数 DOM1 级方法提供特定于命名空间的版本

###1. Node 类型的变化

 - localName：不带命名空间前缀的节点名称。
 - nodeName:prefix+":"+ localName
 - namespaceURI：命名空间 URI(如http://www.w3.org/2000/svg) 或者（在未指定的情况下是） null。
 - prefix


 - isDefaultNamespace(namespaceURI)：在指定的 namespaceURI 是当前节点的默认命名空间的情况下返回 true。
 - lookupNamespaceURI(prefix)：返回给定 prefix 的命名空间。
 - lookupPrefix(namespaceURI):返回给定 namespaceURI 的前缀

###2. Document 类型的变化

 - createElementNS(namespaceURI, tagName)
 - createAttributeNS(namespaceURI, attributeName)
 - getElementsByTagNameNS(namespaceURI, tagName)

###3. Element 类型的变化

 - getAttributeNS(namespaceURI,localName)
 - getAttributeNodeNS(namespaceURI,localName)
 - getElementsByTagNameNS(namespaceURI, tagName)
 - hasAttributeNS(namespaceURI,localName)
 - hasAttribute()
 - removeAttriubteNS(namespaceURI,localName)
 - setAttributeNS(namespaceURI,qualifiedName,value)
 - setAttributeNodeNS(attNode)

###4. NamedNodeMap 类型的变化

 - getNamedItemNS(namespaceURI,localName)
 - removeNamedItemNS(namespaceURI,localName)
 - setNamedItemNS(node)

##12.1.2 其他方面的变化
###1. DocumentType 类型的变化
publicId、 systemId 和 internalSubset
###2. Document 类型的变化
importNode：自动更新ownerDocument 

	var newNode = document.importNode(oldNode, true); //导入节点及其所有子节点
	document.body.appendChild(newNode);

####DOM2 级视图：
defaultView ： 指向拥有给定文档的窗口（或框架）

	//兼容IE
	var parentWindow = document.defaultView || document.parentWindow;
####DOM2 级核心
document.implementation.createDocumentType
document.implementation.createDocument

	var doctype = document.implementation.createDocumentType("html"," -//W3C//DTD XHTML 1.0 Strict//EN","http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd");
	var doc = document.implementation.createDocument("http://www.w3.org/1999/xhtml","html", doctype);

####DOM2 级 HTML
document.implementation.createHTMLDocument

	var htmldoc = document.implementation.createHTMLDocument("New Doc");
	alert(htmldoc.title); //"New Doc"
	alert(typeof htmldoc.body); //"object"

###3. Node 类型的变化
isSupported：不建议使用，返回与实现不一致

	document.body.isSupported("HTML", "2.0")

isSameNode()和 isEqualNode()：类似 === ==

setUserData()/getUserData()

	document.body.setUserData("name", "Nicholas", function(operation, key, value, src, dest){});
	var value = document.body.getUserData("name");

###4. 框架的变化
iframe.contentDocument表示框架内容的文档对象

	var iframeDoc = iframe.contentDocument || iframe.contentWindow.document;

---

#12.2 样式

IE9+:

	var supportsDOM2CSS = document.implementation.hasFeature("CSS", "2.0");
	var supportsDOM2CSS2 = document.implementation.hasFeature("CSS2", "2.0");

##12.2.1 访问元素的样式
行内样式：

	ele.style instanceof CSSStyleDeclaration
	
cssFloat/styleFloat

###1. DOM 样式属性和方法
CSSStyleDeclaration：

 - cssText
 - length
 - parentRule
 - getPropertyCSSValue(propertyName)
	 - @return CSSValue{cssText,cssValueType} 
	 - IE9+ FF7+
 - getPropertyPriority(propertyName)
 - getPropertyValue(propertyName)
	 - @return string cssText
 - item(index)
 - removeProperty(propertyName)
 - setProperty(propertyName,value,priority)

###2. 计算的样式
包含样式表的样式,只读

	document.defaultView.getComputedStyle(myDiv, pseudo)
	//IE
	myDiv.currentStyle

##12.2.2 操作样式表
HTMLLinkElement 和 HTMLStyleElement

	var supportsDOM2StyleSheets = document.implementation.hasFeature("StyleSheets", "2.0");
	link.sheet instanceof CSSStyleSheet
	style.sheet instanceof CSSStyleSheet
	CSSStyleSheet.prototype instanceof StyleSheet

CSSStyleSheet 继承自 StyleSheet

StyleSheet:

 - disabled(可写)
 - href
 - media
 - ownerNode：`<link><style>null`
 - parentStyleSheet：@import
 - title： ownerNode.title
 - type："type/css"

CSSStyleSheet：

 - cssRules/rules(IE)：CSSRuleList
 - ownerRule：@import，IE 不支持
 - deleteRule(index)/removeRule()(IE)
 - insertRule(rule,index)/addRule()(IE)

获取CSSStyleSheet:
1.document.styleSheets:

 - @return StyleSheetList
	 - `<style>`
	 - `<link rel="stylesheet">`
	 - `<link rel="alternate stylesheet">`(IE)

2.link.sheet和style.sheet(link.styleSheet和style.styleSheet IE):

	function getStyleSheet(element){
		return element.sheet || element.styleSheet;
	}

###1. CSS 规则  
CSSRule基类
CSSStyleRule:
即每一条css规则，如

	body{
		margin:0,
		padding:0	
	}

 - cssText：包含选择符文本和围绕样式信息的花括号,只读*
 - parentRule：*
 - parentStyleSheet：*
 - selectorText：只读
 - style：一个 CSSStyleDeclaration 对象，可以通过它设置和取得规则中特定的样式值。
	 - 即 {margin:"0px",padding:"0px"}
	 - 支持数组访问法，返回属性名
 - type：对于样式规则，这个值是 1。 IE 不支持这个属性。 

		var sheet = document.styleSheets[0];
		var rules = sheet.cssRules || sheet.rules; //取得规则列表
		var rule = rules[0]; //取得第一条规则
		alert(rule.selectorText); //"div.box"
		alert(rule.cssText); //完整的 CSS 代码
		alert(rule.style.backgroundColor); //"blue"
		alert(rule.style.width); //"100px"
		alert(rule.style.height); //"200px"
		rule.style.backgroundColor = "red"

###2. 创建规则

	sheet.insertRule("body { background-color: silver }", 0)
	sheet.addRule("body", "background-color: silver", 0); //仅对 IE 有效

	function insertRule(sheet, selectorText, cssText, position){
        if (sheet.insertRule){
            sheet.insertRule(selectorText + "{" + cssText + "}", position);
        } else if (sheet.addRule){
            sheet.addRule(selectorText, cssText, position);
        }
    }

###3.删除规则
	
	sheet.deleteRule(0);
	sheet.removeRule(0);	//仅对 IE 有效
	function deleteRule(sheet, index){
        if (sheet.deleteRule){
            sheet.deleteRule(index);
        } else if (sheet.removeRule){
            sheet.removeRule(index);
        }
    }

##12.2.3 元素大小
###1. 偏移量
需要计算产生，只读，缓存

- offsetHeight：元素在垂直方向上占用的空间大小，以像素计。包括元素的高度、（可见的）水平滚动条的高度、上边框高度和下边框高度。
- offsetWidth：元素在水平方向上占用的空间大小，以像素计。包括元素的宽度、（可见的）垂直滚动条的宽度、左边框宽度和右边框宽度。
- offsetLeft：元素的左外边框至包含元素的左内边框之间的像素距离。
- offsetTop：元素的上外边框至包含元素的上内边框之间的像素距离。
- offsetParent:包含元素,最近的一个具有大小的元素

		function realOffset(ele,prop){
	        var value=ele[prop];
	        var parent=ele.offsetParent;
	        while(parent !== null){
	            value+=parent[prop];
	            parent=parent.offsetParent;
	        }
	        return value;
	    }
除了绝对定位，parentOffset一般为body

###2. 客户区大小
需要计算产生，只读，缓存

 - clientWidth 属性是元素内容区宽度加上左右内边距宽度
 - clientHeight 属性是元素内容区高度加上上下内边距高度

###3. 滚动大小

 - scrollHeight：在没有滚动条的情况下，元素内容的总高度。
 - scrollWidth：在没有滚动条的情况下，元素内容的总宽度。
 - scrollLeft：被隐藏在内容区域左侧的像素数。通过设置这个属性可以改变元素的滚动位置。
 - scrollTop：被隐藏在内容区域上方的像素数。通过设置这个属性可以改变元素的滚动位置。

scrollWidth 和 scrollHeight 主要用于确定元素内容的实际大小，`<html>`,`<body>`(IE6)

	//有滚动条时页面高度
	document.documentElement.scrollHeight
	//无滚动条时页面高度，IE<body>
	var docHeight = Math.max(document.documentElement.scrollHeight,
	document.documentElement.clientHeight);
	var docWidth = Math.max(document.documentElement.scrollWidth,
	document.documentElement.clientWidth);

	function scrollToTop(element){
		if (element.scrollTop != 0){
			element.scrollTop = 0;
		}
	}

###4. 确定元素大小
getBoundingClientRect()：
相对于视口顶部与左边

	function getBoundingClientRect(element) {
        var scrollTop = document.documentElement.scrollTop;
        var scrollLeft = document.documentElement.scrollLeft;
        if (element.getBoundingClientRect) {
            if (typeof arguments.callee.offset != "number") {
                var temp = document.createElement("div");
                temp.style.cssText = "position:absolute;left:0;top:0;";
                document.body.appendChild(temp);
                arguments.callee.offset = -temp.getBoundingClientRect().top - scrollTop;
                document.body.removeChild(temp);
                temp = null;
            }
            var rect = element.getBoundingClientRect();
            var offset = arguments.callee.offset;
            return {
                left: rect.left + offset,
                right: rect.right + offset,
                top: rect.top + offset,
                bottom: rect.bottom + offset
            };
        } else {
            var actualLeft = getElementLeft(element);
            var actualTop = getElementTop(element);
            return {
                left: actualLeft - scrollLeft,
                right: actualLeft + element.offsetWidth - scrollLeft,
                top: actualTop - scrollTop,
                bottom: actualTop + element.offsetHeight - scrollTop
            }
        }
    }

---

#12.3 遍历
NodeIterator 和 TreeWalker,深度优先：优先遍历完左子树
	var supportsTraversals = document.implementation.hasFeature("Traversal", "2.0");
	var supportsNodeIterator = (typeof document.createNodeIterator == "function");
	var supportsTreeWalker = (typeof document.createTreeWalker == "function");

##12.3.1 NodeIterator
IE9+

 - root
 - whatToShow
	 - 位掩码
	 - NodeFilter.XXX 
	 - 除NodeFilter.SHOW_ALL外，支持或操作
			var whatToShow = NodeFilter.SHOW_ELEMENT | NodeFilter.SHOW_TEXT;
 - filter：实现acceptNode方法的对象或函数
	 - @return NodeFilter.FILTER_ACCEPT :NodeFilter.FILTER_SKIP;
 - entityReferenceExpansion：false

		var iterator = document.createNodeIterator(div, NodeFilter.SHOW_ELEMENT,null, false);


 - nextNode()
	 - root-->null
 - previousNode()
	 - null-->root

##12.3.2 TreeWalker
拥有NodeIterator全部功能，能任意方向遍历

 - parentNode()
 - firstChild()
 - lastChild()
 - nextSibling()
 - previousSibling()

	var walker= document.createTreeWalker(div, NodeFilter.SHOW_ELEMENT,filter, false);

filter:`NodeFilter.FILTER_REJECT`跳过节点及其子树

currentNode:上一次遍历中返回的节点,可写，修改起点

---

#12.4 范围

	var supportsRange = document.implementation.hasFeature("Range", "2.0");
	var alsoSupportsRange = (typeof document.createRange == "function");
	//归属于创建的DOM
	var range = document.createRange();

 - startContainer：包含范围起点的节点（即选区中第一个节点的父节点）。
 - startOffset：范围在 startContainer 中起点的偏移量。如果 startContainer 是文本节点、注释节点或 CDATA 节点，那么 startOffset 就是范围起点之前跳过的字符数量。否则，startOffset 就是范围中第一个子节点的索引。
 - endContainer：包含范围终点的节点（即选区中最后一个节点的父节点）。
 - endOffset：范围在 endContainer 中终点的偏移量 （与 startOffset 遵循相同的取值规则）。
 - commonAncestorContainer： startContainer 和 endContainer 共同的祖先节点在文档树中位置最深的那个。

##12.4.1 DOM中的范围
###1. 用 DOM 范围实现简单选择
范围包含空白节点
selectNode()或 selectNodeContents()

 - setStartBefore(refNode)
 - setStartAfter(refNode)
 - setEndBefore(refNode)
 - setEndAfter(refNode)

###2. 用 DOM 范围实现复杂选择
setStart()和 setEnd()

 - @params1:refNode
 - @params2:offset

###3. 操作 DOM 范围中的内容
range中的DOM片段会自动闭和

 - deleteContents()
	 - 删除范围内的片段
 - extractContents()
	 - 移除
	 - 返回移除的片段
 -  cloneContents()

###4. 插入 DOM 范围中的内容

 - insertNode()
 - surroundContents() (类似wrapper)

###5. 折叠 DOM 范围

 - collapse(boolean)
 - collapse

###6. 比较 DOM 范围

 - compareBoundaryPoints(const,range)
 - const:
	 - Range.START_TO_START(0)：比较第一个范围和第二个范围的起点；
	 - Range.START_TO_END(1)：比较第一个范围的起点和第二个范围的终点；
	 - Range.END_TO_END(2)：比较第一个范围和第二个范围的终点；
	 - Range.END_TO_START(3)：比较第一个范围的终点和第一个范围的起点。
 - return
	 - 0
	 - 1后
	 - -1前

###7. 复制 DOM 范围

	var newRange = range.cloneRange();

###8. 清理 DOM 范围

	range.detach(); //从文档中分离
	range = null; //解除引用

##12.4.2 IE8 及更早版本中的范围	

	var range = document.body.createTextRange();

###1. 用 IE 范围实现简单的选择

 - findText()
 - text
 - moveToElementText() == selectNode()
 - parentElement() == commonAncestorContainer

###2. 使用 IE 范围实现复杂的选择

 - move()、 moveStart()、 moveEnd()和 expand()(规范化,组成单词或句子)
	 - @params (unit,number)
	 - unit
		 - "character"：逐个字符地移动。
		 - "word"：逐个单词（一系列非空格字符）地移动。
		 - "sentence"：逐个句子（一系列以句号、问号或叹号结尾的字符）地移动。
		 - "textedit"：移动到当前范围选区的开始或结束位置。

###3. 操作 IE 范围中的内容

 - text
 - pasteHTML()

###4. 折叠 IE 范围
 
 - collapse(boolean)
 - boundingWidth == collapse

###5. 比较 IE 范围

 - compareEndPoints() == compareBoundaryPoints()
 - isEqual()
 - inRange()

###6. 复制 IE 范围

 - duplicate()

---

#小结
DOM2 级规范定义了一些模块，用于增强 DOM1 级。“ DOM2 级核心”为不同的 DOM 类型引入了
一些与 XML 命名空间有关的方法。这些变化只在使用 XML 或 XHTML 文档时才有用；对于 HTML 文
档没有实际意义。除了与 XML 命名空间有关的方法外，“ DOM2 级核心”还定义了以编程方式创建
Document 实例的方法，也支持了创建 DocumentType 对象。
“ DOM2 级样式”模块主要针对操作元素的样式信息而开发，其特性简要总结如下。

 - 每个元素都有一个关联的 style 对象，可以用来确定和修改行内的样式。
 - 要确定某个元素的计算样式（包括应用给它的所有 CSS 规则），可以使用 getComputedStyle()方法。
 - IE 不支持 getComputedStyle()方法，但为所有元素都提供了能够返回相同信息 currentStyle属性。
 - 可以通过 document.styleSheets 集合访问样式表。
 - 除 IE 之外的所有浏览器都支持针对样式表的这个接口， IE 也为几乎所有相应的 DOM 功能提供了自己的一套属性和方法。“ DOM2 级遍历和范围”模块提供了与 DOM 结构交互的不同方式，简要总结如下。
 - 遍历即使用 NodeIterator 或 TreeWalker 对 DOM 执行深度优先的遍历。
 - NodeIterator 是一个简单的接口，只允许以一个节点的步幅前后移动。而 TreeWalker 在提供相同功能的同时，还支持在 DOM 结构的各个方向上移动，包括父节点、同辈节点和子节点等方向。
 - 范围是选择 DOM 结构中特定部分，然后再执行相应操作的一种手段。
 - 使用范围选区可以在删除文档中某些部分的同时，保持文档结构的格式良好，或者复制文档中的相应部分。
 - IE8 及更早版本不支持“ DOM2 级遍历和范围”模块，但它提供了一个专有的文本范围对象，可以用来完成简单的基于文本的范围操作。 IE9 完全支持 DOM 遍历。