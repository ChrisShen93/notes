# Canvas Tutorial

Canvas 是一个利用脚本来动态绘制图片及动画的 HTML 标签。

Canvas 并不被一些老旧的浏览器支持。

## 基础用法

```javascript
<canvas id="tutorial" width="150" height="150"></canvas>
```

Canvas 仅有width和height两个属性，默认值是 300px * 150px (width * height)。可以使用 CSS 来定义大小，但是在绘制时图形会伸缩以适应画布。若 CSS 定义的大小与 初始画布 大小比例不一致时，图形会呈现扭曲的状态。

> 当图形出现扭曲的状态时，可以尝试用 width 和 height 属性来明确定义 Canvas 的宽高，而不用 CSS。

id 属性是用来方便标识、获取 canvas 画布的。canvas 也和常规的HTML一样拥有margin、border等样式属性。但是这些属性并不会影响到 canvas 画布中的内容。

#### 代替内容

```javascript
<canvas id="tutorial" width="150" height="150">
  您当前的浏览器不支持 canvas
</canvas>
```

Canvas 标签中的内容将会在不支持 canvas 的浏览器中被渲染。

参考阅读： [make canvas more accessible](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Hit_regions_and_accessibility)

#### 结束标签

Canvas 的结束标签 &lt;/canvas&gt; 必须存在。

### 渲染上下文

```javascript
let canvas = document.getElementById('tutorial')
let ctx = canvas.getContext('2d')
```

首先先要得到 canvas 对象。 canvas 对象上有一个 getContext 方法，该方法用来获得 **渲染上下文** 和 绘图功能。

### 浏览器支持性检查

```javascript
let canvas = document.getElementById('tutorial')
if (canvas.getContext) {
  let ctx = canvas.getContext()
  // other code
} else {
  console.log('浏览器不支持canvas')
}
```

可以利用 getContext 方法存在与否来检查浏览器的支持性。


## 使用 Canvas 绘制图形
