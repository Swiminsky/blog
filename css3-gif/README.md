## 0x00 序

在浏览器中，CSS 动画不会默认开启 GPU 加速，而是靠浏览器软件来渲染，但在 CSS3  中，可以“欺骗”浏览器去开启 GPU 加速，优化性能，提升动画流畅度。

我一般使用 translateZ 这个属性

    .class{
        -webkit-transform: translateZ(0);
        -moz-transform: translateZ(0);
        -ms-transform: translateZ(0);
        transform: translateZ(0);
    }

传统的 JS 动画通过定时器操控节点的位置属性，对性能的消耗会非常明显，手机上会卡的飞起，而有了 GPU 加速，CSS 动画的流畅性就是传统的 JS 动画无法比拟的了，它的渲染效率会非常高。

之前项目中在做 loading 动效时，使用的是 gif 图，一般的 gif 背景都不是透明的，而我们恰恰就需要透明背景的 gif 图。制作光滑边缘，没有锯齿的透明背景的 gif 比较麻烦，也比较困难，所以就产生了下面这种方式，借助 CSS Animation 和序列帧来逐帧播放动画的黑科技。

## 0x01 DEMO

请前往原文地址查看在线演示，传送门：[http://iwxy.me/archives/css3-gif.html](http://iwxy.me/archives/css3-gif.html)

## 0x02 CODE

    .praise{
    	width: 128px;
    	height: 128px;
        background: url(./img/loading.png) 0 0 no-repeat;
        -webkit-transform: translateZ(0);
        -moz-transform: translateZ(0);
        -ms-transform: translateZ(0);
        transform: translateZ(0);
    	-webkit-animation: load steps(29) 2.5s infinite;
    	-o-animation: load steps(29) 2.5s infinite;
    	animation: load steps(29) 2.5s infinite;
    	background-size: cover;
    }
    @-webkit-keyframes load {
    	from{
    		background-position: top;
    	}
    	to{
    		background-position: bottom;
    	}
    }
    @-o-keyframes load {
    	from{
    		background-position: top;
    	}
    	to{
    		background-position: bottom;
    	}
    }
    @-moz-keyframes load {
    	from{
    		background-position: top;
    	}
    	to{
    		background-position: bottom;
    	}
    }
    @keyframes load {
    	from{
    		background-position: top;
    	}
    	to{
    		background-position: bottom;
    	}
    }

## 0x03 EXPLANATION
通过将 gif 导出为 png 格式的序列帧，按序拼合到一张 png 中(必须要对齐，从左到右或者从上到下排列)，设动画的帧数为 N，那么 CSS animation 中的 step 就设置为 N-1 ，将 timing-function 设置为 step，就会跳跃着播放动画，也就是序列帧逐帧播放，类似于小的时候翻小人书的那个游戏。 

几个需要注意的地方：

 1. 传入 step 的步长必须为帧数-1，否则阶跃的距离将会不正确，也就不能正常播放了。
 
 2. 拼图时，必须对齐，在 PS 中需要严格对齐参考线，否则动起来会出现抖动。
 
 3. background-position 根据图片排列顺序来确定，我这里是上下排列的，所以是从 top 到 bottom 。
 
 4. background-size 必须设置为 cover，确保将序列帧塞进你的容器中。