> 学习目标：学会如何使用jQuery，掌握jQuery的常用api，能够使用jQuery实现常见的效果。

## 为什么要学习jQuery？

使用javascript开发过程中，有许多的缺点：

```markdown
1. 查找元素的方法太少，麻烦。
2. 遍历伪数组很麻烦，通常要嵌套一大堆的for循环。
3. 有兼容性问题。
4. 想要实现简单的动画效果，也很麻烦
5. 代码冗余。
```

## jQuery初体验

```javascript
$(document).ready(function () {
    $("#btn1").click(function () {
      	//隐式迭代：偷偷的遍历，在jQuery中，不需要手动写for循环了，会自动进行遍历。
        $("div").show(200);
    });

    $("#btn2").click(function () {
        $("div").text("我是内容");
    });
});
```

优点总结：

```markdown
1. 查找元素的方法多种多样，非常灵活
2. 拥有隐式迭代特性，因此不再需要手写for循环了。
3. 完全没有兼容性问题。
4. 实现动画非常简单，而且功能更加的强大。
5. 代码简单、粗暴。
```

> 没有对比，就没有伤害，有了对比，处处戳中要害。

## 什么是jQuery?

> jQuery的官网 http://jquery.com/
> jQuery就是一个js库，使用jQuery的话，会比使用JavaScript更简单。

js库：把一些常用到的方法写到一个单独的js文件，使用的时候直接去引用这js文件就可以了。（animate.js、common.js）

我们知道了，jQuery其实就是一个js文件，里面封装了一大堆的方法方便我们的开发，其实就是一个加强版的common.js，因此我们学习jQuery，其实就是学习jQuery这个js文件中封装的一大堆方法。

## jQuery的版本

> 官网下载地址：http://jquery.com/download/
> jQuery版本有很多，分为1.x 2.x 3.x

大版本分类：

```llvm
1.x版本：能够兼容IE678浏览器
2.x版本：不兼容IE678浏览器
1.x和2.x版本jquery都不再更新版本了，现在只更新3.x版本。

3.x版本：不兼容IE678，更加的精简（在国内不流行，因为国内使用jQuery的主要目的就是兼容IE678）
```

关于压缩版和未压缩版

```apache
jquery-1.12.4.min.js:压缩版本，适用于生产环境，因为文件比较小，去除了注释、换行、空格等东西，但是基本没有颗阅读性。
jquery-1.12.4.js:未压缩版本，适用于学习与开发环境，源码清晰，易阅读。
```

## jQuery的入口函数

使用jQuery的三个步骤：

```markdown
1. 引入jQuery文件
2. 入口函数
3. 功能实现
```

关于jQuery的入口函数：

```javascript
//第一种写法
$(document).ready(function() {
	
});
//第二种写法
$(function() {
	
});
```

jQuery入口函数与js入口函数的对比

```markdown
1.	JavaScript的入口函数要等到页面中所有资源（包括图片、文件）加载完成才开始执行。
2.	jQuery的入口函数只会等待文档树加载完成就开始执行，并不会等待图片、文件的加载。
```

## jQuery对象与DOM对象的区别（重点）

```markdown
1. DOM对象：使用JavaScript中的方法获取页面中的元素返回的对象就是dom对象。
2. jQuery对象：jquery对象就是使用jquery的方法获取页面中的元素返回的对象就是jQuery对象。
3. jQuery对象其实就是DOM对象的包装集（包装了DOM对象的集合（伪数组））
4. DOM对象与jQuery对象的方法不能混用。
```

DOM对象转换成jQuery对象：【联想记忆：花钱】

```nsis
var $obj = $(domObj);
// $(document).ready(function(){});就是典型的DOM对象转jQuery对象
```

jQuery对象转换成DOM对象：

```gams
var $li = $(“li”);
//第一种方法（推荐使用）
$li[0]
//第二种方法
$li.get(0)
```

# 选择器

## 什么是jQuery选择器

jQuery选择器是jQuery为我们提供的一组方法，让我们更加方便的获取到页面中的元素。注意：jQuery选择器返回的是jQuery对象。

jQuery选择器有很多，基本兼容了CSS1到CSS3所有的选择器，并且jQuery还添加了很多更加复杂的选择器。【查看jQuery文档】

jQuery选择器虽然很多，但是选择器之间可以相互替代，就是说获取一个元素，你会有很多种方法获取到。所以我们平时真正能用到的只是少数的最常用的选择器。

## 基本选择器

| 名称       | 用法               | 描述                                 |
| ---------- | ------------------ | ------------------------------------ |
| ID选择器   | $(“#id”);          | 获取指定ID的元素                     |
| 类选择器   | $(“.class”);       | 获取同一类class的元素                |
| 标签选择器 | $(“div”);          | 获取同一类标签的所有元素             |
| 并集选择器 | $(“div,p,li”);     | 使用逗号分隔，只要符合条件之一就可。 |
| 交集选择器 | $(“div.redClass”); | 获取class为redClass的div元素         |

> 总结：跟css的选择器用法一模一样。

## 层级选择器

| 名称       | 用法        | 描述                                                        |
| ---------- | ----------- | ----------------------------------------------------------- |
| 子代选择器 | $(“ul>li”); | 使用>号，获取儿子层级的元素，注意，并不会获取孙子层级的元素 |
| 后代选择器 | $(“ul li”); | 使用空格，代表后代选择器，获取ul下的所有li元素，包括孙子等  |

> 跟CSS的选择器一模一样。

## 过滤选择器

> 这类选择器都带冒号:

| 名称         | 用法                               | 描述                                                        |
| ------------ | ---------------------------------- | ----------------------------------------------------------- |
| :eq（index） | $(“li:eq(2)”).css(“color”, ”red”); | 获取到的li元素中，选择索引号为2的元素，索引号index从0开始。 |
| :odd         | $(“li:odd”).css(“color”, ”red”);   | 获取到的li元素中，选择索引号为奇数的元素                    |
| :even        | $(“li:even”).css(“color”, ”red”);  | 获取到的li元素中，选择索引号为偶数的元素                    |

## 筛选选择器(方法)

> 筛选选择器的功能与过滤选择器有点类似，但是用法不一样，筛选选择器主要是方法。

| 名称               | 用法                        | 描述                             |
| ------------------ | --------------------------- | -------------------------------- |
| children(selector) | $(“ul”).children(“li”)      | 相当于$(“ul>li”)，子类选择器     |
| find(selector)     | $(“ul”).find(“li”);         | 相当于$(“ul li”),后代选择器      |
| siblings(selector) | $(“#first”).siblings(“li”); | 查找兄弟节点，不包括自己本身。   |
| parent()           | $(“#first”).parent();       | 查找父亲                         |
| eq(index)          | $(“li”).eq(2);              | 相当于$(“li:eq(2)”),index从0开始 |
| next()             | $(“li”).next()              | 找下一个兄弟                     |
| prev()             | $(“li”).prev()              | 找上一次兄弟                     |

# 元素设置

## CSS操作

1.设置单个样式

```arcade
    //两个参数  name:样式名 value:样式值
    $('li').css('color','red');
    //传入对象
    $('li').css({'color':'red'});
```

2.设置多个样式

```arcade
    $('li').css({
        'color':'green',
        'font-size':'20px'
    });
```

3.获取样式

```scss
 //name:想要获取的样式
  	css(name);   
```

隐式迭代：

设置操作的时候：会给jq内部的所有对象都设置上相同的值。

获取样式的时候：只会返回第一个元素对应的值。

## Class操作

```reasonml
    /*1.添加一个类*/
    $('li').addClass('now');
    /*2.删除一个类*/
    $('li').removeClass('now');
    /*3.切换一个类  有就删除没有就添加*/
    $('li').toggleClass('now');
    /*4.判断是否包含某个类  如果包含返回true否则返回false*/
    $('li').hasClass('now');
```

## 属性操作

```arcade
    /*1.获取属性*/
    $('li').attr('name');
    /*2.设置属性*/
    $('li').attr('name','tom');
    /*3.设置多个属性*/
    $('li').attr({
        'name':'tom',
        'age':'18'
    });
    /*4.删除属性*/
    $('li').removeAttr('name');
```

## prop方法

```reasonml
    /*对于布尔类型的属性，不要使用attr方法，应该用prop方法  prop用法跟attr方法一样。*/
    $("#checkbox").prop("checked");
    $("#checkbox").prop("checked", true);
    $("#checkbox").prop("checked", false);
    $("#checkbox").removeProp("checked");
```

# jQuery动画

## 基本动画

```awk
    /*注意：动画的本质是改变容器的大小和透明度*/
    /*注意：如果不传参数是看不到动画*/
    /*注意：可传入特殊的字符  fast normal slow  分别代表 fast:200ms   normal:400ms  slow:600ms*/
    /*注意：可传入数字 单位毫秒*/
 
    /*1.展示动画*/
   	show();
    /*2.隐藏动画*/
   	hide();
    /*3.切换展示和隐藏*/
  	toggle();
```

## 滑入滑出动画

```reasonml
    /*注意：本质是改变容器的高度
      slideDown slideUp：如果不传参数，有一个默认值normal*/    
    /*1.滑入动画*/
    $('li').slideDown();
    /*2.滑出动画*/
    $('li').slideUp();
    /*3.切换滑入滑出*/
    $('li').slideToggle();
    
```

## 淡入淡出动画

```reasonml
    /*注意：本质是改变容器的透明度
    fadeIn() fadeOut() 不传参数默认normal*/
    
    /*1.淡入动画*/
    $('li').fadeIn();
    /*2.淡出动画*/
    $('li').fadeOut();
    /*3.切换淡入淡出*/
    $('li').fadeToggle();
    //淡入到什么状态
    $('li').fadeTo('speed','opacity');
```

## 自定义动画

```javascript
    /*
    * 自定义动画
    * 参数1：需要做动画的属性
    * 参数2：需要执行动画的总时长
    * 参数3：执行动画的时候的速度
    * 参数4：执行动画完成之后的回调函数
    * */
    $('#box1').animate({left:800},5000);
    $('#box2').animate({left:800},5000,'linear');
    $('#box3').animate({left:800},5000,'swing',function () {
        console.log('动画执行完成');
    });
```

## 动画队列

> jQuery中有个动画队列的机制。当我们对一个对象添加多次动画效果时后添加的动作就会被放入这个动画队列中， 等前面的动画完成后再开始执行。 可是用户的操作往往都比动画快，如果用户对一个对象频繁操作时不处理动画队列就会造成队列堆积，影响到效果。

## stop的使用

```arcade
    /*1.停止当前动画  如果动画队列当中还有动画立即执行*/
    $('div').stop();
    /*2.和stop()效果一致  说明这是默认设置*/
    $('div').stop(false,false);
    /*3.停止当前动画  清除动画队列*/
    $('div').stop(true,false);
    /*4.停止当前动画并且到结束位置  清除了动画队列*/
    $('div').stop(true,true);
    /*5.停止当前动画并且到结束位置  如果动画队列当中还有动画立即执行*/
    $('div').stop(false,true);
```

# jQuery节点操作

## 创建节点

### 1.html();

不传参数:获取元素的所有内容

```arcade
$("#div1").html();
```

传入参数:设置内容,并且会把原来的内容给覆盖,如果设置的内容中含有标签,会把标签给解析出来

```xml
 $("#div1").html('我是设置的内容<a href="http://www.baidu.com" target="_blank">百度1</a>');
```

### 2. $();

```reasonml
 var $a = $('<a href="http://www.baidu.com" target="_blank">百度1</a>');
```

## 克隆节点

```arcade
    /*只存在内存中,如果想要在页面显示,就应该追加到页面上 传参true 表示连带事件一起克隆 false表示不克隆事件*/
    var $cloneP = $('p').clone(true);
```

## 添加&移动节点

```reasonml
    /*追加自身的最后面  传对象和html格式代码*/
    $('#box').append('<a href="http://www.baidu.com" target="_blank"><b>百度3</b></a>');
    $('#box').append($('a'));
    /*追加到目标元素最后面  传目标元素的选择器或者对象*/
    $('<a href="http://www.baidu.com" target="_blank"><b>百度3</b></a>').appendTo($('#box'));
    $('a').appendTo('#box');
    
    prepend();
    prependTo();
    after();
    before();
```

## 删除节点&清空节点

```awk
    /*1.清空box里面的元素*/
    /* 清理门户 */
    $('#box').empty();
    /*2.删除某个元素*/
    /* 自杀 */
    $('#box').remove();
```

# jQuery特殊属性操作

## val方法

> val方法用于设置和获取表单元素的值，例如input、textarea的值

```reasonml
//设置值
$("#name").val(“张三”);
//获取值
$("#name").val();
```

## html方法与text方法

> html方法相当于innerHTML text方法相当于innerText

```gams
//设置内容
$(“div”).html(“<span>这是一段内容</span>”);
//获取内容
$(“div”).html()

//设置内容
$(“div”).text(“<span>这是一段内容</span>”);
//获取内容
$(“div”).text()
```

区别：html方法会识别html标签，text方法会那内容直接当成字符串，并不会识别html标签。

## width方法与height方法

> 设置或者获取高度

```gcode
//带参数表示设置高度
$(“img”).height(200);
//不带参数获取高度
$(“img”).height();
```

获取网页的可视区宽高

```gcode
//获取可视区宽度
$(window).width();
//获取可视区高度
$(window).height();
```

## scrollTop与scrollLeft

> 设置或者获取垂直滚动条的位置

```reasonml
//获取页面被卷曲的高度
$(window).scrollTop();
//获取页面被卷曲的宽度
$(window).scrollLeft();
```

## offset方法与position方法

> offset方法获取元素距离document的位置，position方法获取的是元素距离有定位的父元素的位置。

```reasonml
//获取元素距离document的位置,返回值为对象：{left:100, top:100}
$(selector).offset();
//获取相对于其最近的有定位的父元素的位置。
$(selector).position();
```

# jQuery事件机制

> JavaScript中已经学习过了事件，但是jQuery对JavaScript事件进行了封装，增加并扩展了事件处理机制。jQuery不仅提供了更加优雅的事件处理语法，而且极大的增强了事件的处理能力。

## jQuery事件发展历程(了解)

简单事件绑定>>bind事件绑定>>delegate事件绑定>>on事件绑定(推荐)

> 简单事件注册

```stylus
click(handler)			单击事件
mouseenter(handler)		鼠标进入事件
mouseleave(handler)		鼠标离开事件
```

缺点：不能同时注册多个事件

> bind方式注册事件

```awk
//第一个参数：事件类型
//第二个参数：事件处理程序
$("p").bind("click mouseenter", function(){
    //事件响应方法
});
```

缺点：不支持动态事件绑定

> delegate注册委托事件

```awk
// 第一个参数：selector，要绑定事件的元素
// 第二个参数：事件类型
// 第三个参数：事件处理函数
$(".parentBox").delegate("p", "click", function(){
    //为 .parentBox下面的所有的p标签绑定事件
});
```

缺点：只能注册委托事件，因此注册时间需要记得方法太多了

> on注册事件

## on注册事件(重点)

> jQuery1.7之后，jQuery用on统一了所有事件的处理方法。
>
> 最现代的方式，兼容zepto(移动端类似jQuery的一个库)，强烈建议使用。

on注册简单事件

```arcade
// 表示给$(selector)绑定事件，并且由自己触发，不支持动态绑定。
$(selector).on( "click", function() {});
```

on注册委托事件

```arcade
// 表示给$(selector)绑定代理事件，当必须是它的内部元素span才能触发这个事件，支持动态绑定
$(selector).on( "click",“span”, function() {});
```

on注册事件的语法：

```awk
// 第一个参数：events，绑定事件的名称可以是由空格分隔的多个事件（标准事件或者自定义事件）
// 第二个参数：selector, 执行事件的后代元素（可选），如果没有后代元素，那么事件将有自己执行。
// 第三个参数：data，传递给处理函数的数据，事件触发的时候通过event.data来使用（不常使用）
// 第四个参数：handler，事件处理函数
$(selector).on(events[,selector][,data],handler);
```

## 事件解绑

> unbind方式（不用）

```reasonml
$(selector).unbind(); //解绑所有的事件
$(selector).unbind("click"); //解绑指定的事件
```

> undelegate方式（不用）

```reasonml
$( selector ).undelegate(); //解绑所有的delegate事件
$( selector).undelegate( “click” ); //解绑所有的click事件
```

> off方式（推荐）

```reasonml
// 解绑匹配元素的所有事件
$(selector).off();
// 解绑匹配元素的所有click事件
$(selector).off("click");
```

## 触发事件

```reasonml
$(selector).click(); //触发 click事件
$(selector).trigger("click");
```

## jQuery事件对象

jQuery事件对象其实就是js事件对象的一个封装，处理了兼容性。

```awk
//screenX和screenY	对应屏幕最左上角的值
//clientX和clientY	距离页面左上角的位置（忽视滚动条）
//pageX和pageY	距离页面最顶部的左上角的位置（会计算滚动条的距离）

//event.keyCode	按下的键盘代码
//event.data	存储绑定事件时传递的附加数据

//event.stopPropagation()	阻止事件冒泡行为
//event.preventDefault()	阻止浏览器默认行为
//return false:既能阻止事件冒泡，又能阻止浏览器默认行为。
```

# jQuery补充知识点

## 链式编程

> 通常情况下，只有设置操作才能把链式编程延续下去。因为获取操作的时候，会返回获取到的相应的值，无法返回 jQuery对象。

```scss
end(); // 筛选选择器会改变jQuery对象的DOM对象，想要回复到上一次的状态，并且返回匹配元素之前的状态。
```

## each方法

> jQuery的隐式迭代会对所有的DOM对象设置相同的值，但是如果我们需要给每一个对象设置不同的值的时候，就需要自己进行迭代了。

作用：遍历jQuery对象集合，为每个匹配的元素执行一个函数

```oxygene
// 参数一表示当前元素在所有匹配元素中的索引号
// 参数二表示当前元素（DOM对象）
$(selector).each(function(index,element){});
```

## 多库共存

> jQuery使用作 为 标 示 符 ， 但 是 如 果 与 其 他 框 架 中 的 作为标示符，但是如果与其他框架中的作为标示符，但是如果与其他框架中的冲突时，jQuery可以释放$符的控制权.

```ruby
var c = $.noConflict();//释放$的控制权,并且把$的能力给了c
```

# 插件

## 常用插件

> 插件：jquery不可能包含所有的功能，我们可以通过插件扩展jquery的功能。
>
> jQuery有着丰富的插件，使用这些插件能给jQuery提供一些额外的功能。

### jquery.color.js

> animate不支持颜色的渐变，但是使用了jquery.color.js后，就可以支持颜色的渐变了。

使用插件的步骤

```markdown
1. 引入jQuery文件
2. 引入插件（如果有用到css的话，需要引入css）
3. 使用插件
```

### jquery.lazyload.js

懒加载插件

### jquery.ui.js插件

jQueryUI专指由jQuery官方维护的UI方向的插件。

官方API：http://api.jqueryui.com/category/all/

其他教程：[jQueryUI教程](http://www.runoob.com/jqueryui/jqueryui-tutorial.html)

基本使用:

```markdown
2.	1.	引入jQueryUI的样式文件
2.	引入jQuery
3.	引入jQueryUI的js文件
4.	使用jQueryUI功能
```

## 制作jquery插件

> 原理：jquery插件其实说白了就是给jquery对象增加一个新的方法，让jquery对象拥有某一个功能。

```arcade
//通过给$.fn添加方法就能够扩展jquery对象
$.fn. pluginName = function() {};
//或者直接给$添加方法
$.pluginName = function() {};
```