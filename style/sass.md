# SASS学习笔记

标签（空格分隔）： sass scss css stylesheets

---

[TOC]

## 使用

SASS的四种编译风格：
> nested: 嵌套缩进的CSS代码，缺省值
expanded: 没有缩进的、扩展的CSS代码
compact: 简洁格式的CSS代码
compressed: 压缩后的CSS代码

```ruby
sass --style compressed test.scss test.css

// watch a file
sass --watch input.scss:output.css

//watch a directory
sass --watch app/sass:public/stylesheets
```

---

---

## 基本用法

### 变量

变量以 **$** 开头

```sass
$blue: #1875e7;
div {
    color: $blue;
}
```

若需将变量镶嵌在字符串中，则必须写在 **#{}** 中

```sass
$side: left;
.rounded {
    border-#{$side}-radius: 5px;
}
```

---

### 计算功能

```sass
body {
    margin: (14px/2);
    top: 50px + 100px;
    right: $var * 10%;
}
```

---

### 嵌套

选择器嵌套：
```sass
div {
    h1 {
        color: red;
    }
}
```

等价于

```css
div h1 {
    color: red;
}
```

属性嵌套：

```sass
p {
    // border之后必须有冒号
    border: {
        color: red;
    }
}
```

等价于

```css
p {
    border-color: red;
}
```

在嵌套的代码块内，可以使用 **&** 引用父元素：

```sass
a {
    &:hover {
        color: #ffb3ff;
    }
}
```

等价于

```css
a:hover {
    color: #ffb3ff;
}
```

---

### 注释

标准的CSS注释  /* comment */  会保留到编译后的文件
单行注释 // comment， 只保留在SASS源文件中，编译后被省略
在 /* 后加一个感叹号，表示这是重要注释。即使是压缩模式编译，也会保留这行注释，通常可以用于生命版权信息

---

## 代码的重用

### 继承

```sass
.class1 {
    border: 1px solid #ddd;
}

.class2 {
    @extend .class1;
    font-size: 120%;
}
```

### Mixin

使用 **@mixin** 定义一个可复用的代码块
使用 **@include**  进行调用

```sass
@mixin left {
    float: left;
    margin-left: 10px;
}

div {
    @include left;
}
```

> mixin的强大之处在于其可以指定参数和缺省值

```sass
@mixin left($value: 10px) {
    float: left;
    margin-right: $value;
}

div {
    @include left(20px);
}
```

---

### 颜色函数

SASS内置的颜色函数，用于生成系列颜色

```sass
leghten(#cc3, 10%)  // #d6d65c
darken(#cc3, 10%)   // #a3a329
grayscale(#cc3)     // #808080
complement(#cc3)    // #33c
```

---

### 插入文件

```sass
@import "path/filename.scss"
@import "foo.css"
```

---

---

## 高级用法

### 条件语句

```sass
p {
    @if 1 + 1 == 2 {border: 1px solid;}
    @if 5 < 3 {border: 2px dotted;}
}

@if lightness($color) > 30% {
    background-color: #000;
} @else {
    background-color: #fff;
}
```

---

### 循环语句

```sass
@for $i from 1 to 10 {
    .border-#{$i} {
        border: #{$i}px solid blue;
    }
}

$in: 6;
@while $in > 0 {
    .item-#{$in} {
        width: 2em * $in;
    }
    $in: $in - 2;
}

@each $member in a, b, c, d {
    .#{$member} {
        background-image: url("/image/#{$member}.jpg");
    }
}
```

---

### 自定义函数

```sass
@function double($n) {
    @return $n * 2;
}
#sidebar {
    width: double(5px);
}
```

---

## 注意

若文件中有中文，则应在顶部生命 **@charset "utf-8"**
