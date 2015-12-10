## 移动端前端开发小技巧
### 0x00 序
这片文章主要是记录了移动端前端开发中一些有用的meta以及CSS属性

### 0x01 meta
&lt;meta&gt; 标签提供了有关页面的元信息，例如作者、日期和时间、网页描述、关键词等，除了SEO，在移动端浏览器中它还有着更多的功能：

1.开启响应式视窗

```
	<!-- 视窗大小等于设备大小，初始大小为1.0，不允许用户缩放 -->
	<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no">
```

2.忽略电话号码与邮箱的识别

```
    <!-- 忽略电话号码的识别 -->
    <meta name="format-detection" content="telephone=no">

    <!-- 忽略邮箱的识别 -->
    <meta name="format-detection" content="email=no">
```

3.设置safari书签图表以及主屏幕图标

```
	<!-- 应用图标渐变 -->
    <link rel="apple-touch-icon-precomposed" href="./icon.png">
    
    <!-- 不应用图标渐变 -->
    <link rel="apple-touch-icon" href="./icon.png">
```

4.开启iphone上webapp支持

```
	<!-- 不显示默认的浏览器地址栏和工具栏 -->
    <meta name="apple-mobile-web-app-capable" content="yes"> 
    
    <!-- 顶栏颜色为white，网上能查到还可以为 black 和 black-translucent，自测IOS9.0后已失效 -->
    <meta name="apple-mobile-web-app-status-bar-style" content="white">
```

### 0x02 CSS

1.阻止移动端字体的自动调整

```
	 /* 
        1. 该属性只在移动设备上生效；
        2. 如果你的页面没有定义meta viewport，此属性定义将无效
    */
    html,body {
        -webkit-text-size-adjust:none;
    }
```

2.不显示移动端浏览器上触摸阴影以及输入框

```
	.element {
        -webkit-tap-highlight-color: transparent; 
	}
```

3.不显示移动端浏览器上的输入框内阴影

```
	.element {
        -webkit-appearance:none; 
	}
```

4.阻止长按弹出列表栏

```
	.element {
        -webkit-touch-callout:none;
	}
```

5.禁止选中文本

```
	.element {
        -webkit-user-select:none;
        user-select:none;
	}
```

10.自定义滚动条样式

```
	.element::-webkit-scrollbar{/* 1 */} /*滚动条垂直方向的宽度与水平方向的高度*/
    .element::-webkit-scrollbar-button{/* 2 */} /*滚动条按钮*/
    .element::-webkit-scrollbar-track{/* 3 */} /*滚动条轨道*/
    .element::-webkit-scrollbar-track-piece{/* 4 */} /*滚动条垂直方向轨道件*/
    .element::-webkit-scrollbar-thumb{/* 5 */} /*滚动条轨道上的按钮*/
    .element::-webkit-scrollbar-corner{/* 6 */} /*滚动条轨道上的滚动角*/
    .element::-webkit-resizer{/* 7 */} /*窗口大小调整*/

```
![scroll][1]
 
11.隐藏滚动条

```
	.element::-webkit-scrollbar {display: none;}
```
---

**本文将持续更新，欢迎补充！**

 [1]: http://iwxy.me/usr/uploads/2015/12/1091301192.jpg
