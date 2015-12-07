#第10章 DOM
##10.1 节点层次
document>html>head
###10.1.1 Node类型
  - nodeType:
	  - Node.ELEMENT_NODE(1)
	  - Node.ATTRIBUTE_NODE(2)
	  - Node.TEXT_NODE(3)

####1.nodeName 和 nodeValue 属性
 不同nodeType意义不一致
ELEMENT_NODE：nodeName==TagName

####2. 节点关系

 -  childNodes
	 -  NodeList 对象
		 -  IE8及以下为COMd对象
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