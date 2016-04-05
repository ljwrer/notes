#第11 章 DOM 扩展
##11.1 选择符 API
Selectors API Level 1:IE 8+、 Firefox 3.5+、 Safari 3.1+、 Chrome 和 Opera 10+

 - querySelector
	 - @params css选择符
	 - return node/null
	 - throw 不支持选择符
 - querySelectorAll
	- @params css选择符
 	- return nodeList/null
	 	- 带有所有属性和方法的 NodeList,但非动态
	 	- 类似于一组元素的快照，而非不断对文档进行搜索的动态查询
	 	- 避免使用 NodeList 对象通常会引起的大多数性能问题
 	- throw 不支持选择符
 - matchesSelector
 	-  @params css选择符
 	-  return Boolean
	 	-  元素是否匹配选择符
 	-  Selectors API Level 2

##11.2 元素遍历
Element Traversal API ：IE 9+、 Firefox 3.5+、 Safari 4+、 Chrome 和 Opera 10+

 - childElementCount
 - firstElementChild
 - lastElementChild
 - previousElementSibling
 - nextElementSibling

##11.3 HTML5
###11.3.1 与类相关的扩充

####getElementsByClassName

 - @params ClassName
 - return nodeList
 - IE 9+

####classList

x.classList.constructor === DOMTokenList

 - add(value)
 - contains(value)
 - remove(value)
 - toggle(value)

html5之前：

		var classNames = div.className.split(/\s+/);
		var pos = -1,
			i,
			len;
		for (i=0, len=classNames.length; i < len; i++){
			if (classNames[i] == "user"){
				pos = i;
				break;
			}
		}
		classNames.splice(i,1);
		div.className = classNames.join(" ");	

###11.3.2 焦点管理
IE4+
document.activeElement
document.hasFocus()

###11.3.3 HTMLDocument的变化

####readyState

 - IE4+
 - loading/complete

####document.compatMode

 - IE6+
 - CSS1Compat/BackCompat

####document.head 

	var head=document.head||document.getElementByTagName("head")[0];

###11.3.4 字符集属性
document.charset
document.deafultCharset
 
###11.3.5 自定义数据属性
data-x-y-z
ele.dataset.xYZ

###11.3.6 插入标记
####innerHTML
插入style和script标签时有一些问题
IE8:window.toStaticHTML

####outerHTML 
ff7+

包括外部的HTML

####insertAdjacentHTML
位置参数

	1<div>2xxxx3</div>4

 - "beforebegin"
 - "afterbegin"
 - "beforeend"
 - "afterend"

####性能问题
手工删除要被替换的元素的所有事件处理程序和 JavaScript 对象属性,如$.off()

###11.3.7 scrollIntoView()方法

 - @params
	 - true 与视口顶部平齐
	 - false 与视口底部平齐

##11.4 专有扩展
###11.4.1 文档模式
仅限IE8+

	<meta http-equiv="X-UA-Compatible" content="IE=IEVersion">
	<meta http-equiv="X-UA-Compatible" content="IE=Edge">
	
###11.4.2 children属性
仅含元素节点
IE8-包含注释节点

###11.4.3 contains()方法
ff9+

DOM3:compareDocumentPosition

	var result = document.documentElement.compareDocumentPosition(document.body);
	alert(!!(result & 16))

	function contains(refNode, otherNode) {
	    if (typeof refNode.contains == "function" &&
	        (!client.engine.webkit || client.engine.webkit >= 522)) {
	        return refNode.contains(otherNode);
	    } else if (typeof refNode.compareDocumentPosition == "function") {
	        return !!(refNode.compareDocumentPosition(otherNode) & 16);
	    } else {
	        var node = otherNode.parentNode;
	        do {
	            if (node === refNode) {
	                return true;
	            } else {
	                node = node.parentNode;
	            }
	        } while (node !== null);
	        return false;
	    }
	}

###11.4.4 插入文本
innerText FF不支持
textContent IE9+

	function getInnerText(element) {
	    return (typeof element.textContent == "string") ?
	        element.textContent : element.innerText;
	}
	function setInnerText(element, text) {
	    if (typeof element.textContent == "string") {
	        element.textContent = text;
	    } else {
	        element.innerText = text;
	    }
	}

	if (!("innerText" in document.body)) {
		HTMLElement.prototype.__defineGetter__("innerText", function() {
			return this.textContent;
		});
		HTMLElement.prototype.__defineSetter__("innerText", function(s) {
			this.textContent = s;
		});
	}

outerText  FF不支持,类似outerHTML

###11.4.5 滚动

 - scrollIntoViewIfNeeded(alignCenter)
 - scrollByLines(lineCount)
 - scrollByPages(pageCount)：