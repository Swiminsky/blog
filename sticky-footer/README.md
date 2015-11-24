什么是页脚固定底部的布局？

就从字面上理解，
  1. 当页面高度不足以撑满浏览器视窗时，页脚固定在视窗底部
  2. 当页面高度能够撑满浏览器视窗时，页脚在页面尾部

这种需求使用CSS或JS都可以实现，推荐的是 **能用CSS实现的效果，就不要使用JS**

这里推荐两种常用的方式，适用于两种不同的DOM结构 [示例1][1]、[示例2][2]

## 方法一
这种结构下，footer 和 container 属于同辈关系，可以使用负边距来实现，Let's see the [demo][1]!

    <div class="container">
        <div class="page"></div>
    </div>
    <div class="footer">Footer</div>

关键CSS代码如下

    <style>
        html,body{
            margin: 0;
            padding: 0;
            height: 100%;
        }
        .container{
            min-height: 100%;
        }
        .page{
            /* same as the footer height */
            padding-bottom: 100px; 
        }
        .footer{
            /* footer height */
            height: 100px;
            /* height的负数 */
            margin-top: -100px; 
        }
    </style>


## 方法二
在这种结构下，footer 是 container 的子元素，使用定位元素 relative、absolute 来布局，See the [demo][2]!

    <div class="container">
      <div class="page"></div>
      <div class="footer">Footer</div>
    </div>

关键CSS代码如下

    <style>
        html,body{
            margin: 0;
            padding: 0;
            height: 100%;
        }
        .container{
            position: relative;
            min-height: 100%;
        }
        .page{
            padding-bottom: 100px;
        }
        .footer{
            position: absolute;
            bottom: 0;
            width: 100%;
            height: 100px;
        }
    </style>

页面开发，布局为大。

以此记录两种 DOM 结构下的页脚解决方案。





  [1]: http://iwxy.me/demo/stickyfooter/m1.html
  [2]: http://iwxy.me/demo/stickyfooter/m2.html