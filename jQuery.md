## jQuery简介

jQuery 是一个 JavaScript 库。

jQuery库是一个JavaScript文件，可以在这个网站下载[jQuery下载](https://www.bootcdn.cn/jquery/)

优点：强大的选择器、DOM操作的封装、链式操作方式、隐式迭代、行为层与结构层的分离、开源。

---

## jQuery语法

### 基础语法

**$(selector).action()**

* `$(this).hide()`-隐藏当前元素
* `$("p").hide()`-隐藏所有`<p>`元素
* `$("p.test").hide()`-隐藏所有class=“test”的`<p>`元素
* `$("#test").hide()`-隐藏所有id=“test”的元素

所有jQuery都要位于

```javascript
$(document).ready(function(){
​         //开始写jQuery代码...
​	});
```

中。

### 简洁写法

```javascript
$(function(){
    //开始写jQuery代码...
});
```

## jQuery选择器

优势：写法简洁、支持CSS1~3选择器、选择不存在的元素不会报错。

选择符和css选择器类似，id前加"#"，class前加"."，实例：

* `$(this).hide()`-隐藏当前元素
* `$("p").hide()`-隐藏所有`<p>`元素
* `$("p.test").hide()`-隐藏所有class=“test”的`<p>`元素
* `$("#test").hide()`-隐藏所有id=“test”的元素

### 选择器类型

* 基础选择器

  * #id
  * .class
  * element
  * *

* 层次选择器

  * $("ancestor descendant")
  * $("parent>child")
  * $("prev+next") 紧接着prev元素后的同辈next元素
  * $("prev~sibling") prev元素之后的所有sibling元素

* 过滤选择器

  * 基本过滤选择器
    * :first
    * :last
    * not(selector) 
    * :even 索引是偶数的所有元素 *索引从0开始*
    * :odd 索引是奇数的所有元素
    * :eq(index) 索引等于index的元素
    * :gt(index) 索引大于。。。
    * :lt(index) 索引小于。。。
    * :header 所有标题元素
    * :animated 正在执行动画的所有元素
    * :focus 

  * 内容过滤选择器
    * :contains(text) 选取含有文本内容为“text”的元素
    * :empty 选取不含子元素或者文本元素的空元素
    * :has(selector)
    * :parent 选取含有子元素或者文本的元素

  * 可见性过滤选择器
    * :hidden
    * :visible
  * 属性过滤选择器
    * [attribute]
    * [attribute=value]
    * [attribute!=value]
    * [attribute^=value] 选取属性的值以value开始的元素
    * [attribute$=value] 。。。。。。以value结束的元素
    * [attribute*=value] 。。。。。。含有value的元素
    * [attribute|=value] 选取属性等于给定字符串或以该字符串为前缀的元素
    * [attribute~=value] 选取属性用空格分隔的值中包含一个给定值的元素
    * [attribute 1] [attrbute 2] [attribute N] 复合属性选择器，满足多个条件
  * 子元素过滤选择器
    * :nth-child(index/even/odd/equation) 选取每个父元素下的第indexget子元素或奇偶元素 *index从1算起*
    * :first-child
    * :last-child
    * :only-child 父元素只有这一个子元素时，该子元素被选取
  * 表单对象属性过滤选择器
    * :enable
    * :disable
    * :checked
    * :selected

* 表单选择器

  * :input
  * :text
  * :password
  * :radio
  * :checkbox
  * :submit
  * :image
  * :reset
  * :button
  * :file
  * :hidden

  注意事项：选择器中含有特殊符号时使用转义符

  `<div id="id#b">bb</div>`

  `$("#id\\#b")`

## jQuery事件

在 jQuery 中，大多数 DOM 事件都有一个等效的 jQuery 方法。

实例：`$("p").click(function(){});`

### 常用jQuery事件

`$(document).reaady()`

`click()`

`dblclick()`

`mouseenter()`-当鼠标指针穿过元素时触发事件

`mouseleave()`-当鼠标离开元素时触发事件

`mousedown()`

`mouseup()`

`hover()`-事件里要传入两个函数，当鼠标移动到元素上，会触发第一个函数；当鼠标移出这个元素，会触发第二个函数。实例：

```javascript
$("#p1").hover(
    function(){
        alert("你进入了 p1!");
    },
    function(){
        alert("拜拜! 现在你离开了 p1!");
    }
);
```

`focus()`-当元素获得焦点时触发事件

`blur()`-当元素失去焦点时触发事件

## jQuery效果

### 隐藏和显示

方法

`hide()`

`show()`

`toggle()`

语法

```javascript
$(selector).toggle(speed,callback);
//speed callback 都是可选参数
```

实例

```javascript
$(".toggleBTN").click(function() {
   $("p").toggle(1000, "linear",function(){
       //触发效果后的回调函数，如果“p”有多个，会执行多次
   });
  });
```

### 淡入淡出

方法

`fadeIn()`

`fadeOut()`

`fadeToggle()`

`fadeTo()`-允许渐变为给定的不透明度（值介于 0 与 1 之间）

语法：

```javascript
$(selector).fadeTo(speed,opacity,callback);
//speed为必选参数
```

### 滑动

方法

`slideDown()`

`slideUp()`

`slideToggle()`

效果跟隐藏显示差不多。

如果方法对象是个div，滑动的效果是上边不动，高进行改变达到滑动效果；

隐藏显示是左上的点不动，宽和高同时缩放达到隐藏显示的效果。

### 动画

语法

```javascript
$(Selector).animate({params},speed,callback);
//params参数是必须的，定义形成css属性，可操作多个属性。
//speed、callback参数都是可选的。
```

实例

```javascript
$("button").click(function(){
    $("div").animate({
        height:'200px',
        width:'100px',
        opacity:'0.5',
        left:'50px',
    },slow);
});
```

animate()方法可以操作几乎所有CSS属性，但需要生成颜色动画就要下载jQuery[颜色动画](https://plugins.jquery.com/color/)的插件。

相对值

animate设置CSS属性可以设置相对值（该值相对于元素的当前值），在值前加上‘+=’或‘-=’。

实例：

```javascript
$("div").animate({
    left:'250px',
    height:'+=150px',
    width:'+=150px'
  });
```

预定值

animate()的属性值还能设为“show”、“hide”、“toggle”这样的预定值。

队列功能

用于对同一个对象进行多个animate（）的调用。

实例

```javascript
$("button").click(function(){
  var div=$("div");
  div.animate({height:'300px',opacity:'0.4'},"slow");
  div.animate({width:'300px',opacity:'0.8'},"slow");
  div.animate({height:'100px',opacity:'0.4'},"slow");
  div.animate({width:'100px',opacity:'0.8'},"slow");
});
```

### 停止动画

方法

`stop()`

语法

```javascript
$(selector).stop(stopAll,goToEnd);
//stopAll，可选参数，默认为false，仅停止活动的动画，允许队列中后面的动画继续执行。为ture时，清除所有动画队列。
//goToEnd，可选参数，规定是否立即完成当前动画，默认是 false。
//stop()默认是清除当前正在进行的动画样式。
```

### callback方法

在方法中调用函数将会在动画完成之后才会执行。

而以下实例，alert框会在动画开始前就弹出

```javascript
$("button").click(function(){
  $("p").hide(1000);
  alert("段落现在被隐藏了");
});
```

### 链

在同一个元素上，允许在一条语句中运行多个jQuery。

实例

```javascript
$("#p1").css("color","red").slideUp(2000).slideDown(2000);
//可以换行书写
```

## jQuery HTML

### 获取内容和属性

#### jQuery DOM操作

jQuery中有DOM对应的操作，但需要明确的是，**DOM对象并不是jQuery对象**，DOM对象无法使用jQuery方法，反之亦然。

jQuery对象和DOM对象的互换

jQuery对象转成DOM对象：

```javascript
var $cr=$("#cr");           //jQuery对象，类似一个数组
var cr=$cr[0];         //DOM对象，通过jQuery的下角标找到
```

```javascript
var $cr=$("#cr");             //jQuery对象
var cr=$cr.get(0);         //DOM对象,通过jQuery方法找到
```

DOM对象转成jQuery对象：

```javascript
var cr=domcument.getElementById("cr"); //DOM对象
var $cr=$(cr);            //jQuery对象
```

创建节点

`var $li_1=$("<li title='banna'>banna</li>");`

获取内容

方法

* `text()`-设置或返回所选元素的文本内容
* `html()`-设置或返回所选元素的内容（包括 HTML 标记）
* `val()`-设置或返回表单字段的值，也就是value

当（）中没有值时是获取，有值时是设置。

获取属性

方法

`attr()`

实例

```javascript
$("A").attr("href")
//获取超链接中的地址
```

### 设置内容和属性

`text()`、`html()`、`val()`设置属性可以是字符串，也可以是回调函数。

回调函数有两个参数，被选元素列表中当前元素的下标，以及原始（旧的）值。然后以函数新值返回您希望使用的字符串。

实例

```javascript
 $("#test1").text(function(i,origText){
        return "旧文本: " + origText + " 新文本: Hello world! (index: " + i + ")"; 
    });
```



`attr()`设置值的格式实例

```javascript
$("#runoob").attr("href","http://www.runoob.com/jquery");
```



同时设置多个属性实例

```javascript
$("#runoob").attr({
        "href" : "http://www.runoob.com/jquery",
        "title" : "jQuery 教程"
    });
```

attr()方法也有回调函数，函数同样有两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值。

### 添加元素

方法

* `append()`- 在被选元素的结尾插入内容
* `appendTo()`-将被选元素添加到指定元素的末尾
* `prepend()`\- 在被选元素的开头插入内容
* `prependTo()`
* `after()` - 在被选元素之后插入内容
* `insertAfter()`-将被选元素插入到指定元素的后面
* `before()` - 在被选元素之前插入内容
* `insertBefore()`

这些方法可以同时添加若干新元素。

首先可以通过jQuery、HTML或DOM创建新元素，然后在用append或者prepend将新元素插入到文本当中。

实例

```javascript
function appendText()
{
    var txt1="<p>文本。</p>";  // 使用 HTML 标签创建文本
    var txt2=$("<p></p>").text("文本。");// 使用 jQuery 创建文本
    var txt3=document.createElement("p");
    txt3.innerHTML="文本。";// 使用 DOM 创建文本 text with DOM
    $("body").append(txt1,txt2,txt3);// 追加新元素
}
```

```javascript
function afterText()
{
    var txt1="<b>I </b>"; // 使用 HTML 创建元素
    var txt2=$("<i></i>").text("love ");// 使用 jQuery 创建元素
    var txt3=document.createElement("big");// 使用 DOM 创建元素
    txt3.innerHTML="jQuery!";
    $("img").after(txt1,txt2,txt3);// 在图片后添加文本
}
```

### 删除元素

方法

* `remove()`-删除被选元素（及其子元素） *该方法的返回值是一个指向已被删除的节点的引用*
* `empty()`-清空被选元素中所有子元素
* `detach()`-去掉所有匹配的元素，但不会把元素从jQuery对象中删除，可以再次使用匹配元素及其绑定的事件、附加的数据

`remove()`接受一个参数，该参数是jQuery的选择器

实例

```javascript
$("p").remove(".italic");//删除 class="italic" 的所有 <p> 元素
```

### 复制节点

`clone()`-复制节点，但复制出来的元素不具有任何行为；给方法传入参数true后即可复制元素中所绑定的事件。

### 替换节点

* `replaceWith()`-将选定元素替换成指定的HTML或DOM元素
* `replaceAll()`-与replaceWith()操作相反

### 包裹节点

* `wrap()`-选中某个节点被指定标记包裹起来
* `wrapAll()`-所有匹配的元素被一个元素包裹 *如果其中有其它元素，其它元素被放到包裹元素之后*
* `wrapInner()`-每一个匹配的元素的子元素用指定元素包裹起来

### 获取并设置CSS类

方法

* `addClass()` \- 向被选元素添加一个或多个类
* `removeClass()`- 从被选元素删除一个或多个类
* `toggleClass()`\- 对被选元素进行添加/删除类的切换操作
* `css()`\- 设置或返回样式属性

`addClass() `添加多个类的实例

```javascript
$("body div:first").addClass("important blue");
```

### jQuery css()方法

返回CSS属性`$("p").css("background-color");` 

设置CSS属性`$("p").css("background-color","yellow");`

设置多个CSS属性`$("p").css({"background-color":"yellow","font-size":"200%"});`

### jQuery尺寸

方法

- `width()`-content的宽度
- `height()`
- `innerWidth()`-content+padding的宽度
- `innerHeight()`
- `outerWidth()`-content+padding+border的宽度
- `outerHeight()`

## jQuery遍历

### 祖先

向上遍历DOM树

* `parent()`-返回被选元素的直接父元素。

* `parents()`-返回被选元素的所有祖先元素，它一路向上直到文档的根元素 (<html>)。

* `parentsUnite()`-返回介于两个给定元素之间的所有祖先元素。

  实例：

  ```javascript
  $(document).ready(function(){
    $("span").parentsUntil("div");
  });
  ```

### 后代

向下遍历DOM树。

* `children()`-返回被选元素的所有直接子元素。

  也可以使用可选参数来过滤对子元素的搜索。

  实例

  ```javascript
  $(document).ready(function(){
    $("div").children("p.1");
  });
  //返回类名为 "1" 的所有 <p> 元素，并且它们是 <div> 的直接子元素
  ```

  

* `find()`-返回被选元素的后代元素，一路向下直到最后一个后代。

  实例

  ```javascript
  $(document).ready(function(){
    $("div").find("span");
  });
  //返回属于 <div> 后代的所有 <span> 元素
  ```

  ```javascript
  $(document).ready(function(){
    $("div").find("*");
  });
  //返回 <div> 的所有后代
  ```

### 同胞

在DOM树中水平遍历

- `siblings()`-返回被选元素的所有同胞元素。
- `next()`-返回被选元素的下一个同胞元素。
- `nextAll()`-返回被选元素的所有跟随的同胞元素。
- `nextUntil()`-返回介于两个给定参数之间的所有跟随的同胞元素。
- `prev()`-返回的是前面的同胞元素
- `prevAll()`
- `prevUntil()`

### 过滤

first(), last() 和 eq()，允许基于其在一组元素中的位置来选择一个特定的元素。

实例

```javascript
$(document).ready(function(){
  $("div p").first();
});
//选取首个 <div> 元素内部的第一个 <p> 元素
```

```javasc
$(document).ready(function(){
  $("p").eq(1);
});
选取第二个 <p> 元素（索引号 1）
```

filter() 和 not() 允许选取匹配或不匹配某项指定标准的元素。

实例

```javascript
$(document).ready(function(){
  $("p").filter(".url");
});
//返回带有类名 "url" 的所有 <p> 元素
```

## jQuery AJAX

### jQuery load()

该方法就是AJAX方法。

语法：`$(selector).load(URL,data,callback);`

URL为必选参数，是希望加载的URL；

date为可选参数，是与请求一同发送的查询字符串键/值对集合；

callback是load（）方法完成后执行的函数名称，

callback回调函数有三个参数：responseTxt、StatusTXT、xhr。

实例：

```javascript
$("button").click(function(){
  $("#div1").load("demo_test.txt",function(responseTxt,statusTxt,xhr){
    if(statusTxt=="success")
      alert("外部内容加载成功!");
    if(statusTxt=="error")
      alert("Error: "+xhr.status+": "+xhr.statusText);
  });
});
```

### $.get()、$.post()方法

get（）方法语法：`$.get(URL,date,callback,type);`

callback的第一个参数存有被请求页面的内容，第二个参数存有请求的状态。

type规定服务器端返回内容的格式。

post（）方法语法：`$.post(URL,data,callback);`

data参数规定连同请求发送的数据。

callback同上。

### $.getScript()方法和$.getJSON()方法

`$.getScript(url,function(){})`可以在需要的时候动态创建`<script>`标签，从而加载javaScript文件。

$.getJSON()使用方法同上，回调函数有个参数返回json文件的数据。

可以使用`$.each()`方法来迭代该数据，*与jQuery的each()方法不同*，该方法以一个数组或者对象作为第一个参数，以一个回调函数作为第二个参数；回调函数又拥有两个参数，第一个为对象的成员或数组的索引，第二个为对应变量或内容。

**$.getJSON()方法还能通过JSONP形式的回调函数来实现跨域访问api。**

jQuery会自动将URL里的回调函数，如`URL？callback=？`中的“？”替换成正确的函数名来执行这个回调函数。

### $.ajax()方法

$.ajax()方法可以替代前面的所有方法。该方法只有一个参数。

### 序列化元素

`serialize()方法`、`serializeArray()方法`、`$.param()方法`。

在使用ajax给服务器传输data的时候，可以直接使用`serialize()方法`将jQuery对象（表单或其它选择器选取的元素）中的内容转化成字符串。

`serializeArry()方法`直接返回JSON格式的数据，可以使用`$.each()方法`对数据进行迭代。

`$.param()方法`是`serialize()方法`的核心，用来对一个数组或对象按照key/value进行序列化。

### 全局事件

通过jQuery提供的一些自定义全局函数，能够为各种与Ajax相关的事件注册回调函数。

`ajaxStart()`-Ajax请求开始时触发该方法的回调函数

`ajaxStop()`-Ajax请求结束时触发该方法的回调函数

这些方法是全局，当有多个Ajax请求时，都会被调用，如果想要让Ajax请求不受全局方法的影响，可在使用$.ajax()方法时，将参数global设置为false。



