#第12章 DOM2 和 DOM3
##12.1 DOM 变化

	 document.implementation.hasFeature("Core", "2.0");
###12.1.1 针对XML命名空间的变化
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

####1. Node 类型的变化

 - localName：不带命名空间前缀的节点名称。
 - nodeName:prefix+":"+ localName
 - namespaceURI：命名空间 URI(如http://www.w3.org/2000/svg) 或者（在未指定的情况下是） null。
 - prefix


 - isDefaultNamespace(namespaceURI)：在指定的 namespaceURI 是当前节点的默认命名空间的情况下返回 true。
 - lookupNamespaceURI(prefix)：返回给定 prefix 的命名空间。
 - lookupPrefix(namespaceURI):返回给定 namespaceURI 的前缀

####2. Document 类型的变化

 - createElementNS(namespaceURI, tagName)
 - createAttributeNS(namespaceURI, attributeName)
 - getElementsByTagNameNS(namespaceURI, tagName)

####3. Element 类型的变化

 - getAttributeNS(namespaceURI,localName)
 - getAttributeNodeNS(namespaceURI,localName)
 - getElementsByTagNameNS(namespaceURI, tagName)
 - hasAttributeNS(namespaceURI,localName)
 - hasAttribute()
 - removeAttriubteNS(namespaceURI,localName)
 - setAttributeNS(namespaceURI,qualifiedName,value)
 - setAttributeNodeNS(attNode)

####4. NamedNodeMap 类型的变化

 - getNamedItemNS(namespaceURI,localName)
 - removeNamedItemNS(namespaceURI,localName)
 - setNamedItemNS(node)

###12.1.2 其他方面的变化
####1. DocumentType 类型的变化
publicId、 systemId 和 internalSubset
####2. Document 类型的变化
importNode：自动更新ownerDocument 

	var newNode = document.importNode(oldNode, true); //导入节点及其所有子节点
	document.body.appendChild(newNode);

#####DOM2 级视图：
defaultView ： 指向拥有给定文档的窗口（或框架）

	//兼容IE
	var parentWindow = document.defaultView || document.parentWindow;
#####DOM2 级核心
document.implementation.createDocumentType
document.implementation.createDocument

	var doctype = document.implementation.createDocumentType("html"," -//W3C//DTD XHTML 1.0 Strict//EN","http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd");
	var doc = document.implementation.createDocument("http://www.w3.org/1999/xhtml","html", doctype);

#####DOM2 级 HTML
document.implementation.createHTMLDocument

	var htmldoc = document.implementation.createHTMLDocument("New Doc");
	alert(htmldoc.title); //"New Doc"
	alert(typeof htmldoc.body); //"object"

####3. Node 类型的变化
isSupported：不建议使用，返回与实现不一致

	document.body.isSupported("HTML", "2.0")

isSameNode()和 isEqualNode()：类似 === ==

setUserData()/getUserData()

	document.body.setUserData("name", "Nicholas", function(){});
	var value = document.body.getUserData("name");