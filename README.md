
#实验要求
1.@+环形菜单出现在页面下部，靠近底边，居中
2.其关闭时（鼠标未指向），只显示缩小的@+按钮，效果如图：
3.鼠标指向之后，显示出环形菜单，包括5个控制按钮和一个信息栏（info-bar）
4.具体的交互过程，参考下面的短片：
5.颜色：按钮蓝色：rgba(48, 63, 159, 1)；绿色字体：#079E6E；各种灰色，请自行用rgba来调整。


##构思： 
1.怎么用css实现圆形边框？ 
很简单，将边框设置为一个正方形，然后再使用属性border-radius:50%; 便可完成圆形边框的设置。

2.怎么实现圆形边框内部图片的居中设置且有合适大小？ 
很简单，通过使用background-size属性可以调整背景图片的大小，通过使用backgroud-position:50% 50%可以实现图像的居中设置。

3.怎么实现效果图中各圆圈所在的位置？ 
这个就涉及到元素的布局与定位。对元素的定位我都使用的是绝对定位，即position:absolute,然后通过调整相关属性(我是调整left与bottom)实现效果图中的布局。

4.怎么实现圆圈内各元素的布局？ 
这个也是涉及到元素的布局与定位。对元素的定位我还是使用绝对定位，这样一来它就会参照圆圈的边界进行定位。

5.怎么实现环形菜单的动画效果呢？ 
这个就是项目的核心了！这个效果可以通过纯css实现而不依赖JS。此处我采用的是伪类:hover，以实现鼠标滑过元素时的动态效果。 
那么这里有一个难题：怎么实现多个元素同时被触发？在这里也就是：怎么实现当鼠标停留在@上时，六个小圆圈一起出现呢？这就说明这几个:hover的效果是由同一个触发器触发的。那怎么实现一个触发器触发所有元素的:hover效果呢？这就涉及到了元素的嵌套。 
我们可以将这几个元素嵌套在一个div中(id = “button”)，然后通过#button:hover .E {} 设置相应的属性。这里的E是指六个小圆圈的class名，而我称这个选择器为伪类子选择器，它可以实现对button子元素:hover效果的设置，并同时实现只通过一个触发器来触发所有子元素:hover效果。

6.在未触发状态下，除了@圆圈，其它元素是消失的。但是一旦触发，所有元素都出现了，这又是怎么实现的呢？
一开始我想到的是display:none属性与visibility:hidden属性，但是实践证明，它不能实现我想要的效果。那么，我还有其他办法来解决这一问题么？很简单！对于元素有一个属性可以解决这个问题，那就是透明度 opacity。opacity:1表示不透明， opacity:0.5表示半透明， opacity:0表示全透明。透过设置.E {opacity = 0;}与.E {opacity=1;}来实现这一效果。

7.怎么实现一个缓冲，使得动画不那么快结束？

在这里可以使用过渡的属性。对于过渡，它的实现有四点要求：(1)初始条件；(2)终止条件; (3)过渡器; (4)触发器。如下：


````
/* :hover 是一个触发器 */
li.mask {
    bottom: 16%;
    left: 50%;

    -webkit-transition: all 500ms ease-out 250ms;
       -moz-transition: all 500ms ease-out 250ms;
         -o-transition: all 500ms ease-out 250ms;
            transition: all 500ms ease-out 250ms;

    opacity: 0;
    background: url(assets/images/nomask.png) no-repeat 50% 50% #666;
    background-size: 0;
}
div#button:hover .mask {
    bottom: 21%;
    left: 39%;

    opacity: 1;
    background: url(assets/images/nomask.png) no-repeat 50% 50% #666;
    background-size: 50%;
}
```
8. 我发现浏览器窗口会影响我的动画布局？ 
采用冻结布局的思想，也就是在我们的环形菜单嵌套一个隐形的div，设置好它的宽度即可。# RingMenu
