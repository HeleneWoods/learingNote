# 工具

## 回退

`Modernizr`： 一个js库，用来检测用户浏览器的html5和css3特性。

`@supports`规则：相当于浏览器原生的Modernizr，但兼容性并不是很好。

可以直接通过js来实现以上工具的功能：

检测某个样式属性是否被支持，**核心思路是在任一元素的*element.style*对象上检查该属性是否存在。**

```javascript
function testProperty(property) {
    var root = document.documentElement //<html>
    if (property in root.style) {
        root.classList.add(property.toLowerCase())
        return true
    }
    root.classList.add('no-' + property.toLowerCase())
    return false
}
```

如果要检测具体属性值是否支持，就需要先将它赋值给对应的属性，然后检测浏览器是否还保存着这个值，前提是先要创建一个隐藏元素用来改变其样式：

```javascript
function testValue(id, value, property) {
    var dummy = document.createElement('p')
    dummy.style[property] = value
    if (dummy.style[property]) {
        root.classList.add(id)
        return true
    }
    root.classList.add('no' + id)
    return false
}
```

# 技巧

## 响应式

- 替换元素：img、object、video、iframe等，需要设置一个`max-width: 100%;`。

- 背景图需要完整铺满一个容器，不论容器尺寸如何变化时，使用一下属性：`background-size: cover;`。

- 当图片或其它元素以行列式进行布局时，让视口宽度来决定列的数量，可以使用`Flexbox`或者`display:inline-block;`加上常规的文本折行行为来实现。

  - ```css
    /* 文本折行 */
    white-space: normal;
    word-wrap: break-word;
    word-break: break-all;
    ```

- 使用css3的`多列`时，应指定`column-width`列宽，而非`column-count`列数。

## 简写

`background: rebeccapurple;`和`background-color: rebeccapurple ` 并不等价，展开式写法不会清空相关的其它属性，如`background-image`属性可能会影响最终呈现的效果。

`background`属性可以使用逗号分割，给同一个元素设置多个背景图片，但颜色只能设置一个，且只能放在最后一组：

```css
background: url("image.png") 0% 0%/60px 60px no-repeat padding-box,
            url("image.png") 40px 10px/110px 110px no-repeat content-box,
            url("image.png") 140px 40px/200px 100px no-repeat content-box #58a;
/* 图片存在重叠时，前面的背景图会覆盖后面的背景图 */
```

## 边框

### 半透明边框

> 当想要给容器加一个半透明边框透出父容器背景的颜色时。

```css
/* 容器样式 */
border: 10px solid hsla(0, 0%, 100%, .5);
background: white;
```

如果是以上的写法，你会发现边框消失了，原因是默认情况下，容器的背景会延伸到边框区域的下层，如p1所示。

![半透明边框](D:\MD_note\learingNote\img\transparent-border.png)

当添加了`background-clip: padding-box;`后，浏览器会用内边距的外沿来把背景裁切掉。

`background-clip`的默认值是`border-box`，意味着容器的背景会被边框的外沿（border-box）裁切掉。

可以结合盒模型来理解上面那段话：

![盒模型](D:\MD_note\learingNote\img\盒模型.png)

content-box在最里面，padding-box包裹着content，border-box包裹着padding-box，最外层是margin-box，整个构成了一个完整的容器。p1和p2的容器大小一样，但在**给div设定宽高时，实际上是在给content-box设定大小**，完整的容器大小需要加上外层的padding、border、margin。而设定的background属性的范围不一定就是给content-box设定的范围。我们可以假设，background-color的默认范围是整个浏览器，`background-clip`**属性的值决定了保留哪部分的背景色**，当值是`boder-box`时，背景色从边框盒的最外沿开始裁切，也就导致content的背景色蔓延到了border下，同理，`padding-box`是由内边距外沿开始裁切，也就是边框的内沿，所以背景色不会蔓延到边框下。p1和p2的content-box大小和整个容器的大小是一样的，只是背景色的范围大小不同而已。

### 多重边框

1、利用`box-shadow`伪造成边框的样式，使用逗号分隔制造多层边框的效果。

box-shadow的第四个参数`spread`扩张半径可以让投影面积加大或减小，配合两个为0的偏移量和一个为0的模糊值，看起来就像一个边框。

阴影是层层叠加的，第一层位于最顶层，所以第二层边框的扩张半径要比第一层的大。

```css
box-shadow: 0 0 0 10px #655, 
			0 0 0 15px deeppink, 
			0 2px 5px 15px rgba(0, 0, 0, .6);

```

![多重边框](D:\MD_note\learingNote\img\多重边框.png)

这个hack方法和border不一样的一点是，box-shadow不会影响布局，不受box-sizing属性的影响，但可以通过内外边距模拟边框所占的空间。它还不会响应鼠标时间，比如悬停、点击等。

![box-shadow不影响布局](D:\MD_note\learingNote\img\box-shadow和border的区别.png)

2、当只需要两层边框时，使用`outline`绘制外层边框。

outline相对box-shadow的好处是边框的样式比较灵活。

如果想要实现上面的多重边框效果，代码如下：

```css
border: 10px solid #655;
outline: 5px solid deeppink;
```

缺点是，当元素是圆角时，outline无法贴合，它的描边还是直角的，算是css里的bug。

### 灵活的背景定位 

除了使用`background-position`和`background-origin`的扩展语法方案外（[详解](#bc)），还可以利用`calc()`函数。

```css
background: url('img.svg') no-repeat;
background-position: calc(100% - 20px) calc(100% - 10px);/* 以左上角作为偏移基点，距最右20px，距最下10px */
```

### 边框内圆角

![边框内圆角](D:\MD_note\learingNote\img\边框内圆角.png)

想要做到上图效果，有两种方法：

- 创建两个`div`，内层div设圆角 ，外层假装成border。

- 使用`outline`属性进行hack。

  - ```css
    background: tan;
    border-radius: .8em;
    padding: 1em;
    box-shadow: 0 0 0 .4em #655; /* 填补容器圆角和outline之间的空隙，投影扩张值不能和描边的 宽度一样，渲染的时候会溢出，要稍微小一点 */
    outline: .6em solid #655;
    ```

# 基础

## 单位

`px`：固定单位；

`em` ：相对父级字号进行响应式；

`rem`：相对根级字号进行响应式；

`vw`：相对可视区域的宽度进行响应式，全宽为100；

`vh`：相对可视区域的高度进行响应式，全高为100；

`vmin`、`vmax`：取最小或最大长度的百分比，这里对比的是可视区宽和高的长度；

## 属性

### inherit关键字

`inherit`可以用在任何属性中：

```css
input, select, button { font: inherit } /* 将表单元素的字体设定为与页面其它部分相同 */
a { color: inherit } /* 把超链接的颜色设定为与页面其它部分相同 */
```

同样适用于`background`、`border`等属性中。

### background

简写顺序：简写的顺序如下: bg-color || bg-image || bg-position [ / bg-size]? || bg-repeat || bg-attachment || bg-origin || bg-clip

- `background-image`: 设置背景图像, 可以是真实的图片路径, 也可以是创建的渐变背景;
- `background-position`: 设置背景图像的位置;
-  `background-size`: 设置背景图像的大小;
-  `background-repeat`: 指定背景图像的铺排方式;
-  `background-attachment`: 指定背景图像是滚动还是固定;
-  `background-origin`: 设置背景图像显示的原点[background-position相对定位的原点];
-  `background-clip`: 设置背景图像向外剪裁的区域;
-  `background-color`: 指定背景颜色。

#### <span id="bc">background-position</span>

**允许指定背景图片距离任意角的偏移量**。

```css
background: url('img.svg') no-repeat #58a;
background-position: right 20px bottom 20px; /* 距离右下角20px */

/* 也可以直接简写成 */
background: url('img.svg') no-repeat right 20px bottom 20px #58a;
```

#### background-origin

在给背景图片设置偏移量的时候，**决定是基于哪个盒模型进行的偏移**。

默认值是`padding-box`，根据的是内边距的外沿框，所以上面的例子，当给容器设置了padding，背景图的位置就会发生变化，而如果值是`content-box`，偏移量就以内容区的边缘作为基准。

```css
padding: 10px;
background: url('img.svg') no-repeat bottom right #58a;
background-origin: content-box;
```

### box-shadow

和background一样，box-shadow也支持逗号分隔语法，来达到创建多个投影的效果。

### outline

描边，语法和border一样，效果也差不多，可以在border的外层再绘制出一层边框。

使用`outline-offset`属性可以控制outline和元素边缘的间距，接受正负值。可以用这一属性做出漂亮的缝边效果：

```css
outline: 1px solid #fff;
outline-offset: -20px;
```

![stitches](D:\MD_note\learingNote\img\outline-offset.png)

 ### HSLA颜色

H：色度，取值（1~360）；

S：饱和度，取值（0%~100%）；

L：亮度，取值（0%~100%），0%最暗，100%最亮；

A：不透明度，取值（0.0~1.0），0.0完全透明，1.0完全不透明。

