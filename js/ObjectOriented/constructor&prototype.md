#JavaScript面向对象

 - constructor
 - this
 - 原型继承
 - 原型链

---
#constructor
对象构造器，对象类型
 - new 
 - 对象字面量

##自定义constructor

 - 使用函数
 - this创建属性
 - 函数类型的属性即方法
 - 使用new 创建对象
 - 先创建构造函数类型的对象，然后用参数执行构造器函数

###创建构造器的三种方法：
 - 函数定义
 - 函数表达式
 - new Function()

###不是所有的函数都可以都可以当成constructor
如Math.min(),parseInt(),但一般自定义函数和内置构造器都可以当cosntructor使用

###如果构造器有返回值
使用new关键字创建对象时，如果构造函数返回值是对象类型,直接将返回的对象当作新建的对象

---
#this

 - 全局环境中的this就是全局对象，在浏览器即window
 - 构造器中的this就是新创建出来的对象
 - 函数中的this指向函数的调用者
 - new Function中的this指全局对象
 - eval中的this指调用上下文中的this

##apply、	call
改变调用方法中的this

---
#原型
将对象共享的部分公共出来
##prototype
 - 构造器的属性，就是对象的原型`__proto__`
 - 实现原型继承
 - 将公共的属性和方法放到原型中，当对象找不到属性和方法时，就去原型链查找

---
#原型链

 - 构造器也是Function类型的实例，因此`__proto__`指向Function.prototype 从而继承了apply()、call()等方法
 - 构造器的原型同时也是Object类型的实例，因此`__proto__`指向Object.prototype 从而继承了valueOf()、hasOwnProperty()等方法
 - obj---constuctor.prototype---obj.prototype构成原型链
 - 对象属性的查找，修改，删除都是通过原型链进行的

##属性操作
###属性查找
顺着原型链查找，先查找自身，找不到则顺着原型链查找
###属性修改
永远都是修改自身的属性(引用类型除外)
本身没有则增加属性
通过obj[property]是无法修改到原型上的属性值
通过cosntructor.prototype[property]，可以修改原型属性，但是会影响到所有创建出来的对象
###属性删除
与属性修改类似，使用delete永远都是删除自身的属性，无法删除原型上的属性
###判断属性是否来自对象本身
hasOwnProperty()

---
#ES5中的原型继承
Object.create(proto[,propertiesObject])
直接创建一个对象，并指定原型，并添加额外的属性

Function F()
F.prototype=proto
return new F();