# 第 1 章 异步：现在与将来
# 1.1　分块的程序
代码运行的时候，浏览器可能会认为需要把控制台 I/O 延迟到后台，在这种情况下，等到浏览器控制台输出对象内容时，后续代码可能已经执行，因此会显示不一致。
>如果遇到这种少见的情况，最好的选择是在 JavaScript 调试器中使用断点，而不要依赖控制台输出。次优的方案是把对象序列化到一个字符串中，以强制执行一次“快照”，比如通过 JSON.stringify(..)。

# 1.5　任务
 - Macrotask 包括:
	 - setImmediate
	 - setTimeout
	 - setInterval
 - Microtask 包括:
	 - process.nextTick
	 - Promise
	 - Object.observe
	 - MutaionObserver
	 - MutaionObserver

```js
for (macroTask of macroTaskQueue) {
    // 1. Handle current MACRO-TASK
    handleMacroTask();
    // 2. Handle all NEXT-TICK
    for (nextTick of nextTickQueue) {
        handleNextTick(nextTick);
    }
    // 3. Handle all MICRO-TASK
    for (microTask of microTaskQueue) {
        handleMicroTask(microTask);
    }
}
```

# 第 2 章 回调
## 2.3　信任问题
控制反转（inversion of control）， 也就是把自己程序一部分的执行控制交给某个第三方。在你的代码和第三方工具（一组你希望有人维护的东西）之间有一份并没有明确表达的契约。

 - 调用回调过早（在追踪之前）；
 - 调用回调过晚（或没有调用）；
 - 调用回调的次数太少或太多（就像你遇到过的问题！）；
 - 没有把所需的环境 / 参数成功传给你的回调函数；
 - 吞掉可能出现的错误或异常；

### 2.3.2　不只是别人的代码
类型的检查 / 规范化的过程对于函数输入是很常见的，即使是对于理论上完全可以信任的代码。大体上说，这等价于那条地缘政治原则：“信任，但要核实。

## 2.4　省点回调
分离回调（一个用于成功通知，一个用于出错通知）
“error-first 风格”（有时候也称为“Node 风格”，因为几乎所有 Node.js API 都采用这种风格），其中回调的第一个参数保留用作错误对象（如果有的话）。如果成功的话，这个参数就会被清空 / 置假（后续的参数就是成功数据）。不过，如果产生了错误结果，那么第一个参数就会被置起 / 置真（通常就不会再传递其他结果）

#### 超时信任问题
```js
function timeoutify(fn, delay) {
    var intv = setTimeout(function () {
        intv = null;
        fn(new Error("Timeout!"));
    }, delay);
    return function () {
        // 还没有超时？
        if (intv) {
            clearTimeout(intv);
            fn.apply(this, arguments);
        }
    };
}

// 使用"error-first 风格" 回调设计
function foo(err, data) {
    if (err) {
        console.error(err);
    } else {
        console.log(data);
    }
}

ajax("http://some.url.1", timeoutify(foo, 500));

```
#### 永远要异步
```js
function asyncify(fn) {
    var orig_fn = fn,
        intv = setTimeout(function () {
            intv = null;
            if (fn) fn();
        }, 0);
    fn = null;
    return function () {
        // 触发太快，在定时器intv触发指示异步转换发生之前？
        if (intv) {
            fn = orig_fn.bind.apply(
                orig_fn,
                // 把封装器的this添加到bind(..)调用的参数中，
                // 以及克里化（currying）所有传入参数
                [this].concat([].slice.call(arguments))
            );
        }
        // 已经是异步
        else {
        // 调用原来的函数
            orig_fn.apply(this, arguments);
        }
    };
}

function result(data) {
    console.log(a);
}

var a = 0;
ajax("..pre-cached-url..", asyncify(result));
a++;
```
第一，大脑对于事情的计划方式是线性的、阻塞的、单线程的语义，但是回调表达异步流
程的方式是非线性的、非顺序的，这使得正确推导这样的代码难度很大。难于理解的代码
是坏代码，会导致坏 bug。
我们需要一种更同步、更顺序、更阻塞的的方式来表达异步，就像我们的大脑一样。
第二，也是更重要的一点，回调会受到控制反转的影响，因为回调暗中把控制权交给第三
方（通常是不受你控制的第三方工具！）来调用你代码中的 continuation。这种控制转移导
致一系列麻烦的信任问题，比如回调被调用的次数是否会超出预期。

# 第 3 章 回调
## 3.1　什么是 Promise
### 3.1.1　未来值
#### 1. 现在值与将来值
为了统一处理现在和将来，我们把它们都变成了将来，即所有的操作都成了异步的

Promise 决议后就是外部不可变的值,多次执行then拿到的都是不可变的值

>new Promise( function(..){ .. } ) 模式通常称为 revealing constructor（http://domenic.me/2014/02/13/the-revealing-constructor-pattern/
>）。传入的函数会立即执行（不会像 then(..) 中的回调一样异步延迟），它有两个参数，在本例中我们将其分别称为 resolve 和 reject。这些是 promise 的决议函数。resolve(..) 通常标识完成，而 reject(..) 则标识拒绝。

## 3.2　具有 then 方法的鸭子类型
```
if (
    p !== null &&
    (
        typeof p === "object" ||
        typeof p === "function"
    ) &&
    typeof p.then === "function"
) {
// 假定这是一个thenable!
}
else {
// 不是thenable
}
```

## 3.3　Promise 信任问题
### 3.3.1　调用过早
一个任务有时同步完成，有时异步完成，这可能会导致竞态条件,Promise 就不必担心这种问题
不再需要插入你自己的 setTimeout(..,0) hack

### 3.3.2　调用过晚
Promise 创建对象调用 resolve(..) 或 reject(..) 时，这个 Promise 的then(..) 注册的观察回调就会被自动调度。可以确信，这些被调度的回调在下一个异步事件点上一定会被触发
> add to micro task queue

同步查看是不可能的，所以一个同步任务链无法以这种方式运行来实现按照预期有效延迟另一个回调的发生。也就是说，一个 Promise 决议后，这个 Promise 上所有的通过then(..) 注册的回调都会在下一个异步时机点上依次被立即调用。这些回调中的任意一个都无法影响或延误对其他回调的调用

### 3.3.3　回调未调用
首先，没有任何东西（甚至 JavaScript 错误）能阻止 Promise 向你通知它的决议（如果它决议了的话）。如果你对一个 Promise 注册了一个完成回调和一个拒绝回调，那么 Promise
在决议时总是会调用其中的一个。
当然，如果你的回调函数本身包含 JavaScript 错误，那可能就会看不到你期望的结果，但实际上回调还是被调用了。
Promise 也提供了解决方案，其使用了一种称为竞态的高级抽象机制Promise.race

### 3.3.4　调用次数过少或过多
Promise 只能被决议一次

### 3.3.5　未能传递参数 / 环境值
Promise 至多只能有一个决议值（完成或拒绝）。

### 3.3.6　吞掉错误或异常
Promise 把 JavaScript 异常也变成了异步行为

### 3.3.7　是可信任的 Promise 吗
如果向 Promise.resolve(..) 传递一个非 Promise、非 thenable 的立即值，就会得到一个用这个值填充的 promise。
而如果向 Promise.resolve(..) 传递一个真正的 Promise，就只会返回同一个 promise。
更重要的是，如果向 Promise.resolve(..) 传递了一个非 Promise 的 thenable 值，前者就会试图展开这个值，而且展开过程会持续到提取出一个具体的非类 Promise 的最终值
```
var p = {
    then: function (cb) {
        cb(42);
    }
};
// 这可以工作，但只是因为幸运而已
p.then(
    function fulfilled(val) {
        console.log(val); // 42
    },
    function rejected(err) {
// 永远不会到达这里
    }
);
var p = {
    then: function (cb, errcb) {
        cb(42);
        errcb("evil laugh");
    }
};
p.then(
    function fulfilled(val) {
        console.log(val); // 42
    },
    function rejected(err) {
// 啊，不应该运行！
        console.log(err); // 邪恶的笑
    }
);
Promise.resolve(p).then(
    function fulfilled(val) {
        console.log(val); // 42
    },
    function rejected(err) {
// 永远不会到达这里
    }
);

```

## 3.4　链式流
 - 每次你对 Promise 调用 then(..) ，它都会创建并返回一个新的 Promise，我们可以将其链接起来；
 - 不管从 then(..) 调用的完成回调（第一个参数）返回的值是什么，它都会被自动设置为被链接 Promise（第一点中的）的完成。

## 3.5　错误处理

### 3.5.2　处理未捕获的情况
```
if (typeof Promise.prototype.done === 'undefined') {
    Promise.prototype.done = function (onFulfilled, onRejected) {
        this.then(onFulfilled, onRejected).catch(function (error) {
            setTimeout(function () {
                throw error;
            }, 0);
        });
    };
}
var promise = Promise.resolve();
promise.done(function () {
    JSON.parse('this is not json');    // => SyntaxError: JSON.parse
});
```

## 3.6　Promise 模式
### 3.6.1　 Promise.all([ .. ])
从 Promise.all([ .. ]) 返回的主 promise 在且仅在所有的成员 promise 都完成后才会完成。如果这些 promise 中有任何一个被拒绝的话，主 Promise.all([ .. ]) promise 就会立即被拒绝，并丢弃来自其他所有 promise 的全部结果。
永远要记住为每个 promise 关联一个拒绝 / 错误处理函数，特别是从 Promise.all([ .. ])返回的那一个。

### 3.6.2　 Promise.race([ .. ])
如果你传入了一个空数组，主race([..]) Promise 永远不会决议，而不是立即决议。

#### 2. finally
在各种各样的 Promise 库中，finally(..) 还是会创建并返回一个新的Promise（以支持链接继续）

```
// polyfill安全的guard检查
if (!Promise.observe) {
  Promise.observe = function (pr, cb) {
// 观察pr的决议
    pr.then(
      function fulfilled(msg) {
// 安排异步回调（作为Job）
        Promise.resolve(msg).then(cb);
      },
      function rejected(err) {
// 安排异步回调（作为Job）
        Promise.resolve(err).then(cb);
      }
    );
// 返回最初的promise
    return pr;
  };
}
```

### 3.6.3　 all([ .. ]) 和 race([ .. ]) 的变体
 - none([ .. ])
这个模式类似于 all([ .. ]) ，不过完成和拒绝的情况互换了。所有的 Promise 都要被拒绝，即拒绝转化为完成值，反之亦然。
 - any([ .. ])
这个模式与 all([ .. ]) 类似，但是会忽略拒绝，所以只需要完成一个而不是全部。
 - first([ .. ])
这个模式类似于与 any([ .. ]) 的竞争，即只要第一个 Promise 完成，它就会忽略后续的任何拒绝和完成。
 - last([ .. ])
这个模式类似于 first([ .. ]) ，但却是只有最后一个完成胜出。
```
if(!Promise.first){
    Promise.first = function (prs,errCb) {
        return new Promise(function (resolve, reject) {
            let i = 0
            prs.forEach(function (pr) {
                return Promise.resolve(pr).then(resolve).catch(function (reason) {
                    i++
                    if(i===prs.length){
                        reject(reason)
                    }
                    errCb(reason)
                })
            })
        })
    }
}
```

### 3.6.4　并发迭代
有些时候会需要在一列 Promise 中迭代，并对所有 Promise 都执行某个任务，非常类似于
对同步数组可以做的那样（比如 forEach(..) 、 map(..) 、 some(..) 和 every(..) ）。
```
if (!Promise.map) {
  Promise.map = function(vals,cb) {
// 一个等待所有map的promise的新promise
    return Promise.all(
// 注：一般数组map(..)把值数组转换为 promise数组
      vals.map( function(val){
// 用val异步map之后决议的新promise替换val
        return new Promise( function(resolve){
          cb( val, resolve );
        } );
      } )
    );
  };
}
```

## 3.7　Promise API 概述
### 3.7.3　 then(..) 和 catch(..)
每个 Promise 实例（不是 Promise API 命名空间）都有 then(..) 和 catch(..) 方法，通过
这两个方法可以为这个 Promise 注册完成和拒绝处理函数。Promise 决议之后，立即会调用
这两个处理函数之一，但不会两个都调用，而且总是异步调用

## 3.8　Promise 局限性
### 3.8.1　顺序错误处理
如果链中的任何一个步骤事实上进行了自身的错误处理（可能以隐藏或抽象的不可见的方式），那你就不会得到通知。
基本上，这等同于 try..catch 存在的局限： try..catch 可能捕获一个异常并简单地吞掉它

### 3.8.2　单一值
采用解构或者spread解决
```
function spread(fn) {
    return Function.apply.bind( fn, null );
}
```

### 3.8.3　单决议
Promise 最本质的一个特征是：Promise 只能被决议一次（完成或拒绝）。
>这个设计在某种程度上破坏了关注点与功能分离（SoC）的思想。你很可能想要把事件处理函数的定义和
对事件的响应（那个 Promise 链）的定义放在代码中的不同位置。如果没有辅助机制的话，在这种模式下很难这样实现。
另外一种清晰展示这种局限性的方法是：如果能够构建某种“可观测量（observable），可以将一个 Promise 链对应到这个“可观测量”就好了。有一些库已经创建了这样的抽象（比如 RxJS，<http://rxjs.codeplex.com>），但是这种抽象看起来非常笨重，以至于你甚至已经看不到任何 Promise 本身的特性。这样厚重的抽象带来了一些需要考虑的重要问题，比如这些机制（无Promise）是否像 Promise 本身设计的那样可以信任。

### 3.8.5　无法取消的 Promise
一旦创建了一个 Promise 并为其注册了完成和 / 或拒绝处理函数，如果出现某种情况使得这个任务悬而未决的话，你也没有办法从外部停止它的进程。
单独的 Promise 不应该可取消，但是取消一个可序列是合理的，因为你不会像对待 Promise那样把序列作为一个单独的不变值来传送

## 4.2 生成器
### 4.1　打破完整运行
生成器就是一类特殊的函数，可以一次或多次启动和停止，并不一定非得要完成

### 4.1.1　输入和输出
一般来说，需要的 next(..) 调用要比 yield 语句多一个
总有一个假定的 / 隐式的 return; （也就是 return undefined; ）

### 4.1.2　多个迭代器
每次构建一个迭代器，实际上就隐式构建了生成器的一个实例，通过这个迭代器来控制的是这个生成器实例
```
function step(gen) {
	var it = gen();
	var last;
	return function() {
		// 不管yield出来的是什么，下一次都把它原样传回去！
		last = it.next( last ).value;
	};
}
```