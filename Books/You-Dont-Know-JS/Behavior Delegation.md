# 第 6 章　行为委托
## 对象关联 OLOO
 - 尽量避免在[[Prototype]]链的不同级别中使用相同的命名
 - 在API接口的设计中，委托最好在内部实现

![](https://raw.githubusercontent.com/ljwrer/You-Dont-Know-JS/master/this%20%26%20object%20prototypes/fig4.png)
---
![](https://raw.githubusercontent.com/ljwrer/You-Dont-Know-JS/master/this%20%26%20object%20prototypes/fig5.png)
---
![](https://raw.githubusercontent.com/ljwrer/You-Dont-Know-JS/master/this%20%26%20object%20prototypes/fig6.png)

## 6.4　更好的语法
```
Object.setPrototypeOf( AuthController, LoginController );
```
```
var Foo = {
    bar: function(x) {
        if(x<10){
           return Foo.bar( x * 2 );
        }
        return x; 
    },
    baz: function baz(x) { 
        if(x < 10){
           return baz( x * 2 ); 
        }
        return x; 
    }
};
```
>如果你需要自我引用的话，那最好使用传统的具名函数表达式来定义对应的函数（·baz: function baz(){..}·），不要使用简洁方法

### 自省
```
Object.prototype.isPrototypeOf()
Object.getPrototypeOf()
```

# ES6 Class
>传统面向类的语言中父类和子类、子类和实例之间其实是复制操作，但是在[[Prototype]]中并没有复制，相反，它们之间只有委托关联。

## A.2　class陷阱
class并不会像传统面向类的语言一样在声明时静态复制所有行为。如果你（有意或无意）修改或者替换了父“类”中的一个方法，那子“类”和所有实例都会受到影响，因为它们在定义时并没有进行复制，只是使用基于[[Prototype]]的实时委托

class语法无法定义类成员属性（只能定义方法）

class语法仍然面临意外屏蔽的问题

super并不是动态绑定的，它会在声明时“静态”绑定
```
class P {
	foo() { console.log( "P.foo" ); }
}

class C extends P {
	foo() {
		super();
	}
}

var c1 = new C();
c1.foo(); // "P.foo"

var D = {
	foo: function() { console.log( "D.foo" ); }
};

var E = {
	foo: C.prototype.foo
};

// Link E to D for delegation
Object.setPrototypeOf( E, D );

E.foo(); // "P.foo"
此处C 的 super 已经固定为 P
若为动态，则E的foo调用super为D
```


>如果你使用.bind(..)函数来硬绑定函数，那么这个函数不会像普通函数那样被ES6的extend扩展到子类中。