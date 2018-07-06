# Canvas Tutorial

Canvas 是一个利用脚本来动态绘制图片及动画的 HTML 标签。

Canvas 并不被一些老旧的浏览器支持。

## 相关术语

Canvas interface element: 实现了规范定义的绘图方法和属性的元素，即 *canvas* 元素。

Drawing context: 绘图上下文，一个左上角为 (0, 0) 的笛卡尔坐标平面，往右往下x、y分别增加。

Immediate-mode: 立即模式，一种绘图模式。当绘图完成后，所有绘图结构将从内存中立即丢弃。canvas即为此种。

Retained-mode: 残留模式，一种绘图模式。当绘图完成后，所有的绘图结构仍存在于内存中。DOM、SVG即为此种。

Raster: 光栅风格，一种图形风格。由多行断开的图片（行）组成，每行都包含确定的像素个数。

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

首先先要得到 canvas 对象。 canvas 对象上有一个 getContext 方法，该方法用来获得 **渲染上下文** 和 绘图功能。

### 浏览器支持性检查

```javascript
let canvas = document.getElementById('tutorial')
if (canvas.getContext) {
  let ctx = canvas.getContext('2d')
  // other code
} else {
  console.log('浏览器不支持canvas')
}
```

可以利用 getContext 方法存在与否来检查浏览器的支持性。


## 使用 Canvas 绘制图形

### 栅格和坐标空间

canvas 元素默认被网格锁覆盖。通常来说网格中的一个单元相当于canvas元素中的一像素。

栅格的起点为左上角（坐标为(0, 0)），所有元素的位置都相对于原点定位。

### 绘制矩形

矩形是canvas中唯一支持的原生图形。所有其他的图形都需要通过生成路径的方式绘制。

canvas中绘制矩形有三种方法：

```javascript
let canvas = document.getElementById('tutorial')
let ctx = canvas.getContext('2d')
// 绘制一个填充的矩形
ctx.fillRect(x, y, width, height)
// 绘制一个矩形的边框
ctx.strokeRect(x, y, width, height)
// 清除指定矩形区域，清除部分完全透明
ctx.clearRect(x, y, width, height)
```

这三个方法执行后会即时生效，立刻显示在canvas画布中。

### 绘制路径

图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。

一个路径，甚至一个子路径，必须是闭合的。

使用路径绘制图形一般有以下几个步骤：

1. 创建路径起始点
2. 使用 **画图命令** 画出路径
3. 闭合路径
4. 路径生成后，可通过描边或填充路径区域来渲染图形

一些需要用到的API

```javascript
// 新建一条路径，生成后，图形绘制命令被指向到路径上生成路径
// 路径由子路径构成，子路径存在一个列表中，所有的子路径（线、弧形、etc）构成图形
// 调用该方法后，列表会被清空重置。之后可以绘制新的图形
// beginPath() 之后，应总使用 moveTo() 设定起始位置。
beginPath()

// 闭合路径之后图形绘制命令又重新指向到上下文中
// 该命令会绘制一条从当前点到起始点的直线来闭合图形
// 调用 fill() 函数，图形将自动闭合。stroke() 不会。
closePath()

// 通过线条来绘制图形轮廓
stroke()

// 通过填充路径的内容区域生成实心的图形
fill()
```

#### 一些常用的API

```javascript
// 将笔触移动到指定的坐标(x, y)上
ctx.moveTo(x, y)

// 绘制一条从当前位置到指定坐标(x, y)的直线
ctx.lineTo(x, y)

// 绘制一个以(x, y)为圆心，以radius为半径的圆弧或者圆
// 从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）
// anticlockwise 为false时为顺时针
// startAngle endAngle 的单位为弧度
// 弧度和角度的转换： radians = (Math.PI / 180) * degrees
ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise)

// 二次贝塞尔曲线
// (cp1x, cp1y)为控制点，(x, y)为结束点
ctx.quadraticCurveTo(cp1x, cp1y, x, y)

// 三次贝塞尔曲线
// (cp1x, cp1y)为控制点一
// (cp2x, cp2y)为控制点二
// (x, y)为结束点
ctx.bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)
```

### Path2D 对象

利用 Path2D 对象，可以缓存或记录绘制命令，使得快速回顾路径成为可能。

使用方法：

```javascript
// 空的Path对象
new Path2D()
// 克隆Path对象
new Path2D(path)
// 从SVG建立Path对象
new Path2D(d)

// 添加一条路径到当前路径，可以使一个变换矩阵
Path2D.addPath(path [, transform])
```

一个使用案例，画了一个空心的矩形和一个黑色的实心圆：

```javascript
function draw () {
  let canvas = document.getElementById('tutorial')
  if (canvas.getContext) {
    let ctx = canvas.getContext('2d')

    let rectangle = new Path2D()
    rectangle.rect(10, 10, 50, 50)

    let circle = new Path2D()
    circle.moveTo(125, 35)
    circle.arc(100, 35, 25, 0, 2 * Math.PI)

    ctx.stroke(rectangle)
    ctx.fill(circle)
  }
}
```

## Canvas 的状态

每个上下文都包含一个绘图状态的堆，绘图状态包括：

* 当前的 transformation matrix
* 当前的 clipping region
* 当前的属性值：fillStyle, font, globalAlpha, globalCompositeOperation, lineCap, lineJoin, lineWidth, miterLimit, shadowBlur, shadowColor, shadowOffsetX, shadowOffsetY, strokeStyle, textAlign, textBaseline

> 当前 path 和当前 bitmap 不是绘图状态的一部分。
> 当前 path 是持久存在的，仅能被 beginPath() 复位
> 当前 bitmap 是 canvas 的属性，而非绘图上下文

绘图状态是当前画面应用的所有样式和变形的一个快照。状态的应用可以避免绘图代码的过度膨胀。

## APIs

DOM:

```
interface CanvasElement: Element {
  attribute unsigned long width;
  attribute unsigned long height;

  Object getContext(in DOMString contextId);
  DOMString toDataURL(optional in DOMString type, in any... args);
};
```

```
CanvasRenderingContext2D {
  // back-reference to the canvas
  readonly attribute HTMLCanvasElement canvas;

  // state
  void restore();   // pop state stack and restore state
  void save();      // push state on state stack

  // transformations
  // default transform is the identity matrix
  void rotate(in float angle);
  void scale(in float x, in float y);
  void setTransform(in float m11, in float m12, in float m21, in float m22, in float dx, in float dy);
  void transform(in float m11, in float m12, in float m21, in float m22, in float dx, in float dy);
  void translate(in float x, in float y);

  // composition
  attribute float globalAlpha;    // default to 1.0
  attribute DOMString globalCompositeOperation;   // 设定重叠图像的覆盖方式，source-over(default), destination-over, source-atop, destination-atop, destination-atop, source-in, destination-in, source-out, destination-out, lighter, darker, copy, xor, vendorName-operationName

  // colors and styles
  attribute any fillStyle;      // default black
  attribute any strokeStyle;    // default black
  CanvasGradient createLinearGradient(in float x0, in float y0, in float x1, in float y1);
  CanvasGradient createRadialGradient(in float x0, in float y0, in float r0, in float x1, in float y1, in float r1);
  CanvasPattern createPattern(in HTMLCanvasElement image, in DOMString repetition);

  // line styles
  attribute DOMString lineCap;      // 'butt'(default), 'round', 'square'
  attribute DOMString lineJoin;     // 'miter'(default), 'round', 'bevel'
  attribute float lineWidth;        // default 1
  attribute float miterLimit;       // default 10

  // shadows
  attribute float shadowBlur;       // default 0
  attribute DOMString shadowColor;  // default transparent black
  attribute float shadowOffsetX;    // default 0
  attribute float shadowOffsetY;    // default 0

  // rects
  void clearRect(in float x, in float y, in float width, in float height);
  void fillRect(in float x x, in float y, in float width, in float height);
  void strokeRect(in float x, in float y, in float width, in float height);

  // Complex shapes (paths) API
  void arc(in float x, in float y, in float radius, in float startAngle, in float endAngle, in boolean anticlockwise);
  void arcTo(in float x1, in float y1, in float x2, in float y2, in float radius);
  void beginPath();
  void bezierCurveTo(in float cp1x, in float cp1y, in float cp2x, in float cp2y, in float x, in float y);
  void clip();
  void closePath();
  void fill();
  void lineTo(in float x, in float y);
  void MoveTo(in float x, in float y);
  void quadraticCurveTo(in float cpx, in float cpy, in float x, in float y);
  void rect(in float x, in float y, in float width, in float height);
  void stroke();
  boolean isPointInPath(in float x, in float y);

  // text
  attribute DOMString font;     // default 10px sans-serif
  attribute DOMString textAlign;    // 'start'(default), 'end', 'left', 'right', 'center'
  attribute DOMString textBaseline; // 'top'(default), 'handing', 'middle', 'alphabetic', 'ideographic', 'bottom'
  void fillText(in DOMString text, in float x, in float y, optional in float maxWidth);
  TextMetrics measureText(in DOMString text);
  void strokeText(in DOMString text, in float x, in float y, optional in float maxWidth);

  // drawing images
  void drawImage(in HTMLImageElement image, in float dx, in float dy, optional in float dw, in float dh);
  viod drawImage(in HTMLImageElement image, in float sx, in float sy, in float sw, in float sh, in float dx, in float dy, in float dw, in float dh);

  // pixel manipulation
  ImageData createImageData(in float sw, in float sh);
  ImageData createImageData(in ImageData imagedata);
  ImageData getImageData(in float sx, in float sy, in float sw, in float sh);
  void putImageData(in ImageData imagedata, in float dx, in float dy, optional in float dirtyX, in float dirtyY, in float dirtyWidth, in float dirtyHeight);
};
```

```
interface CanvasGradient {
  // opaque object
  void addColorStop(in float offset, in DOMString color);
};

interface CanvasPattern {
  // opaque object
};

interface TextMetrics {
  readonly attribute float width;
};

interface ImageData {
  readonly attribute CanvasPixelArray data;
  readonly attribute unsigned long height;
  readonly attribute unsigned long width;
};

interface CanvasPixelArray {
  readonly attribute unsigned long length;
  getter octet(in unsigend long index);
  setter vold(in unsigned long index, in octet value);
}
```
