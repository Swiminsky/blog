## 0x00 序

在之前的 [Flexbox Layout (1) ][1] 文章中，主要是介绍了基本的语法和实用的入门工具，这篇文章会从快捷键开始，到如何精确计算距离来阐述 Flexbox 元素强大的伸缩特性

## 0x01 开始

要研究 Flexbox 元素，首先得得到它，那么如何获得一个 flexbox 元素呢？我们知道，要获得一个 flexbox 元素，只需要在其包裹层 (即父元素) 上声明 `display: flex`，那么，该父元素下的所有元素都会成为 flexbox 元素，也称 弹性子元素，这个 `display: flex` 在各个浏览器的支持程度不同，所以我们需要添加浏览器私有前缀来保证它在不同浏览器下得到的表现是一致的，庆幸的是，强大的 emmet 已经为我们分配好了快捷键，我们只需要在文件中输入 `display: flex` 的缩写 df 按下 tab，就可以得到下面的结果：

    display: -webkit-flex;
    display: -moz-flex;
    display: -ms-flex;
    display: -o-flex;
    display: flex;

弹性父元素声明完毕，接下来可以开始正题了。

## 0x02 只需要三行代码，水平垂直居中对齐
这是 flexbox 的强大之处之一

在以前，如果要实现水平垂直居中对齐，可能会用到表格，或者是用到相对定位加上 CSS3 的 transform，它们都不是一个直接操作的方法，往往综合了许多变换，而 flexbox 打破了这一格局，使水平垂直居中变得非常的简单，那么如何实现呢？

只需要在想要进行水平垂直居中的元素的父元素上，声明如下代码即可

    display: flex;
    justify-content: center;
    align-items: center;

单词记不住？放心， emmet 完全实现了广大记不住单词的同学的梦想，除了前面提到的，只需要输入 `df` 就可以享受到 `display: flex` 带上全浏览器私有前缀的服务，还可以通过输入 `jcc` + tab ,和 `aic` + tab 获得 

    justify-content: center;
    -ms-align-items: center;
    align-items: center;

我们甚至连它的类名都不需要，就能得到水平、垂直居中的子元素，抛开浏览器兼容性不谈，在移动设备上，绝对比从前的布局方式来的高效并且简洁。

## 0x03 伸

flexbox 会被命名为伸缩盒，就是因为其强大的伸缩特性，我们知道，在弹性子元素上，可以通过 `flex` 属性来规定容器在伸缩时，弹性子元素将会如何分配空间

`flex` 其实是一个复合属性，它复合了 flex-grow , flex-shrink , flex-basis

 1. flex-grow：定义弹性盒子元素的扩展比率。
 2. flex-shrink：定义弹性盒子元素的收缩比率。
 3. flex-basis：定义弹性盒子元素的默认基准值。

经常会看到 `flex: 1;` 这样的缩写，这样的缩写的计算值为 `flex: 1 1 0;` 

先看下面这个例子

![][6]

**HTML代码如下**
  
    <div class="wrap">
        <div>a</div>
        <div>b</div>
        <div>c</div>
    </div>

**CSS关键代码如下**

    .wrap{
        margin: 0 auto;
        max-width: 600px;
        height: 100%;
        display: -webkit-flex;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .wrap div:nth-child(1){
        flex:1 1 100px;
        background: #10ab86;
    }
    .wrap div:nth-child(2){
        flex:2 2 200px;
        background: #F39C12;
    }
    .wrap div:nth-child(3){
        flex:3 3 200px;
        background: #428bca;
    }

在这个例子中，a , b , c 三个元素将剩余的宽度分成了 6 份，a 占 1 份，b 占 2 份，c 占 3 份，它们的 flex-basis 之和为 100px+200px+200px = 500px，容器的最大宽度为 600px ，**子元素的基准值之和** 并没有充满容器，所以子元素会扩张去填满容器的宽度，而 `flex-grow`，也就是 `flex` 属性中第一个值，将规定子元素如何去伸张并充满容器。

 1. 首先，计算待分配的宽度，在这里，容器最大宽度为 600px ，而子元素基准值之和为 500px ，待分配的宽度像素值为 100px

 2. 将所有 **弹性子元素** 的 `flex-grow` 值相加，在这里 a 的 `flex-grow` 值为 1，b 的 `flex-grow` 值为 2， c 的 `flex-grow` 值为3，所以它们之和为 6

 3. 每个弹性子元素的 `flex-grow` 占全部弹性子元素 `flex-grow`  之和的比值，这个比值乘上待分配的宽度像素值，即为该元素将要扩张的值

用表达式结合例子来说明，

 1. 在这个例子中，
    a ： `flex-grow: 1;flex-basis: 100px`
    b ： `flex-grow: 2;flex-basis: 200px`
    c ： `flex-grow: 3;flex-basis: 200px`

 2. 待分配的剩余空间为：`600-(100+200+200) = 100px`

 3. a 元素将要伸张的宽度值为 `100*1/(1+2+3) = 17px`
    同理，b 元素将要伸张的值为 `100*2/(1+2+3) = 33px`
    c 元素将要伸张的值为 `100*3/(1+2+3) = 50px`

 4. 所以，基准值加上每个元素伸张的值，最终我们看到的 a，b，c 三个元素的宽度值分别为 `117px，233px，250px`，如何验证？如果上面的效果图没有被缩放，打开 QQ 自带的截图工具即可


## 0x04 缩

有一句古话怎么说来着？男子汉大丈夫，能伸能缩，上面介绍了 flexbox 元素的伸张特性，接下来介绍弹性子元素的基准值在超过容器宽度时，会如何进行缩放，

看下面这个例子

![][7]

**HTML代码如下**
  
    <div class="wrap">
        <div>a</div>
        <div>b</div>
        <div>c</div>
    </div>

**CSS关键代码如下**

    .wrap div{
        height: 100px;
        line-height: 100px;
        text-align: center;
        color: #fff;
        font-family: cursive;
        font-size: 48px;
    }
    .wrap div:nth-child(1){
        flex:1 1 200px;
        background: #10ab86;
    }
    .wrap div:nth-child(2){
        flex:3 2 300px;
        background: #F39C12;
    }
    .wrap div:nth-child(3){
        flex:6 3 600px;
        background: #428bca;
    }

在这个例子中，a , b , c 三个元素的 flex-basis 之和为 200px+300px+600px = 1100px，容器的最大宽度为 600px ，所以，在这个例子中，**子元素的基准值之和** 溢出了容器，所以子元素会收缩去适应容器，相应的， `flex-shrink` 也就是 `flex` 属性中第二个值，将规定子元素如何去收缩。

 1. 在这个例子中，
    a ： `flex-grow: 1;flex-shrink: 1;flex-basis: 200px`
    b ： `flex-grow: 3;flex-shrink: 2;flex-basis: 300px`
    c ： `flex-grow: 6;flex-shrink: 3;flex-basis: 600px`

 2. 溢出的空间为：`1100-(200+300+600) = 500px`

 3. 由于设置了收缩因子，计算加权可得 `1*200+2*300+3*600 = 2600`

 4. 计算被移除的溢出量
    a：`200*1/2600*500 = 38px`
    b：`300*2/2600*500 = 115px`
    c：`600*3/2600*500 = 346px`

 5. 所以，基准值减去每个元素被移除的溢出量，最终我们看到的 a，b，c 三个元素的宽度值分别为 `162px，185px，254px`，如果上面的效果图没有被缩放，打开 QQ 自带的截图工具可以验证这一计算公式。

## 0x05 QUESTION

`flex-grow` 表示在弹性子元素总宽度比容器小的时候，为了让其填满容器，每个弹性子元素应当增加的宽度。

`flex-shrink` 表示在弹性子元素总宽度比容器大的时候，为了让其填满容器，每个弹性子元素应当减少的宽度。

为什么两个如此相近的属性，在计算伸张和缩放时，所用的公式会有差别呢？

因为我们在计算和值的时候，宽度始终比基准值大，但是在相减的时候，如果只计算赋予的 `flex-shrink` 值，那么很有可能最后减少的宽度比 `flex-basis` 大，这样一来，弹性子元素的宽度就会变成负值。

那么该如何修正呢？即把 `flex-basis` 当成参数计算进去，这样就能保证减少的宽度将永远小于 `flex-basis`，从而解决了弹性子元素宽度可能为负值的问题。

## 0x06 SUMMARY

 1. 当弹性子元素基准值之和小于父容器宽度时，`实际值 = 基准值 + 差值 * ( flex-grow / flex-grow 之和 )`

 2. 当弹性子元素基准值之和超过父容器宽度时，`实际值 = 基准值 - 溢出值 * (flex-shrink*flex-basis/flex-shrink*flex-basis 之和 )`

## 0x07 REFERENCE
 - [http://css.doyoe.com/properties/flex/index.htm][3]

 - [https://developer.mozilla.org/zh-CN/docs/CSS/Tutorials/Using_CSS_flexible_boxes][4]

 - [https://css-tricks.com/snippets/css/a-guide-to-flexbox/#flexbox-background][5]


  [1]: http://iwxy.me/archives/css-flexbox-layout-introduce.html
  [2]: http://iwxy.me/usr/uploads/2015/08/1908833263.png
  [3]: http://css.doyoe.com/properties/flex/index.htm
  [4]: https://developer.mozilla.org/zh-CN/docs/CSS/Tutorials/Using_CSS_flexible_boxes
  [5]: https://css-tricks.com/snippets/css/a-guide-to-flexbox/#flexbox-background
  [6]: http://iwxy.me/usr/uploads/2015/08/1200612496.png
  [7]: http://iwxy.me/usr/uploads/2015/08/1908833263.png