# JS

标签（空格分隔）： 前端 JavaScript

---

## JS的函数作用域和声明提前

在类C语言中，花括号内的每一段代码都具有各自的作用域，而且变量在声明自身的代码块之外是不可见的，这是块级作用域。
JS中没有块级作用域，其采用函数作用域：变量在声明自身的函数体以及这个函数体嵌套的任意函数体内都是有定义的。

## "==="严格运算符和"=="相等运算符

参考：[云澹@知乎](http://www.zhihu.com/question/31442029/answer/53641960)
[阮一峰--运算符](http://javascript.ruanyifeng.com/grammar/operator.html#toc6)

严格运算符的运算规则如下：

1. 不同类型值
如果两个值的类型不同，直接返回false。
2. 同一类的原始类型值
同一类型的原始类型的值（数值、字符串、布尔值）比较时，值相同就返回true，值不同就返回false。
3. 同一类的复合类型值
两个复合类型（对象、数组、函数）的数据比较时，不是比较它们的值是否相等，而是比较它们是否指向同一个对象。
4. undefined和null
undefined 和 null 与自身严格相等。

相等运算符在比较相同类型的数据时，与严格相等运算符完全一样。
在比较不同类型的数据时，相等运算符会先将数据进行类型转换，然后再用严格相等运算符比较。类型转换规则如下：

1. 原始类型的值
原始类型的数据会转换成数值类型再进行比较。字符串和布尔值都会转换成数值。
2. 对象与原始类型值比较
对象（这里指广义的对象，包括数值和函数）与原始类型的值比较时，对象转化成原始类型的值，再进行比较。
3. undefined和null
undefined和null与其他类型的值比较时，结果都为false，它们互相比较时结果为true。
4. 相等运算符的缺点
相等运算符隐藏的类型转换，会带来一些违反直觉的结果。

```javascript
'' == '0'           // false
0 == ''             // true
0 == '0'            // true

false == 'false'    // false
false == '0'        // true

false == undefined  // false
false == null       // false
null == undefined   // true

' \t\r\n ' == 0     // true
链接：http://www.zhihu.com/question/31442029/answer/53641960
```


## 正则表达式

两种等价的创建方式：

```javascript
var pattern1 = /[bc]at/i;

var pattern2 = new RegExp('[bc]at', 'i');
```

使用RegExp()构造函数的话，需要对元字符进行双重转义。

举例：

|字面量表达式|等价的字符串|
|------------------|------------------|
|    /\\[bc\ ]at/    | "\\\\[bc\\\\]at" |
|\\.at/|"\\\\.at"|
|/name\/age|"name\\\\/age"|
|\\d.\\d(1,2)/|"\\\\d.\\\\(1,2)"|

> JavaScript高级程序设计（第3版）5.4.0最后一个案例有错误。第一个循环也能输出10个true。

```javascript
var myReg = /(\w+)\s(\w+)/;
var str  = "John Smith";
var newstr = str.replace(myReg, "$2, $1");
console.log(newstr);

console.log('=============================');

var yuan = '￥6000';
var doll = '$1000';
var regMoney = /([￥$])(\d+)/;
regMoney.exec(yuan);
console.log('use $1: ===> ' + RegExp.$1);       // $1~$9是RegExp的静态属性，只能通过RegExp访问
console.log('use $2: ===> ' + RegExp.$2);
console.log('1: ===> ' + regMoney.exec(yuan)[1]);
console.log('2: ===> ' + regMoney.exec(doll)[2]);

console.log('=============================');
// 提炼出单位部分和整数部分
var yuan2 = '0508.1106$';
var regMoney2 = /(\d+)(\.?)(\d+)([$￥])/;
regMoney2.exec(yuan2);
console.log('单位部分：' + RegExp.$4);
console.log('整数部分：' + RegExp.$1);
console.log('下面引入非捕获组');
// 上面的正则或原始数据发生改变，但依然需要输出单位和整数部分的话，使用$1和$4可能会输出不同的捕获组
// 使用符号 ?: 标记的分组即为非捕获组
var regMoney3 = /(\d+)(?:\.?)(?:\d+)([$￥])/;
regMoney3.exec(yuan2);
console.log('单位部分：' + RegExp.$2);
console.log('整数部分：' + RegExp.$1);

console.log('=============================');

/*
 * 肯定顺/逆序环视（也叫肯定式向前/后查找）
 *  exp1(?=exp2) 表达式会匹配exp1表达式，但只有其后面内容是exp2的时候才会匹配
 */
var test1Str = '12332aa438aaf';
var test1Reg = /[0-9a-z]{2}(?=aa)/g;
var test1match = test1Reg.exec(test1Str);
console.log('匹配到的索引1为 ===> ' + test1match.index);
console.log('匹配到的字符串为 ===> ' + test1match[0]);
test1match = test1Reg.exec(test1Str);       // 此处必须再次匹配一次
console.log('匹配到的索引2为 ===> ' + test1match.index);
console.log('匹配到的字符串为 ===> ' + test1match[0]);
```

## 回调与作用域


JavaScript没有块级作用域
```javascript
if(true) {
    var color = 'blue';
}
console.log(color);
```

## 闭包

闭包的实现形式：

```javascript
// 第一种
function f() {
    var b = 'b';
    return function() {
        return b;
    }
}


// 第二种
var n;
function f() {
    var b = 'b';
    n = function() {
        return b;
    }
}


// 第三种
function f(arg) {
    var n = function() {
        return arg;
    };
    arg++;
    return n;
}
```


## 对象

创建对象的两种方式：

```javascript
// 文本对象标识法
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


## prototype

函数对象中最重要的属性是prototype

* 每个函数的prototype属性中都包含了一个对象
* 只有在该函数是构造器时prototype才发挥作用
* 该函数创建的所有对象都会持有一个该prototype属性的引用，并可以将其当做自身的属性来使用


## 绑定事件

主要讲述.on()


## AJAX

原生AJAX与$.ajax()，其他封装略带




## 事件冒泡与阻止冒泡




## $.extend()