# JavaScript常见设计模式

标签（空格分隔）： JavaScript

---

[TOC]

## 单例模式

### 实现一：对象字面量

```javascript
var singleton = {
    attr: 1,
    method: function() {
        return this.attr;
    }
}

var t1 = singleton;
var t2 = singleton;

console.log(t1 === t2);     // true
```

点评：最简单的单例实现，但没有体现封装性，所有的方法和属性都是暴露的。

### 实现二： 构造函数内部判断

```javascript
function Construct() {
    // 确保只有单例
    if(Construct.unique !== undefined) {
        return Construct.unique;
    }
    // 其他代码
    this.name = 'yhshen';
    this.age = '23';
    Construct.unique = this;
};
var t1 = new Construct();
var t2 = new Construct();

console.log(t1 === t2);     // true
```

点评：没有安全性，体现在一旦在外部修改了Construct的unique属性，则单例模式也就被破坏了。

### 实现三：闭包方式

```javascript
var single = (function() {
    var unique;
    function Construct() {
        if(Construct.unique !== undefined) {
            return Construct.unique;
        }
    };
    unique = new Construct();

    return unique;
})();

var t1 = single;
var t2 = single;
console.log(t1 === t2);     // true
```

点评：与对象字面量方式类似，稍微安全一点。
