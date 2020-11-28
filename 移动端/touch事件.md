> 移动端点击有300毫秒的延迟，pc端的双击事件失效，于是就新增了touch事件。

## 300毫秒延迟的原因

最初移动端的页面都是pc端直接移植过去的，导致页面画面太小，很难看清楚。苹果的Safari通过双指缩放和双击来对页面进行缩放，为此在点击时设置了300毫秒的延迟来确定是单击还是双击。



## touch事件

---

touchEvent, touchstart, touchmove, touchend, touchcancel

touchcancel是当点击事件被外部事件（比如电话来电、弹窗信息）打断时触发的。

### 事件对象

* touches --所有的触点信息
* changedTouches --当前被监听的事件的触点信息
* targetTouched --当前被监听的元素的触点信息

#### 触点信息

indentifier --触点标识（用于多点触控）

clientX, clientY --当前浏览器视口的x/y轴坐标

pageX，pageY --文档的x/y轴坐标

screenX，screenY --屏幕的x/y轴坐标