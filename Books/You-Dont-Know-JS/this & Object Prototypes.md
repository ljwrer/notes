# 第二部分 this和对象原型
# 第 1 章　关于this
## 1.1　为什么要用this
隐式“传递”一个对象引用
## 1.3　this到底是什么
this是在运行时进行绑定的，并不是在编写时绑定，它的上下文取决于函数调用时的各种条件。this的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式。
当一个函数被调用时，会创建一个活动记录（有时候也称为执行上下文）。这个记录会包含函数在哪里被调用（调用栈）、函数的调用方法、传入的参数等信息。this就是记录的其中一个属性，会在函数执行的过程中用到。
> 动态性

# 第 2 章　this全面解析
## 2.1　调用位置
>tips： 插入一条debugger;语句调试

## 2.2　绑定规则
### 2.2.1　默认绑定

 - strict
	 - this is undefined
 - 非strict
	 - this指向全局对象


### 2.2.2　隐式绑定
调用位置有上下文对象,对象属性引用链中只有最顶层或者说最后一层会影响调用位置
```js
function foo() { 
    console.log( this.a );
}

var obj2 = { 
    a: 42,
    foo: foo 
};

var obj1 = { 
    a: 2,
    obj2: obj2 
};

obj1.obj2.foo(); // 42
```
#### 隐式丢失
```js
var obj = { 
    a: 2,
    foo(){
		console.log(this.a)
	} 
};
setTimeout(obj.foo,1000);
```
>jQuery会把回调函数的this强制绑定到触发事件的DOM元素上

##### setTimeout内部
```js
function setTimeout(fn,delay) {
    // 等待delay毫秒
    fn(); // <-- 调用位置！
}
```

### 2.2.3 显式绑定
#### 硬绑定
```js
var bar = function() {
    foo.call( obj );
};
//or
var bar = function() {
    return foo.apply( obj, arguments );
};
//helper
function bind(fn, obj) { 
    return function() {
        return fn.apply( obj, arguments ); 
    };
}
```

### 2.2.4　new绑定
实际上并不存在所谓的“构造函数”，只有对于函数的“构造调用”
使用new来调用函数，或者说发生构造函数调用时，会自动执行下面的操作。

1. 创建（或者说构造）一个全新的对象。

2. 这个新对象会被执行[[原型]]连接。

3. 这个新对象会绑定到函数调用的this。

4. 如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象。

####判断this
判断this

现在我们可以根据优先级来判断函数在某个调用位置应用的是哪条规则。可以按照下面的顺序来进行判断：

1. 函数是否在new中调用（new绑定）？如果是的话this绑定的是新创建的对象。

	```
	var bar = new foo()
	```
2. 函数是否通过call、apply（显式绑定）或者硬绑定调用？如果是的话，this绑定的是指定的对象。

	```
	var bar = foo.call(obj2)
	
	```
3. 函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，this绑定的是那个上下文对象。

	```
	var bar = obj1.foo()
	```
4. 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到undefined，否则绑定到全局对象。

	```
	var bar = foo()
	```

####bind polyfill
```js
Function.prototype.bind = function (oThis) {
    if (typeof this !== "function") {
        // closest thing possible to the ECMAScript 5
        // internal IsCallable function
        throw new TypeError("Function.prototype.bind - what is trying to be bound is not callable");
    }

    var aArgs = Array.prototype.slice.call(arguments, 1),
        fToBind = this,
        fNOP = function () {},
        fBound = function () {
            let context,args;
            if(this instanceof fNOP){
                //when do new,this._proto_ instanceof fNOP
                //this.__proto__.__proto__ === fNOP.prototype
                context = this
            }else {
                // this is global now
                context = oThis || this
            }
            args = aArgs.concat(Array.prototype.slice.call(arguments));
            return fToBind.apply(context,args)
        };

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();

    return fBound;
};
```
## 2.4　绑定例外
### 2.4.1　被忽略的this
bind(null)
call(null)
apply(null)
this绑定到全局，不安全

>ES6 foo(...[1,2])=foo(1,2)=foo.apply(null,[1,2])

#### 安全的this
```js
const ø = Object.create( null );
foo.apply( ø, [2, 3] );
``` 

### 2.4.3　软绑定
```js
Function.prototype.bindSoft = function (oThis) {
    const fToBind = this;
    return function () {
        let context;
        if (!this || this === global) {
            context = oThis
        } else {
			//之后会绑定新的this
            context = this
        }
        return fToBind.apply(context)
    };
};
Function.prototype.bindHard = function (oThis) {
    const fToBind = this;
    return function () {
		//之后的绑定和this无关了
        return fToBind.apply(oThis)
    };
};
```

##箭头函数
类似 const self = this