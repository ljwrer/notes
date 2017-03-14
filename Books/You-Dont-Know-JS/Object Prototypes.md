# 第三章 对象
## 3.2　类型
不同的对象在底层都表示为二进制，在JavaScript中二进制前三位都为0的话会被判断为object类型，null的二进制表示是全0，自然前三位也是0，所以执行typeof时会返回“object”。

## 3.3　内容
在对象中，属性名永远都是字符串。如果你使用string（字面量）以外的其他值作为属性名，那它首先会被转换为一个字符串。

```js
var myObject = { };

myObject[true] = "foo"; 
myObject[3] = "bar"; 
myObject[myObject] = "baz";

myObject["true"]; // "foo" 
myObject["3"]; // "bar" 
myObject["[object Object]"]; // "baz"
```

### 3.3.1　可计算属性名
```
var myObject = {
    [Symbol.Something]: "hello world"
}
```

### 3.3.3　数组
如果你试图向数组添加一个属性，但是属性名“看起来”像一个数字，那它会变成一个数值下标（因此会修改数组的内容而不是添加一个属性）

### 3.3.4　复制对象
#### json safe 
```
var newObj = JSON.parse( JSON.stringify( someObj ) );
```
#### 浅拷贝
```
Object.assign( {}, myObject );
```

### 3.3.5　属性描述符
2. Configurable
configurable:false还会禁止删除这个属性

### 3.3.6　不变性

<div class="contentTableWrapper"><table responsive="true" summary="table"><tbody><tr responsive="true"><th scope="col"><p><span id="mt17" class="sentence" data-guid="c1c425268e68385d1ab5074c17a94f14" data-source="Function" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">函数</sentencetext></span></p></th><th scope="col"><p><span id="mt18" class="sentence" data-guid="969e3db3597a925112adbefd4d2d3137" data-source="Object is made non-extensible" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">对象已设置为不可扩展的</sentencetext></span></p></th><th scope="col"><p><span id="mt19" class="sentence" data-guid="7dfdd1a1a538b21d7850d9e45a673c5d" data-source="<strong>configurable</strong> is set to <strong>false</strong> for each property" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">为每个属性将 <strong xmlns="http://www.w3.org/1999/xhtml">configurable</strong> 设置为 <strong xmlns="http://www.w3.org/1999/xhtml">false</strong></sentencetext></span></p></th><th scope="col"><p><span id="mt20" class="sentence" data-guid="d0905cccfb4159270410f69f5b273c0d" data-source="<strong>writable</strong> is set to <strong>false</strong> for each property" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">为每个属性将 <strong xmlns="http://www.w3.org/1999/xhtml">writable</strong> 设置为 <strong xmlns="http://www.w3.org/1999/xhtml">false</strong></sentencetext></span></p></th></tr><tr><td data-th="函数"><p><a href="https://msdn.microsoft.com/zh-cn/library/ff806191(v=vs.94).aspx">Object.preventExtensions</a></p></td><td data-th="对象已设置为不可扩展的"><p><span id="mt22" class="sentence" data-guid="a6105c0a611b41b08f1209506350279e" data-source="Yes" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">是</sentencetext></span></p></td><td data-th="为每个属性将 configurable 设置为 false"><p><span id="mt23" class="sentence" data-guid="7fa3b767c460b54a2be4d49030b349c7" data-source="No" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">否</sentencetext></span></p></td><td data-th="为每个属性将 writable 设置为 false"><p><span id="mt24" class="sentence" data-guid="7fa3b767c460b54a2be4d49030b349c7" data-source="No" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">否</sentencetext></span></p></td></tr><tr><td data-th="函数"><p><a href="https://msdn.microsoft.com/zh-cn/library/ff806192(v=vs.94).aspx">Object.seal</a></p></td><td data-th="对象已设置为不可扩展的"><p><span id="mt26" class="sentence" data-guid="a6105c0a611b41b08f1209506350279e" data-source="Yes" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">是</sentencetext></span></p></td><td data-th="为每个属性将 configurable 设置为 false"><p><span id="mt27" class="sentence" data-guid="a6105c0a611b41b08f1209506350279e" data-source="Yes" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">是</sentencetext></span></p></td><td data-th="为每个属性将 writable 设置为 false"><p><span id="mt28" class="sentence" data-guid="7fa3b767c460b54a2be4d49030b349c7" data-source="No" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">否</sentencetext></span></p></td></tr><tr><td data-th="函数"><p><strong><span id="mt29" class="sentence" data-guid="8557a70f87c0add7b3334e158436fe7e" data-source="Object.freeze" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">Object.freeze</sentencetext></span></strong></p></td><td data-th="对象已设置为不可扩展的"><p><span id="mt30" class="sentence" data-guid="a6105c0a611b41b08f1209506350279e" data-source="Yes" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">是</sentencetext></span></p></td><td data-th="为每个属性将 configurable 设置为 false"><p><span id="mt31" class="sentence" data-guid="a6105c0a611b41b08f1209506350279e" data-source="Yes" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">是</sentencetext></span></p></td><td data-th="为每个属性将 writable 设置为 false"><p><span id="mt32" class="sentence" data-guid="a6105c0a611b41b08f1209506350279e" data-source="Yes" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">是</sentencetext></span></p></td></tr></tbody></table></div>

<div class="contentTableWrapper"><table responsive="true" summary="table"><tbody><tr responsive="true"><th scope="col"><p><span id="mt34" class="sentence" data-guid="c1c425268e68385d1ab5074c17a94f14" data-source="Function" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">函数</sentencetext></span></p></th><th scope="col"><p><span id="mt35" class="sentence" data-guid="e10eff337d0306fff3f89b2109addc5f" data-source="Object is extensible?" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">对象是否可扩展</sentencetext></span></p></th><th scope="col"><p><span id="mt36" class="sentence" data-guid="dd6fa77a61638021818b20c3c61728b5" data-source="<strong>configurable</strong> is <strong>false</strong> for all properties?" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">为所有属性将 <strong xmlns="http://www.w3.org/1999/xhtml">configurable</strong> 设置为 <strong xmlns="http://www.w3.org/1999/xhtml">false</strong></sentencetext></span></p></th><th scope="col"><p><span id="mt37" class="sentence" data-guid="ebd3ae09204e047c23b689680bf0c4af" data-source="<strong>writable</strong> is <strong>false</strong> for all data properties?" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">为所有数据属性将 <strong xmlns="http://www.w3.org/1999/xhtml">writable</strong> 设置为 <strong xmlns="http://www.w3.org/1999/xhtml">false</strong></sentencetext></span></p></th></tr><tr><td data-th="函数"><p><a href="https://msdn.microsoft.com/zh-cn/library/ff806188(v=vs.94).aspx">Object.isExtensible</a></p></td><td data-th="对象是否可扩展"><p><span id="mt39" class="sentence" data-guid="a6105c0a611b41b08f1209506350279e" data-source="Yes" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">是</sentencetext></span></p></td><td data-th="为所有属性将 configurable 设置为 false"><p><span id="mt40" class="sentence" data-guid="7fa3b767c460b54a2be4d49030b349c7" data-source="No" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">否</sentencetext></span></p></td><td data-th="为所有数据属性将 writable 设置为 false"><p><span id="mt41" class="sentence" data-guid="7fa3b767c460b54a2be4d49030b349c7" data-source="No" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">否</sentencetext></span></p></td></tr><tr><td data-th="函数"><p><a href="https://msdn.microsoft.com/zh-cn/library/ff806189(v=vs.94).aspx">Object.isSealed</a></p></td><td data-th="对象是否可扩展"><p><span id="mt43" class="sentence" data-guid="7fa3b767c460b54a2be4d49030b349c7" data-source="No" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">否</sentencetext></span></p></td><td data-th="为所有属性将 configurable 设置为 false"><p><span id="mt44" class="sentence" data-guid="a6105c0a611b41b08f1209506350279e" data-source="Yes" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">是</sentencetext></span></p></td><td data-th="为所有数据属性将 writable 设置为 false"><p><span id="mt45" class="sentence" data-guid="a6105c0a611b41b08f1209506350279e" data-source="Yes" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">是</sentencetext></span></p></td></tr><tr><td data-th="函数"><p><a href="https://msdn.microsoft.com/zh-cn/library/ff806185(v=vs.94).aspx">Object.isFrozen</a></p></td><td data-th="对象是否可扩展"><p><span id="mt47" class="sentence" data-guid="7fa3b767c460b54a2be4d49030b349c7" data-source="No" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">否</sentencetext></span></p></td><td data-th="为所有属性将 configurable 设置为 false"><p><span id="mt48" class="sentence" data-guid="a6105c0a611b41b08f1209506350279e" data-source="Yes" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">是</sentencetext></span></p></td><td data-th="为所有数据属性将 writable 设置为 false"><p><span id="mt49" class="sentence" data-guid="a6105c0a611b41b08f1209506350279e" data-source="Yes" xml:space="preserve"><sentencetext xmlns="http://www.w3.org/1999/xhtml">是</sentencetext></span></p></td></tr></tbody></table></div>

### 3.3.8　[[Put]]
1. 属性是否是访问描述符（参见3.3.9节）？如果是并且存在setter就调用setter。

2. 属性的数据描述符中writable是否是false？如果是，在非严格模式下静默失败，在严格模式下抛出TypeError异常。

3. 如果都不是，将该值设置为属性的值。

### 3.3.9　Getter和Setter
当你给一个属性定义getter、setter或者两者都有时，这个属性会被定义为“访问描述符”（和“数据描述符”相对）。对于访问描述符来说，JavaScript会忽略它们的value和writable特性，取而代之的是关心set和get（还有configurable和enumerable）特性。
```
var myObject = {
    // 给 a 定义一个getter
    get a() {
        return 2; 
    }
};

myObject.a = 3;

myObject.a; // 2
```

### 3.3.10 存在性
#### 1. 枚举
 - propertyIsEnumerable
	 - 给定的属性名是否直接存在于对象中（而不是在原型链上）并且满足enumerable:true
 - Object.keys
	 - 返回一个数组，包含所有可枚举属性
 - Object.getOwnPropertyNames
	 - 返回一个数组，包含所有属性，无论它们是否可枚举

## 3.4　遍历
for..of循环首先会向被访问对象请求一个迭代器对象，然后通过调用迭代器对象的next()方法来遍历所有返回值
数组有内置的@@iterator
```
var myArray = [ 1, 2, 3 ];
var it = myArray[Symbol.iterator]();

it.next(); // { value:1, done:false }
it.next(); // { value:2, done:false }
it.next(); // { value:3, done:false }
it.next(); // { done:true }
```
>我们使用ES6中的符号Symbol.iterator来获取对象的@@iterator内部属性。引用类似iterator的特殊属性时要使用符号名，而不是符号包含的值。此外，虽然看起来很像一个对象，但是@@iterator本身并不是一个迭代器对象，而是一个返回迭代器对象的·函数·——这点非常精妙并且非常重要。

普通的对象没有内置的@@iterator，所以无法自动完成for..of遍历。为了避免影响未来的对象类型。
```js
var myObject = { 
    a: 2,
    b: 3 
};

Object.defineProperty( myObject, Symbol.iterator, { 
    enumerable: false,
    writable: false,
    configurable: true,
    value: function() { 
        var o = this;
        var idx = 0;
        var ks = Object.keys( o ); 
        return {
            next: function() {
                return {
                    value: o[ks[idx++]], 
					 done: (idx > ks.length)
                }; 
            }
        }; 
    }
} );
					
// 手动遍历myObject
var it = myObject[Symbol.iterator](); 
it.next(); // { value:2, done:false }
it.next(); // { value:3, done:false }
it.next(); // { value:undefined, done:true }

// 用for..of遍历myObject
for (var v of myObject) { 
    console.log( v );
}
// 2
// 3

```