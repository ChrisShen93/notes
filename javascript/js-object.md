# 对象



## 综述



对象的“字典”功能，是最常见的对象的应用场景之一。然而对象不仅仅是字符串到值的映射，除了保持其自由属性外，JavaScript对象还可以从原型对象继承属性。



对象最常见的用法有创建(create)、设置(set)、查找(query)、删除(delete)、检测(test)、枚举(enumerate)其属性。

对象的属性包括名字和值。属性名可以是任意字符串，唯一的限制是对象中不能存在同名属性。值可以是任意JavaScript值，或可以是一个getter或setter方法。除名和值之外，每个属性还有一些与之相关的值，称为“属性特性”(property attribute)：

* 可写(writable attribute)，表明是否可以设置该属性的值
* 可枚举(enumerable attribute)，表明是否可以通过迭代器返回该属性
* 可配置(configurable attribute)，表明是否可以删除或修改该属性



## 创建对象



对象创建一般有三种方式：

```javascript
// 对象直接量
var o = {
    a = '',
    b = ''
}

// 构造器函数创建
function o(arg) {
    this.arg = arg;
    this.fun = function() {
        dosomething();
    }
}

// Object.create()
/**
 * @param {Object, Function} prop 待创建对象的原型
 * @param {Object} propertiesObject 对对象属性的进一步描述
 */
let o1

```

创建对象同时赋予了该对象一个构造器属性，该属性是一个指向用于创建该对象的构造器函数的引用。

```javascript
function Hero(name) {
    this.name = name;
}

var a = new Hero('Chris');

console.log(a.constructor);     // [Function: Hero]
```

因此也可以这样创建对象：

```javascript
var b = new a.constructor('yhshen');
console.log(b.name);            // yhshen
```



## Object.create()



```javascript
// 一个可行的polyfill
// 注：不支持传入null
if (typeof Object.create !== 'function') {
  Object.create = function (proto, propertiesObject) {
    if (typeof proto !== 'object' && typeof proto !== 'function') {
      throw new TypeError('Object prototype may only be an Object: ' + proto)
    } else if (proto === null) {
      throw new Error('This browser\'s implementation of Object.create is a shim and doesn\'t support null as the first argument.')
    }
    
    if (typeof propertiesObject != 'undefined') throw new Error("This browser's implementation of Object.create is a shim and doesn't support a second argument.")

    function F() {}
    F.prototype = proto

    return new F()
  }
}
```





## prototype 原型



每一个JavaScript对象（null除外）都和原型对象相关联，每一个对象都从原型继承属性。

* 通过对象直接量创建的对象都具有同一个原型对象，并可以通过JavaScript代码Object.prototype获得对原型对象的引用；
* 通过关键字new和构造函数创建的对象的原型就是构造函数的prototype属性的值。eg: 通过 new Array()创建的对象的原型就是Array.prototype

没有原型的对象位数不多，Object.prototype就是其中之一。它未继承任何属性。其他原型对象都是普通对象，普通对象都具有原型。所有的内置构造器及大多数的自定义构造器都有一个继承自Object.prototype的原型。eg: new Date()创建的对象同时继承自Date.prototype和Object.prototype，即所谓的原型链。



函数对象中最重要的属性是prototype

- 每个函数的prototype属性中都包含了一个对象
- 只有在该函数是构造器时prototype才发挥作用
- 该函数创建的所有对象都会持有一个该prototype属性的引用，并可以将其当做自身的属性来使用