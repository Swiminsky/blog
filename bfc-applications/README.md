# 细谈BFC
***
## 0x01 BFC概念

FC: Formatting Context，格式化上下文，指页面中的一个渲染区域，拥有独立的渲染规则，它决定了其子元素如何定位，以及与其他元素的相互关系和作用。

BFC：Block Formatting Context，块级格式化上下文，它是指一个独立的块级渲染区域，只有块级盒参与，该区域拥有一套渲染规则来约束块级盒子的布局，且与它外部无关

## 0x02 创建BFC

创建BFC的方法可归为以下几种

 1. 根元素
 2. float 除了 none 以外的值 ( left、right、inherit)
 3. overflow 除 visible 以外的值 ( hidden、auto、scroll)
 4. display (table-cell、table-caption、inline-block、flex、inline-flex)
 5. position 值为 absolute 或 fixed
 6. fieldset 元素

以上几种情况，可以创建出 BFC

## 0x03 BFC的约束规则

浏览器对 BFC 这块区域的约束规则如下：

 1. 生成 BFC 元素的子元素会一个接一个的放置，垂直方向上他们的起点是一个包含块的顶部，两个相邻子元素之间的垂直距离取决于元素的 margin 特性，在 BFC 中相邻的块级元素外边距会折叠。
 2. 生成 BFC 元素的子元素中，每一个子元素做外边距与包含块的左边界相接触，（对于从右到左的，右外边距接触右边界），即使浮动元素也是如此（尽管子元素的内容区域会由于浮动而压缩），除非这个子元素也创建了一个新的 BFC 。

虽然这里的约束规则只分了两条，但是将这两条规则分解之后，可以总结为

 1. 内部的 BOX 会在垂直方向上一个接一个的放置
 2. 垂直方向上的距离由 margin 决定，同属于一个 BFC 的两个相邻 BOX 的 margin 会发生重叠
 3. 每个元素的左外边距与包含块的左边界相接触（从左到右），即使浮动元素亦如此，（这也说明 BFC 中的子元素不会超出它的包含块）
 4. BFC 的区域不会与 float 的元素区域重叠
 5. 计算 BFC 的高度时，浮动子元素也参与计算
 6. BFC 就是页面上的一个独立容器，独立容器内部的元素不会受到外部的影响，反之也是一样

## 0x04 BFC应用

 **1. 解决折叠 margin 重叠的问题**

先来看这样一段代码

    <style>
      .a,.b,.c {
        width: 200px;
        height: 130px;
        margin: 25px;
        background: #428bca;
      }
    </style>

      <div class="a">Hello a</div>
      <div class="b">Hello b</div>
      <div class="c">Hello c</div>

在这样一段代码中，因为 a,b,c 这三个类的 margin 值均为 25px，一般地，我们会认为这里 a 和 b ，b 和 c 之间的间距为 50px，但其实不然，先看看 chrome 调试器下会出现什么状况

![bfcmargin.png][2]

我们明显的看见，这里 margin 是重叠的，正解释了为什么这里 a 和 b ，b 和 c 之间的间距为 25px 而不是 50px

那么要如何解决这个 margin 重叠的问题呢？那就是为每一个元素添加一个包裹层，创建独立的 BFC 上下文

    <style>
      .a,.b,.c {
        width: 200px;
        height: 130px;
        margin: 25px;
        background: #428bca;
      }
    
      .bfc {
        overflow: auto;
      }
    </style>
      <div class="bfc">
        <div class="a">Hello a</div>
      </div>
      <div class="bfc">
        <div class="b">Hello b</div>
      </div>
      <div class="bfc">
        <div class="c">Hello c</div>
      </div>

这样一来，每一个 BFC 区域内都拥有独立的上下文，并且不再受到外部的影响，在这里，a 和 b ，b 和 c 之间的间距才是 50px。

 **2. 为多栏布局提出一种解决方案**

在常见的双栏布局中，我们也许会用这样的方式

    <style>
      .left {
        width: 100px;
        height: 100px;
        float: left;
        background: #3BD49E;
      }
      .right {
        background: #428bca;
        height: 150px;
      }
    </style>
    
      <div class="left">I'm left</div>
      <div class="right">I'm right</div>

当右栏的高度超过了左栏时，我们会发现实际上右栏会把左栏包裹在内，但是这并不是我们想要的效果

![shuanglan1.png][3]

当我们了解了 BFC 的原理之后（参见第四条约束规则），只需要加上一行代码就可以完美的解决这个基础的双栏布局问题，那就是为 right 创建一个 BFC

重写 right 部分的 CSS

      .right {
        background: #428bca;
        height: 150px;
        overflow:auto;
      }

效果如下：

![shuanglan2.png][4]


 **3. 解决包裹浮动的问题**

典型的解决包裹浮动的问题就是四宫格的解决方案，如何创建一个四宫格？也许会这样写

    <style>
    .red {
      background: red;
      width: 100px;
      height: 100px;
      float: left;
    }
    
    .orange {
      background: orange;
      width: 100px;
      height: 100px;
      float: left;
    }
    
    .yellow {
      background: yellow;
      width: 100px;
      height: 100px;
      float: left;
    }
    
    .green {
      background: green;
      width: 100px;
      height: 100px;
      float: left;
    }
    </style>
    
    <div class="A">
      <div class="red"></div>
      <div class="orange"></div>
    </div>
    <div class="B">
      <div class="yellow"></div>
      <div class="green"></div>
    </div>

很显然这无法达到我们的要求，为解决浮动布局不换行的问题，通过理解 BFC 的概念，为 A 创建 BFC ，包裹浮动元素，就可以实现

在 CSS 代码中新增 overflow:auto; 为其创建 BFC 

    .A {
      overflow: auto;
    }

效果如图：

![sgg.png][5]

 **4. 解决浮动塌陷的问题**

什么是浮动塌陷？看下面这个例子

    <style>
      .parent {
        border: 2px solid #428bca;
      }
      .child {
        width: 100px;
        height: 100px;
        float: left;
        background: #3BD49E;
      }
    </style>

    <div class="parent">
        <div class="child"></div>
    </div>



上述代码的效果就会是这样，可能和你想的有一些出入

![yichu.png][6]

这其中的原理是这样：

 - 如果父元素只包含浮动元素，且父元素未设置高度和宽度的时候，那么它的高度就会塌缩为零。

 - 如果父元素不包含任何的可见背景，这个问题会很难被注意到，但是这是一个很重要的问题，上面的代码就是一个典型的例子。

为解决浮动塌陷的问题，也可以用 BFC 来解决（参见第五条约束规则）

为其父元素创建 BFC

修改父元素的 CSS 代码：

      .parent {
        border: 2px solid #428bca;
        overflow: auto;
      }
![yichu2.png][7]

## 0x05 Summary

**兼容：**在 IE6 IE7 IE8 中只有 hasLayout 特性，而没有BFC的概念，所以若要兼容低版本的 IE 浏览器，还需要做其他的处理，而我对兼容的看法就是，针对产品的面向对象来确定兼容方案。

**实用：**通过理解 BFC，可以非常轻易地解决很多平时经常会遇到的问题，如：解决 margin 重叠、布局、实现浮动包裹、解决浮动塌陷。


  [1]: http://iwxy.me/usr/uploads/2015/07/3603214173.png
  [2]: http://iwxy.me/usr/uploads/2015/07/633944288.png
  [3]: http://iwxy.me/usr/uploads/2015/07/3242030061.png
  [4]: http://iwxy.me/usr/uploads/2015/07/4017755204.png
  [5]: http://iwxy.me/usr/uploads/2015/07/2363049427.png
  [6]: http://iwxy.me/usr/uploads/2015/07/204279401.png
  [7]: http://iwxy.me/usr/uploads/2015/07/3540765952.png