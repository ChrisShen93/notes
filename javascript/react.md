# React

标签（空格分隔）： JS React

---

## 学习JSX

JSX 的基本语法规则：遇到HTML标签（以<开头），就用HTML规则解析；遇到代码块（以 { 开头），就用JavaScript规则解析。JSX允许直接在模板插入JavaScript变量。如果这个变量是一个数组，则会展开这个数组的所有成员：

```javascript
var arr = [
  <h1>Hello world!</h1>,
  <h2>React is awesome</h2>,
];
React.render(
  <div>{arr}</div>,
  document.getElementById('example')
);
```

## 前期准备

必须引入的文件：
* react.js
* react-dom.js
* browser.js

> 下方的内容由React的官网教程翻译而来
全部代码在[github](https://github.com/reactjs/react-tutorial)中。

## 组件

React的核心思想是组件化。

原生的HTML元素名用小写字母开头，而自定义的React元素（类）用大写字母开头。

React的核心思想是扩展JavaScript语法使之支持HTML，因此推荐使用JSX，在JavaScript里出现XML风格的语法。在react中使用JSX，将比原生的JavaScript更具可读性。

通过JavaScript对象向<code>React.createClass();</code>中传递一些方法来创建一个新的React组件，其中最重要的方法是<code>render()</code>，它将返回当前React组件树，这棵树最终被渲染成HTML。

```javascript
var CommentBox = React.createClass({displayName: 'CommentBox',
  render: function() {
    return (
      React.createElement('div', {className: "commentBox"},
        "Hello, world! I am a CommentBox."
      )
    );
  }
});
ReactDOM.render(
  React.createElement(CommentBox, null),
  document.getElementById('content')
);
```

上述代码中的div标签并不是一个真正的DOM节点，它是React div组件的实例。
React不会生成HTML字符串，因此默认阻止了XSS攻击。

React用户不需要返回基本的HTML，返回一颗组件树即可。这一特性使得React得以组件化，这也是前端可维护的一条关键原则。

<code>ReactDOM.render()</code>实例化根组件，启动框架，将标记注入到该方法第二个参数指定的原始DOM节点中。

ReactDom模块暴露了DOM相关的方法；React则持有不同平台上的React的核心工具。

在React的官网教程中，<code>ReactDOM.render()</code>在整个脚本的最尾部，必须在定义了其他复合组件之后再调用<code>ReactDOM.render()</code>。

## 编写组件

```javascript
var CommentList = React.createClass({
  render: function() {
    return (
      <div className="commentList">
        Hello, world! I am a CommentList.
      </div>
    );
  }
});

var CommentForm = React.createClass({
  render: function() {
    return (
      <div className="commentForm">
        Hello, world! I am a CommentForm.
      </div>
    );
  }
});

// 注意这里是如何将HTML标签和所创建的组件混合的
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList />
        <CommentForm />
      </div>
    );
  }
});
```

HTML组件就是普通的React组件，唯一不同的一点是，JSX编译器将自动地重写HTML标签为<code>React.createElement(tagName)</code>表达式，其他的什么事都不做。这样也能有效地防止全局命名空间被污染。

## 组件属性
