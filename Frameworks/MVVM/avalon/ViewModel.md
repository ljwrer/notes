#2. 模块化、ViewModel、作用域
ms-controller--ViewModel的ID

操作数据即操作DOM

 - 监控属性：就是改变了它会同步视图的属性；
 - 非监控属性：就是改变了它不会同步视图的属性，通常是以$开头，或放在$skipArray数组的属性（angular到1.3才引入单向数据绑定）；
 - 计算属性：就是一个定义set, get方法的对象，是一种高级的监控属性；
 - 监控数组：就是一个数组，如果它没有以$开头，或名字没有放到$skipArray数组里，框架就会自动转换它为监控数组，当用户调用它的方法时，就会同步视图通常它是与ms-repeat、ms-each指令配合使用。

VM通过劫持Model访问器属性达到修改View的目的

ViewModel在DOM树的作用范围其实与CSS很相似，采取就近原则
	
	 ms-controller="model name"
	 ms-important="model name" !important
	 ms-skip
因为{{}}也算一种指令，而任何指令在被扫描后都会被移除，如果我们想保留某个区域的{{}}，就需要用到ms-skip

ViewModel属性

 - $id： VM的ID，方便在avalon.vmodels里查找到它，或用在ms-controller、ms-important上。
 - $events：里面存放着各种回调，它们是通过$watch方法添加的。
 - $watch：这是一个方法，有两个参数，第一个是VM中的某一个属性名，只能这个VM的直接子属性名，第二个是回调函数，当此属性发生改变时，就会执行此回调。回调里会依次传入它的新老属性值。
 - $unwatch：移除某个属性的回调。
 - $fire：手动触发此回调。
 - $accessors：放置与监听属性相连动的视图刷新函数，当我们改变某一属性时，框架就会在这里找到对应的视图刷新函数，传入当前值，实现对视图的同步。
 - $123323213：它的格式是$加上一串数字，它是用于放置监控数组的视图刷新函数，当我们调用监控数组的方法时，框架就此根据当前数组的个数与排列顺序，重新渲染对应的区域。它与$accessors一样，不开放给用户调用的。
 - $model：就是ViewModel的净化版，没有$XXX属性，访问器属性全部还原为普通属性，专门用于提交到后台用。当然我们提交后台，还需要用JSON.parse(JSON.stringify(VM.$model))处理一下，将里面的函数干掉。

3、绑定属性与扫描机制

指令：

 - ms-* 小写
 - {{}}  