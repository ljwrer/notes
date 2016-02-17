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
###