#第三章 DOM#
##3.4 节点##
 - 元素节点 nodeType==1
 - 文本节点 nodeType==3
 - 属性节点 nodeType==2

###3.4.5 获取元素###
getElementsByClassName（"a b c"）  

 - 参数顺序无关
 - 需要兼容

###3.5.1 getAttribute###
为空则返回null
###3.5.2 setAttribute###
修改不会反映在源代码里

##小结##
DOM工作模式

 - 先加载文档静态资源
 - 再动态刷新，动态刷新不影响文档的静态内容
 - DOM对页面内容进行刷新不需要在浏览器里刷新页面
