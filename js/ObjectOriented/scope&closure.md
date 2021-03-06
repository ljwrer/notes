#作用域与闭包
---
#变量作用域
 - 生命周期
 - 作用范围
 - 引用的值来自何处


##静态作用域
词法作用域
  
 - 编译阶段决定变量的引用
 - 只和程序定义的位置有关系
 - 与代码执行顺序无关
 - 不用考虑嵌套关系

 
---
##js变量作用域

 - 使用静态作用域
 - 没有块级作用域
 - ES5使用词法环境管理静态作用域
 

---
##词法环境##
描述静态作用域的数据结构

 - 环境记录
 	- 作用域定义的函数
 	- 形参
 	- 变量
 - 对外部词法环境的引用(outer) 嵌套结构  最外层为null

###创建：
代码执行之前，先初始化词法环境
全局代码或者函数代码开始执行前
 - 形参值初始化
 - 函数定义，即函数声明
 - 变量定义  var定义的变量，初始化时为undefined

以上在代码开始执行前写入环境记录
var变量写入时为undefined，执行时再赋值

###函数定义：
初始化词法环境时创建函数对象  
将形参，函数体放入函数对象  
保存当前作用域到函数对象  

###函数执行：
执行函数之前先创建函数的词法作用域，之后同上，且outer保存当前外部作用域
 - 形参值初始化
 - 函数定义，即函数声明
 - 变量定义  var定义的变量，初始化时为undefined

由于静态作用域中变量只和程序定义的位置有关系，因此返回的函数外部作用域依旧为定义时的外部作用域，out指向定义时的外部作用域

###问题：
 1. 形参、函数定义、变量定义名称冲突
	 - 优先级：函数定义>形参>变量定义
 2. arguments对象
	 - 函数内部定义好的对象 
	 - 通过arguments访问函数传过来的实参
	 - arguments也放入环境记录
 3. 函数表达式
	 - 函数表达式 var定义，初始化时为undefined 
	 - 函数的词法环境是在函数执行前，函数定义的outer是在创建时设置了scope,代码执行之前就完成了
	 - 函数表达式在执行时才创建函数对象，scope为当前作用域
	 - 函数执行期间词法环境可能发生变化

###词法环境变化
####with
初始化不变  
执行到with时创建with的临时with词法环境  
将with里对象的属性定义到临时词法环境  
outer指向当前外部作用域  
with里使用var定义变量和with外var定义变量一样  
with执行外后词法环境消失  
####try...catch
初始化不变  
执行到catch时创建catch的临时with词法环境  
词法环境里只有一个变量e:Error  
###带名称的函数表达式
创建新的词法环境（名称为函数表达式名称）  
将函数本身定义到该环境，outer指向外层作用域  
且函数定义本身不能被修改  

---

#闭包
函数作为变量被返回时，创建函数变量对象  
包含形参，函数体内容  
并且`保存`当前作用域环境，环境中的变量变为自由变量，scope指向当前的作用域  
注意：是保存不是引用！  
外部接受结果的变量除了自己的函数代码，还保存了对外部作用域的引用  

返回函数执行：
创建closure词法环境  
外层作用域为之前保存的外部作用域  

总结：函数作为返回值的时候，函数肯能引用外部的变量，保存对外部词法环境的引用

 - 闭包由函数和与其相关的引用环境组合而成
 - 不仅包含一段代码，还包含上下文的环境
 - 闭包允许函数访问器引用环境中的变量，又称自由变量
 - 广义上来说，所有JS的函数都可以称为闭包，因为JS函数在创建时保存了当前的词法环境

##闭包的应用
 - 保存变量现场
 - 封装：信息隐藏
