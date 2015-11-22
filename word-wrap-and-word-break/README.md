
## 0x00 序

对于连续长字符的换行，一直以来都是用的 `word-break:break-all;`，前几天同事问我，word-wrap 是怎么用的？和 word-break 有什么区别？我只知道它们都可以解决连续长字符换行的问题，有什么区别还真说不上来，后来查了些资料，总结了一些异同。

## 0x01 word-wrap

**取值：**<br>
`normal`：允许内容顶开或溢出指定的容器边界。

`break-word`：内容将在边界内换行。如果需要，单词内部允许断行。

word-wrap 属性原本属于微软的一个私有属性，在 CSS3 现在的文本规范草案中已经被重名为 overflow-wrap ， word-wrap 现在被当作 overflow-wrap 的 “别名”。 

稳定的谷歌 chrome 和 Opera 浏览器版本支持这种新语法，其它浏览器对它的兼容性并不好，当使用 overflow-wrap 时，最好同时使用 word-wrap 向前兼容。

## 0x02 word-break
**取值：**<br>
`normal`：依照不同语言的文本规则，使用默认的断行规则。

`keep-all`：中文/日文/韩文的文本不断行，除此之外文本表现同 normal。

`break-all`：对于非中文/日文/韩文的文本，可在任意字符间断行。

## 0x03 demo

文档并没有很清楚的说明这两个属性的不同到底在哪，那么来看看这一个例子。

先看看对于连续长字符，默认的断行规则。

**code**

    <style>
    .a,.b{
    	margin: 20px;
    	padding: 5px;
    	width: 100px;
    	border: 2px solid #12a7b7;
    	color:#428bca;
    }
    </style>

    <div class="a">aaaaaaaaaaaaaaaaaaaaaa</div>
    <div class="b">bbbbbbbbbbbbbbbbbbbbbb</div>

![屏幕快照 2015-11-01 18.42.38.png][1]

可以看到，对于这种只存在连续长字符的情况，默认的换行规则是 **不换行**。

下面为两个 div 分别设置 word-wrap 和 word-break

    .a{
    	word-wrap: break-word;
    }
    .b{
    	word-break: break-all;
    }

![屏幕快照 2015-11-01 18.45.31.png][2]

我们看见，这种情况下这两个属性使用起来并没有区别，也就是说具有相同的表现。

再看看这种情况，文本中除了连续长字符，还有其它字符

    <div class="a">你好！ aaaaaaaaaaaaaaaaaaaaaa</div>
    <div class="b">你好！ bbbbbbbbbbbbbbbbbbbbbb</div>

![屏幕快照 2015-11-01 18.50.51.png][3]

发现差异了，在这种情况下，使用了 `word-wrap: break-word;` 它会首先尝试挪到下一行，看下一行的宽度够不够，不够的话就进行连续字符的断句。

而 `word-break:break-all` 会直接进行字符内的断句。

## 0x04 Summary

一句话总结，`word-wrap:break-word` 和 `word-break:break-all`，它们的相同点都是换行，不同点是前面有其它字符的时候，要不要另开新行。

## 0x05 Referrence

 - [http://css.doyoe.com/][4]

 - [https://developer.mozilla.org/en-US/docs/Web/CSS/word-break][5]

 - [https://developer.mozilla.org/en-US/docs/Web/CSS/word-wrap][6]


  [1]: http://iwxy.me/usr/uploads/2015/11/4061879716.png
  [2]: http://iwxy.me/usr/uploads/2015/11/2469248921.png
  [3]: http://iwxy.me/usr/uploads/2015/11/3907903962.png
  [4]: http://css.doyoe.com/
  [5]: https://developer.mozilla.org/en-US/docs/Web/CSS/word-break
  [6]: https://developer.mozilla.org/en-US/docs/Web/CSS/word-wrap