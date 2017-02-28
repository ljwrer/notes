# javascript精粹
# 第1章 精华
```js
Function.prototype.method = function (name, func) {
    this.prototype[name] = func;
    return this;
}
```

# 第2章 语法
## 数字
###double 64bit
```
const a = 1;
const b = 1.000;
console.log(a===b) // true
```
###NaN
```
isNaN(NaN) //true
```

## 字符串

- unicode-16
- \转义
- \指定数字字符编码
	- "A" === "\u0041"

## 语句
### 假值

 - null
 - undefined
 - ''
 - 0
 - NaN
 - false

## 检索
>||填充默认值
```
const status = flight.status || "unknown"
```

## 反射
```
Object.prototype.hasOwnProperty.call
```

## 全局变量
```
YUI = {};
YUI_CONFIG = {};
```

# 第4章 函数
## 函数对象
```
const foo = function(){};
foo.prototype.constructor === foo //true 
```

## 调用
### 附加参数this,arguments
#### this

 - 方法调用
	 - 调用对象
 - 函数调用
	 - 全局
 - 构造器调用
	 - 创建对象
 - apply调用
	 - 指定

####arguments
```
const foo = function(...args){
	
}
```

## 异常
error.\_protp\_

 - @prop message
 - @prop name

## 扩充

```
Function.prototype.method = function (name, func) {
    if (!this.prototype[name]) {
        this.prototype[name] = func;
    }
    return this
};
```

## 递归

 - 汉诺塔问题
 - 尾递归优化

## 闭包
除this和arguments

## 函数式

 - 模块
 - 级联
 - 柯里化
 - 记忆

```
Function.method('curry', function (  ) {
    var slice = Array.prototype.slice,
        args = slice.apply(arguments),
        that = this;
    return function (  ) {
        return that.apply(null, args.concat(slice.apply(arguments)));
    };
});

var memoizer = function (memo, fundamental) {
    var shell = function (n) {
        var result = memo[n];
        if (typeof result !== 'number') {
            result = fundamental(shell, n);
            memo[n] = result;
        }
        return result;
    };
    return shell;
};
```

# 第5章 继承
```
//DC式继承
var constructor = function (spec, my) {
    var that, other private instance variables;
    my = my || {};

    Add shared variables and functions to my

    that = a new object;

    Add privileged methods to that

    return that;
}

var mammal = function (spec) {
    var that = {};

    that.get_name = function (  ) {
        return spec.name;
    };

    that.says = function (  ) {
        return spec.saying || '';
    };

    return that;
};

var myMammal = mammal({name: 'Herb'});
    
    
====================================
var cat = function (spec) {
    spec.saying = spec.saying || 'meow';
    var that = mammal(spec);
    that.purr = function (n) {
        var i, s = '';
        for (i = 0; i < n; i += 1) {
            if (s) {
                s += '-';
            }
            s += 'r';
        }
        return s;
    };
    that.get_name = function (  ) {
        return that.says(  ) + ' ' + spec.name +
                ' ' + that.says(  );
    return that;
};

var myCat = cat({name: 'Henrietta'});
    
    
====================================
Object.method('superior', function (name) {
    var that = this,
        method = that[name];
    return function (  ) {
        return method.apply(that, arguments);
    };
});
    
    
====================================
var coolcat = function (spec) {
    var that = cat(spec),
        super_get_name = that.superior('get_name');
    that.get_name = function (n) {
        return 'like ' + super_get_name(  ) + ' baby';
    };
    return that;
};

var myCoolCat = coolcat({name: 'Bix'});
var name = myCoolCat.get_name(  );
```
