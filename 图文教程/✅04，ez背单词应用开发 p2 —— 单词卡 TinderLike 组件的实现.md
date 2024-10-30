

# 前言

在 ez背单词 APP 中，最核心的 UI 就是一个类似`Tinder`或`探探`的卡片效果的组件，本篇教程就带大家从零开发一个 TinderLiker 组件。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b1ca2fb986644d00a2163ad14e9bfcab~tplv-k3u1fbpfcp-watermark.image)
用手指触摸可以拖动卡片，并将卡片朝任意方向拖拽飞出去，效果是很酷的！

# 先叠起来

首先让三个卡片按照`近大远小`的原则分别设置设置`z-index`，宽和高，比如每一层卡片的宽和高比上一层卡片要缩小20个像素（还有一种做法是通过zoom或者scale来设置远处卡片的缩小级别）。然后加入绝对定位`position:absolute`和`z-index`就可以将卡片层叠起来了。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1de6b260984f4d3f95d5f3d91857a1d5~tplv-k3u1fbpfcp-watermark.image)

# 拖动第一张卡

因为只有第一张卡片可以拖动，所以我们其实只要监听第一张卡片的触摸事件。
`touchstart`,`touchmove`,`touchcancel`,`touchend`。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0c3d9f782df34dd4a350fb4fe85b33fa~tplv-k3u1fbpfcp-watermark.image)

由于卡片的顶点在左上角，所以拖动的时候如果我们直接设置卡片的 left 和 top，那么拖动的时候卡片左上角就会直接跳到触摸点。

为了去掉这个跳动，点哪儿拖哪儿，让拖动更自然，在触摸刚开始`touchstart`的时候记录一下手指按下时，和卡片顶点位置的差，然后在`touchmove`时减去这个位置，看上去就是非常跟手的了。

~~~javascript
touchStart:function(e){
	var curTouch=e.touches[0];
	this.startLeft=curTouch.clientX-this.left;
	this.startTop=curTouch.clientY-this.top;
}
touchMove:function(e){
	var curTouch=e.touches[0];
	this.left=curTouch.clientX-this.startLeft;
	this.top=curTouch.clientY-this.startTop;
}
~~~

# 飞出去

下面我们来实现朝任意拖拽方向飞出去的效果，这需要用到一些数学公式。

首先时计算卡片当前拖拽的坐标和起始坐标的夹角
~~~javascript
var angle=Math.atan2((当前坐标.y-起点坐标.y), (当前坐标.x-起点坐标.x));
~~~

然后是飞出去落点的`x`轴坐标，通过计算angle的余弦值再乘以力度得出
~~~javascript
this.left=Math.cos(angle)*this.throwDistance;
~~~
而飞出去落点的`y`轴坐标通过计算angle的正弦值再乘以力度得出
~~~javascript
this.top=Math.sin(angle)*this.throwDistance;
~~~

要让整个交互更舒服，咱们做得再完善一些，在拖动结束的时候判断一下当前拖动的距离是否足够触发飞卡效果。如果不触发飞卡效果，则让卡片回归到起始的位置。这样的话也可以防止用户误操作，一点击卡片就飞出去了。

~~~javascript
//计算两点之间的直线距离
getDistance:function(x1, y1, x2, y2) {
    var _x = Math.abs(x1 - x2); 
    var _y = Math.abs(y1 - y2); 
    return Math.sqrt(_x * _x + _y * _y);
}
            
var distance=this.getDistance(0,0,this.left,this.top);
if(distance>this.throwTriggerDistance){
    this.makeCardThrow();
}else{
    this.makeCardBack();
}
~~~

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0ba699a2ad0747b889f307954c51507b~tplv-k3u1fbpfcp-watermark.image)

# 下层的卡片上推

上推其实很简单，一开始的时候，我就定义了四张（`不是3张吗？怎么变4张了`）卡片的大小和位置。

当第一张卡飞出去后

- 第2张卡片变更为原本第1张卡片的位置和大小

~~~javascript
this.width2=this.cardWidth;
this.height2=this.cardHeight;
this.left2=0;
this.top2=0;
~~~

- 第3张卡片变更为原本第2张卡片的位置和大小

~~~javascript
this.width3=(this.cardWidth-this.leftPad*2);
this.height3=(this.cardHeight-this.topPad*2);
this.left3=this.leftPad;
this.top3=(this.topPad*3);
~~~

- 第4张卡片原本是透明的，现在变为第3张卡片的位置和大小

~~~javascript
this.width4=(this.cardWidth-this.leftPad*4);
this.height4=(this.cardHeight-this.topPad*4);
this.left4=this.leftPad*2;
this.top4=(this.topPad*6);
this.opacity4=1;
~~~

我把阴影效果先去掉，大家观察一下这个细节

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e9678dc3521544bfbd662b80643b752a~tplv-k3u1fbpfcp-watermark.image)

# 重置所有卡片

底层的卡片上推和第一张卡片的飞出效果是同时进行的，由css的`transition`来控制。不过时间是我们设定好的，所以只要在上推和飞出的动画时间结束后，我们重置一下所有4张卡片的大小和位置即可。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/befc7c05e10f400ebb41d65018b0e30a~tplv-k3u1fbpfcp-watermark.image)

~~~javascript
this.onThrowStart();
setTimeout(function(){
    that.isThrow=false;
    that.isAnimating=false;
    that.onThrowDone();
    that.resetAllCard();
},400);
~~~

这里需要注意，所有四张卡片都需要`瞬间`完成重置，所以这步之前应该禁用掉 transition 动画。

# 组件化

为了适应各种使用场景，我们要将这个效果封装一下。

~~~javascript
//提供几个事件，分别是拖动时，拖动结束，飞卡结束，飞卡失败（回位）
@onDragMove='onCardDragMove'
@onDragStop='onCardDragStop'
@onThrowDone='onCardThrowDone'
@onThrowFail='onCardThrowFail'

//参数就不细说了，都能看明白
:cardWidth="200" 
:cardHeight="200"
cardBgColor="#fff"
:leftPad="10"
:topPad="6"
:borderRadius="8"
:throwTriggerDistance="100"
dragDirection="all"
:hasShadow="false"
:hasBorder="true"
~~~

提供两个slot，用于往卡片里防止任意内容
~~~javascript
//firstCard，secondCard，thirdCard
<slot name="firstCard"></slot>
~~~

# 现在来模仿几个效果

## 某乎的推荐回答

~~~javascript
@onDragMove='onCardDragMove'
@onDragStop='onCardDragStop'
@onThrowDone='onCardThrowDone'
:cardWidth="300" 
:cardHeight="120"
:throwTriggerDistance="100"
dragDirection="horizontal"
:hasShadow="true"
~~~

设置成仅允许水平拖动

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a703a3f3f43446e3b0e7ea026b8e2415~tplv-k3u1fbpfcp-watermark.image)


## 探探的效果

实现探探效果的核心是通过实时监听卡片拖动时处于的位置，在屏幕左边就显示不喜欢，在屏幕右边就显示喜欢。

~~~javascript
onCardDragMove(obj){
    if(obj.left<-10){
        this.actionName="不喜欢";
    }else if(obj.left>10){
        this.actionName="喜欢";
    }else{
        this.actionName="";
    }
}
~~~

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4fcdac5da3524ad6bd682449596b78f9~tplv-k3u1fbpfcp-watermark.image)


---
添加大帅老师微信（itdashuai）进入答疑群获取源码和用于测试的苹果开发者证书