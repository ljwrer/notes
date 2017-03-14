# 第 5 章　原型
## 5.1　[[Prototype]]
### 5.1.2　属性设置和屏蔽
如果foo不直接存在于myObject中而是存在于原型链上层时myObject.foo = "bar"会出现的三种情况:
1. 如果在[[Prototype]]链上层存在名为foo的普通数据访问属性并且没有被标记为只读（writable:false），那就会直接在myObject中添加一个名为foo的新属性，它是屏蔽属性。

2. 如果在[[Prototype]]链上层存在foo，但是它被标记为只读（writable:false），那么无法修改已有属性或者在myObject上创建屏蔽属性。如果运行在严格模式下，代码会抛出一个错误。否则，这条赋值语句会被忽略。总之，不会发生屏蔽。
	- Object.defineProperty不受此影响

3. 如果在[[Prototype]]链上层存在foo并且它是一个setter（参见第3章），那就一定会调用这个setter。foo不会被添加到（或者说屏蔽于）myObject，也不会重新定义foo这个setter。

### !隐式屏蔽 (禁用++操作符)
```
var anotherObject = { 
    a:2
};

var myObject = Object.create( anotherObject ); 

anotherObject.a; // 2
myObject.a; // 2

anotherObject.hasOwnProperty( "a" ); // true
myObject.hasOwnProperty( "a" ); // false

myObject.a++; // 隐式屏蔽！

anotherObject.a; // 2 
myObject.a; // 3

myObject.hasOwnProperty( "a" ); // true
```

## 5.3　（原型）继承
```
// ES6之前需要抛弃默认的Bar.prototype
Bar.ptototype = Object.create( Foo.prototype );

// ES6开始可以直接修改现有的Bar.prototype
Object.setPrototypeOf( Bar.prototype, Foo.prototype );
```
#### 检查“类”关系
instanceof ：在a的整条[[Prototype]]链中是否有指向Foo.prototype的对象

如果使用内置的.bind(..)函数来生成一个硬绑定函数的话，该函数是没有.prototype属性的。在这样的函数上使用instanceof的话，目标函数的.prototype会代替硬绑定函数的.prototype。
>通常我们不会在“构造函数调用”中使用硬绑定函数，不过如果你这么做的话，实际上相当于直接调用目标函数。同理，在硬绑定函数上使用instanceof也相当于直接在目标函数上使用instanceof。

```js
//访问（获取值）a.__proto__时，实际上是调用了a.__proto__(),this指向a
Object.defineProperty( Object.prototype, "__proto__", { 
    get: function() {
        return Object.getPrototypeOf( this ); 
    },
    set: function(o) {
        // ES6中的setPrototypeOf(..)
        Object.setPrototypeOf( this, o );
        return o;
    } 
} );
```

## 5.4　对象关联
```
if (!Object.create) { 
    Object.create = function(o) {
        function F(){} 
        F.prototype = o; 
        return new F();
    }; 
}
```
### 5.4.2　关联关系是备用
```
var anotherObject = { 
    cool: function() {
        console.log( "cool!" );
    }
};

var myObject = Object.create( anotherObject );

myObject.doCool = function() { 
    this.cool(); // 内部委托！
};

myObject.doCool(); // "cool!"
```