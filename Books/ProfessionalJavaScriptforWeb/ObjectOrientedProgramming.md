#第6章 面向对象的程序设计
可以把ECMAScript 的对象想象成散列表：无非就是一组名值对，其中值可以是数据或函数。
每个对象都是基于一个引用类型创建的，这个引用类型可以是原生类型，也可以是开发人员定义的类型。
#6.1 理解对象
ECMAScript 中有两种属性：数据属性和访问器属性

##6.1.1 属性类型
ES5(IE9+)
###1.数据属性

 - [[Configurable]]：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true。
 - [[Enumerable]]：表示能否通过 for-in 循环返回属性。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true。
 - [[Writable]]：表示能否修改属性的值。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 true。
 - [[Value]]：包含这个属性的数据值。读取属性值的时候，从这个位置读；写入属性值的时候，把新值保存在这个位置。这个特性的默认值为 undefined。

Object.defineProperty()
 - 这个方法接收三个参数：属性所在的对象、属性的名字和一个描述符对象。其中，描述符（descriptor）对象的属性必须是： configurable、 enumerable、 writable 和 value。设置其中的一或多个值，可以修改对应的特性值。
 - 把 configurable 设置为 false，表示不能从对象中删除属性。而且，一旦把属性定义为不可配置的，就不能再把它变回可配置了。
 - 若不指定configurable、 enumerable、 writable，默认均为false
 - 不要在 IE8 中使用 Object.defineProperty() 

###2.访问器属性
属性前面加`_`用于表示只能通过对象方法访问的属性

 - [[Configurable]]：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为数据属性。对于直接在对象上定义的属性，这个特性的默认值为true。
 - [[Enumerable]]：表示能否通过 for-in 循环返回属性。对于直接在对象上定义的属性，这个特性的默认值为 true。
 - [[Get]]：在读取属性时调用的函数。默认值为 undefined。
 - [[Set]]：在写入属性时调用的函数。默认值为 undefined。

Object.defineProperty()  IE9+
 - 使用访问器属性的常见方式，设置一个属性的值会导致其他属性发生变化。
 - 只指定 getter 意味着属性是不能写
 - 只指定 setter 函数的属性不能读

非标准方法
`__defineGetter__()`和`__defineSetter__()`

在 不 支 持 Object.defineProperty() 方 法 的 浏 览 器 中 不 能 修 改 [[Configurable]] 和[[Enumerable]]。

##6.1.2 定义多个属性
Object.defineProperties() (修改其他属性writable还是要写的。。。)
第一个对象是要添加和修改其属性的对象，第二个对象的属性与第一个对象中要添加或修改的属性一一对应

##6.1.3 读取属性的特性
Object.getOwnPropertyDescriptor()接收两个参数：属性所在的对象和要读取其描述符的属性名称
返回值是一个对象，如果是访问器属性，这个对象的属性有 configurable、 enumerable、 get 和 set；如果是数据属性，这个对象的属性有 configurable、enumerable、 writable 和 value。
