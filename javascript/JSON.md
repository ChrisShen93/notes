# JSON 对象

标签（空格分隔）： JavaScript

---

## JSON 对象

|value|replacer|space|
|---|---|---|
|将要序列化成一个JSON字符串的值|如果该参数是一个函数，则在序列化过程中，被序列化的每个属性都会经过该函数的转换和处理；如果该参数时一个数组，则只有包含在这个数组中的属性名才会被序列化到最终的JSON字符串中|指定缩进用的空白字符串，用于美化输出。若该参数为数字，则表示有多少个空格，上线为10，小于1则意味着没有空格；若为字符串，则该字符串（前10个字母）将被作为空格。|

### JSON.stringify(value[, replacer [, space]])

```javascript
function replacer (key, value) {
    if (typeof value === 'string') {
        return undefined
    }
    return value
}
let foo = {foundation: "Mozilla", model: "box", week: 45, transport: "car", month: 7}
let json_replacer_1 = JSON.stringify(foo, replacer)           // {"week":45,"month":7}
let json_replacer_2 = JSON.stringify(foo, ['week', 'month'])    // {"week":45,"month":7}
```

### JSON.parse(text[, reviver])

reviver 函数用来在返回解析值前进行一次转换：

```javascript
JSON.parse('{"p": 5}', function (key, value) {
    if (k === '') return v
    return v * 2
})
// ==> {p: 10}

// reviver 在遍历时按深度优先遍历
JSON.parse('{"1": 1, "2": 2,"3": {"4": 4, "5": {"6": 6}}}', function (k, v) {
    console.log(k); // 输出当前的属性名，从而得知遍历顺序是从内向外的，
                    // 最后一个属性名会是个空字符串。
    return v;       // 返回原始属性值。
});
// 1
// 2
// 4
// 6
// 5
// 3
// ""
```
