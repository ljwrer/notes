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

#6.2 创建对象
避免产生大量的重复代码

##6.2.1 工厂模式

	function createPerson(name, age, job){
		var o = new Object();
		o.name = name;
		o.age = age;
		o.job = job;
		o.sayName = function(){
		alert(this.name);
		};
		return o;
	}
没有解决对象识别的问题

##6.2.2 构造函数模式

	function Person(name, age, job){
		this.name = name;
		this.age = age;
		this.job = job;
		this.sayName = function(){
		alert(this.name);
		};
	}

 - 没有显式地创建对象；
 - 直接将属性和方法赋给了 this 对象；
 - 没有 return 语句。
 - 以这种方式定义的构造函数是定义在 Global 对象中的

 `new` 操作符：

 1. 创建一个新对象；
 2. 将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）；
 3. 执行构造函数中的代码（为这个新对象添加属性）；
 4. 返回新对象。

<!-- -->
 - 使用new操作符创建的对象将获得constructo(构造函数)属性
 - 可以使用instanceof识别对象类型
 - 所有对象均继承自 Object

###1. 将构造函数当作函数
 - 任何函数，只要通过 new 操作符来调用，那它就可以作为构造函数；
 - 任何函数，如果不通过 new 操作符来调用，那它跟普通函数也不会有什么两样。
 - 只是this指向的问题

###2. 构造函数的问题
 - 每个方法都要在每个实例上重新创建一遍，导致不同的作用域链和标识符解析，但创建 Function 新实例的机制仍然是相同的。因此，不同实例上的同名函数是不相等的  
 - 把函数的定义转移到了构造函数外部，在全局作用域中定义的函数实际上只能被某个对象调用，要定义很多个全局函数，没有封装性

##6.2.3 原型模式
 - 每个函数都有一个 prototype（原型）属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法
 - 使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法

###1. 理解原型对象
 - 只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个 prototype属性，这个属性指向函数的原型对象
 - 在默认情况下，所有原型对象都会自动获得一个 constructor（构造函数）属性，这个属性包含一个指向 prototype 属性所在函数的指针
	 - Function.prototype.constructor=Function
 - 当调用构造函数创建一个新实例后，该实例的内部将包含一个指针[[Prototype]]（内部属性），指向构造函数的原型对象,使用_proto_访问
 	- instance._proto_=Function.prototype
 - 实例与构造函数没有直接关系，通过prototype访问
![](http://7xkcnd.com1.z0.glb.clouddn.com/prototype.png)

`isPrototypeOf()`：Function.prototype.isPrototypeOf(instance)

ES5(IE9+):`Object.getPrototypeOf()`,在利用原型实现继承时常用，与_proto_类似

 - 不能通过对象实例重写原型中的值
 - 当为对象实例添加一个属性时，这个属性就会屏蔽原型对象中保存的同名属性
 - 使用 delete 操作符可以完全删除实例属性，从而让我们能够重新访问原型中的属性

`hasOwnProperty()`:检测一个属性是存在于实例中，还是存在于原型中

ES5:`Object.getOwnPropertyDescriptor`()方法只能用于实例属性，要取得原型属性的描述符，必须直接在原型对象上调用 Object.getOwnPropertyDescriptor()方法。

###2. 原型与 in 操作符
无论属性存在于实例中还是存在于原型中,in都会返回true

	function hasPrototypeProperty(object, name){
		return !object.hasOwnProperty(name) && (name in object);
	}

 - 在使用 for-in 循环时，返回的是所有能够通过对象访问的、可枚举的（enumerated）属性
 - 既包括存在于实例中的属性，也包括存在于原型中的属性。
 - 屏蔽了原型中不可枚举属性（即将[[Enumerable]]标记为 false 的属性）的实例属性也会在 for-in 循环中返回，因为根据规定，所有开发人员定义的属性都是可枚举的——只有在 IE8 及更早版本中例外。
 - 该 bug 会影响默认不可枚举的所有属性和方法，包括：hasOwnProperty()、propertyIsEnumerable()、toLocaleString()、toString()和 valueOf()

ES5（IE9+）: `Object.keys()`接收一个对象作为参数，返回一个包含所有可枚举属性的字符串数组

ES5（IE9+）:`Object.getOwnPropertyNames()`接收一个对象作为参数，返回一个包含所有实例属性的字符串数组，无论它是否可枚举,包括constructor

###3. 更简单的原型语法

	ConFunc.prototype={};
 - 重写了默认的 prototype 对象
 - constructor 属性也就变成了新对象的 constructor 属性 （指向 Object 构造函数），不再指向 ConFunc 函数
 - instanceof操作符能返回正确的结果，但 constructor 无法确定对象的类型

重设constructor，会导致它的[[Enumerable]]特性被设置为 true

	ConFunc.prototype={
		constructor：ConFunc
	};

ES5:

	//重设构造函数，只适用于 ECMAScript 5 兼容的浏览器
	Object.defineProperty(ConFunc.prototype, "constructor", {
		enumerable: false,
		value: ConFunc
	});

###4. 原型的动态性
 - 由于在原型中查找值的过程是一次搜索，因此我们对原型对象所做的任何修改都能够立即从实例上反映出来——即使是先创建了实例后修改原型也照样如此。
 - 把原型修改为另外一个对象就等于切断了构造函数与最初原型之间的联系。
 - 实例中的指针仅指向原型，而不指向构造函数
 - 重写原型对象切断了现有原型与任何之前已经存在的对象实例之间的联系；它们引用的仍然是最初的原型。

![](http://7xkcnd.com1.z0.glb.clouddn.com/prototype2.png)

###5. 原生对象的原型
- 所有原生引用类型（Object、 Array、 String，等等）都在其构造函数的原型上定义了方法
- 通过原生对象的原型，不仅可以取得所有默认方法的引用，而且也可以定义新方法
- 不推荐在产品化的程序中修改原生对象的原型

###6. 原型对象的问题
 - 省略了为构造函数传递初始化参数这一环节，结果所有实例在默认情况下都将取得相同的属性值
 - 原型中所有属性是被很多实例共享的
 - 函数与基本类型问题不大
 - ！修改引用类型将反映到所有的实例中（注意是修改不是重写），此问题导致很少单独使用原型模式

##6.2.4 组合使用构造函数模式和原型模式
 - 创建自定义类型的最常见方式,默认模式
 - 构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性
 - 每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用
 - 支持向构造函数传递参数


		function Person(name, age, job){
			this.name = name;
			this.age = age;
			this.job = job;
			this.friends = ["Shelby", "Court"];
		}
		Person.prototype = {
			constructor : Person,
			sayName : function(){
				alert(this.name);
			}
		}

##6.2.5 动态原型模式
 - 把所有信息都封装在了构造函数
 - 通过在构造函数中初始化原型
 - 仅在第一次new时定义原型对象
 - 使用动态原型模式时，不能使用对象字面量重写原型

		function Person(name, age, job){
			//属性
			this.name = name;
			this.age = age;
			this.job = job;
			//方法
			if (typeof this.sayName != "function"){
				Person.prototype.sayName = function(){
					alert(this.name);
				};
			}
		}

##6.2.6 寄生构造函数模式
 - 封装创建对象的代码，然后再返回新创建的对象
 - 像是典型的构造函数
 - 内部和工厂模式一致
 - 使用new
 - instance prototype constructor 全部失效

		function Person(name, age, job){
			var o = new Object();
			o.name = name;
			o.age = age;
			o.job = job;
			o.sayName = function(){
				alert(this.name);
			};
			return o;
		}
 - 需要修改原生类型时使用
 - 在可以使用其他模式的情况下，不要使用这种模式。

##6.2.7 稳妥构造函数模式
稳妥对象（durable objects）：

 - 没有公共属性，而且其方法也不引用 this 的对象
 - 最适合在安全的环境中（这些环境中会禁止使用 this 和 new），或者在防止数据被其他应用程序（如 Mashup程序）改动时使用。
 - 创建对象的实例方法不引用 this
 - 不使用 new 操作符调用构造函数
 - instance prototype constructor 全部失效

		function Person(name, age, job){
			//创建要返回的对象
			var o = new Object();
			//可以在这里定义私有变量和函数
			//添加方法
			o.sayName = function(){
				alert(name);
			};
			//返回对象
			return o;
		}
 - 除了内部方法，没有别的方式可以访问其数据成员
 - 内部方法使用闭包访问变量

#6.3 继承
ECMAScript 中无法实现接口继承。 ECMAScript 只支持实现继承，而且其实现继承主要是依靠原型链来实现的

##6.3.1 原型链
利用原型让一个引用类型继承另一个引用类型的属性和方法。实现的本质是重写原型对象，让原型对象等于另一个类型的实例。

	function SuperType() {
		this.prop = true;
	}
	SuperType.prototype.getValue = function() {
		return this.prop;
	}
	
	function SubType() {
		this.subprop = false;
	}
	SubType.prototype = new SuperType();
	SubType.prototype.constructor = SubType;
	SubType.prototype.getSubValue = function() {
		return this.subprop;
	}
	alert(new SubType().getValue());
	alert(new SubType().getSubValue())
![](http://7xkcnd.com1.z0.glb.clouddn.com/prototypechain.png)

 - 将SubType 原型设为SuperType 的实例。于是，新原型不仅具有作为一个 SuperType 的实例所拥有的全部属性和方法，而且其内部还有一个指针，指向了 SuperType 的原型。
 - instance.constructor将会指向SuperType
 - 搜索顺序
	 1. 搜索实例
	 2. 搜索 SubType.prototype
	 3. 搜索 SuperType.prototype

###1. 默认的原型
 - 所有引用类型默认都继承了 Object，而这个继承也是通过原型链实现的。
 - 所有函数的默认原型都是 Object 的实例，因此默认原型都会包含一个内部指针，指向 Object.prototype。

![](http://7xkcnd.com1.z0.glb.clouddn.com/prototypechainobject.png)

###2. 确定原型和实例的关系
 -  instanceof 测试实例与原型链中出现过的构造函数
 -  isPrototypeOf()方法。只要是原型链中出现过的原型，都可以说是该原型链所派生的实例的原型

###3. 谨慎地定义方法
 - 给原型添加方法的代码一定要放在替换原型的语句之后,否则添加的方法会丢失
 - 通过原型链实现继承时，不能使用对象字面量创建原型方法。因为这样做就会重写原型链，切断SubType 和 SuperType之间的原型链

###4. 原型链的问题
 - 引用类型值的原型属性会被所有实例共享，与原型模式创建对象有同样的问题，修改原型中的引用类型会动态反映到所有实例中
 - 没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数
 - 实践中很少会单独使用原型链

##6.3.2 借用构造函数
 - 在子类型构造函数的内部调用超类型构造函数。通过使用 apply()和 call()方法也可以在（将来）新创建的对象上执行构造函数。
 - 在新 SubType 对象上执行 SuperType()函数中定义的所有对象初始化代码。SubType 的每个实例就都会具有自己的属性副本

		function SuperType() {
			this.colors = ["red", "blue", "green"];
		}
		
		function SubType() {
			//继承了 SuperType
			SuperType.call(this);
		}
		var instance1 = new SubType();
		instance1.colors.push("black");
		alert(instance1.colors); //"red,blue,green,black"
		var instance2 = new SubType();
		alert(instance2.colors); //"red,blue,green"
###1. 传递参数
为了确保SuperType 构造函数不会重写子类型的属性，在调用超类型构造函数后，再添加应该在子类型中定义的属性。
###2. 借用构造函数的问题
- 无法避免构造函数模式存在的问题，函数无法复用
- 超类型的原型中定义的方法，对子类型而言也是不可见的，结果所有类型都只能使用构造函数模式

##6.3.3 组合继承
 - 使用原型链实现对原型属性和方法的继承
 - 通过借用构造函数来实现对实例属性的继承
 - 既通过在原型上定义方法实现了函数复用，又能够保证每个实例都有它自己的属性

		function SuperType(name){
			this.name=name;
			this.color=["red","blue","green"];
		}
		SuperType.prototype.sayName=function(){
			alert(this.name);
		}
		function SubType(name,age){
			//继承属性
			SuperType.call(this,name);
			this.age=age;
		}
		//继承方法
		SubType.prototype=new SuperType();
		SubType.prototype.constructor=SubType;
		SubType.prototype.sayAge=function(){
			alert(this.age);
		}
		var ins1=new SubType("Ray",18);
		ins1.color.push("yellow");
		ins1.sayAge();
		ins1.sayName();
		alert(ins1.color);
		var ins2=new SubType("Rmaiy",24);
		ins2.sayAge();
		ins2.sayName();
		alert(ins2.color);

组合继承避免了原型链和借用构造函数的缺陷，融合了它们的优点，成为 JavaScript 中最常用的继承模式。而且， instanceof 和 isPrototypeOf()也能够用于识别基于组合继承创建的对象

##6.3.4 原型式继承
借助原型可以基于已有的对象创建新对象，类似浅拷贝

	function object(o) {
		function F() {}
		F.prototype = o;
		return new F();
	}
	var person = {
		name: "Nicholas",
		friends: ["Shelby", "Court", "Van"]
	};
	var anotherPerson = object(person);
	anotherPerson.name = "Greg";
	anotherPerson.friends.push("Rob");
	var yetAnotherPerson = object(person);
	yetAnotherPerson.name = "Linda";
	yetAnotherPerson.friends.push("Barbie");
	alert(person.friends); //Shelby,Court,Van,Rob,Barbie
	alert(anotherPerson.friends);
	alert(yetAnotherPerson.friends)
	alert(person.name); 
	alert(anotherPerson.name);
	alert(yetAnotherPerson.name)

ES5(IE9+) `Object.create()`接收两个参数：

 - 一个用作新对象原型的对象
 - （可选的）一个为新对象定义额外属性的对象,第二个参数与Object.defineProperties()方法的第二个参数格式相同
<!-- -->
 - 只想让一个对象与另一个对象保持类似的情况下，原型式继承是完全可以胜任的。
 - 包含引用类型值的属性始终都会共享相应的值，就像使用原型模式一样。

##6.3.5 寄生式继承
与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后再像真地是它做了所有工作一样返回对象

	function createAnother(original){
		var clone = object(original); //通过调用函数创建一个新对象
		clone.sayHi = function(){ //以某种方式来增强这个对象
			alert("hi");
		};
		return clone; //返回这个对象
	}

 - 主要考虑对象而不是自定义类型和构造函数的情况下
 - 不能做到函数复用

##6.3.6 寄生组合式继承
组合继承的缺点：

 - 调用两次超类型构造函数：一次是在创建子类型原型的时候，另一次是在子类型构造函数内部。
 - 子类型最终会包含超类型对象的全部实例属性，不得不在调用子类型构造函数时重写这些属性。

		function SuperType(name) {
			this.name = name;
			this.colors = ["red", "blue", "green"];
		}
		SuperType.prototype.sayName = function() {
			alert(this.name);
		};
		
		function SubType(name, age) {
			SuperType.call(this, name); //第二次调用 SuperType()
			this.age = age;
		}
		SubType.prototype = new SuperType(); //第一次调用 SuperType()
		SubType.prototype.constructor = SubType;
		SubType.prototype.sayAge = function() {
			alert(this.age);
		};
 - 在第一次调用 SuperType 构造函数时，SubType.prototype 会得到两个属性： name 和 colors；它们都是 SuperType 的实例属性，只不过现在位于 SubType 的原型中。
 - 当调用 SubType 构造函数时，又会调用一次 SuperType 构造函数，这一次又在新对象上创建了实例属性 name 和 colors。于是，这两个属性就屏蔽了原型中的两个同名属性。
 - 有两组 name 和 colors 属性：一组在实例上，一组在 SubType 原型中

![](http://7xkcnd.com1.z0.glb.clouddn.com/CombinationInheritance.png)

SubType.prototype 并不需要调用SuperType的构造函数得到SuperType的实例，而且这样会导致数据重复，只需要一份SuperType.prototype的副本即可。

---

寄生组合继承的实现：

	function inheritPrototype(subType, superType){
		var prototype = object(superType.prototype); //创建对象
		prototype.constructor = subType; //增强对象
		subType.prototype = prototype; //指定对象
	}
 1. 创建超类型原型的一个副本。
 2. 为创建的副本添加 constructor 属性，从而弥补因重写原型而失去的默认的 constructor 属性
 3. 将新创建的对象（即副本）赋值给子类型的原型。

其他部分和组合继承一致：

	function SuperType(name) {
		this.name = name;
		this.colors = ["red", "blue", "green"];
	}
	SuperType.prototype.sayName = function() {
		alert(this.name);
	};

	function SubType(name, age) {
		SuperType.call(this, name);
		this.age = age;
	}
	inheritPrototype(SubType, SuperType);//只需SuperType.prototype的副本即可，无需SuperType实例
	SubType.prototype.sayAge = function() {
		alert(this.age);
	};

 - 只调用了一次 SuperType 构造函数，并且因此避免了在 SubType.prototype 上面创建不必要的、多余的属性。
 - 原型链还能保持不变，能够正常使用instanceof 和 isPrototypeOf()。
 - 寄生组合式继承是引用类型最理想的继承范式。

两种继承方式在方法继承上的差异：
![](http://7xkcnd.com1.z0.glb.clouddn.com/InheritanceDiff.png)

#6.4 小结
ECMAScript 支持面向对象（OO）编程，但不使用类或者接口。对象可以在代码执行过程中创建和增强，因此具有动态性而非严格定义的实体。在没有类的情况下，可以采用下列模式创建对象。

 - 工厂模式，使用简单的函数创建对象，为对象添加属性和方法，然后返回对象。这个模式后来被构造函数模式所取代。
 - 构造函数模式，可以创建自定义引用类型，可以像创建内置对象实例一样使用 new 操作符。不过，构造函数模式也有缺点，即它的每个成员都无法得到复用，包括函数。由于函数可以不局限于任何对象（即与对象具有松散耦合的特点），因此没有理由不在多个对象间共享函数。
 - 原型模式，使用构造函数的 prototype 属性来指定那些应该共享的属性和方法。组合使用构造函数模式和原型模式时，使用构造函数定义实例属性，而使用原型定义共享的属性和方法。


JavaScript 主要通过原型链实现继承。原型链的构建是通过将一个类型的实例赋值给另一个构造函数的原型实现的。这样，子类型就能够访问超类型的所有属性和方法，这一点与基于类的继承很相似。  
原型链的问题是对象实例共享所有继承的属性和方法，因此不适宜单独使用。解决这个问题的技术是借用构造函数，即在子类型构造函数的内部调用超类型构造函数。这样就可以做到每个实例都具有自己的属性，同时还能保证只使用构造函数模式来定义类型。使用最多的继承模式是组合继承，这种模式使用原型链继承共享的属性和方法，而通过借用构造函数继承实例属性。  

此外，还存在下列可供选择的继承模式。

 - 原型式继承，可以在不必预先定义构造函数的情况下实现继承，其本质是执行对给定对象的浅复制。而复制得到的副本还可以得到进一步改造。
 - 寄生式继承，与原型式继承非常相似，也是基于某个对象或某些信息创建一个对象，然后增强对象，最后返回对象。为了解决组合继承模式由于多次调用超类型构造函数而导致的低效率问题，可以将这个模式与组合继承一起使用。
 - 寄生组合式继承，集寄生式继承和组合继承的优点与一身，是实现基于类型继承的最有效方式。

---
