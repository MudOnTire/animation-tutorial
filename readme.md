# 简介

Animation可以让你不用依赖javascript或jquery，用纯CSS在网页中轻松实现各种动画效果。

# 兼容性

animation在绝大部分主流浏览器都得到了很好的支持！还在兼容IE9的同学要谨慎使用。

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/d28b6f5d27cf941bb695.png)

# CSS 坐标系

在了解animtion之前，我们有必要先了解css的坐标系，因为很多的animation效果都和元素的坐标密切相关。在css3中网页不再是一个二维平面，而是一个三维空间，水平方向、竖直方向和垂直屏幕方向分别对应三维坐标系的x，y，z轴，如下图所示。箭头方向为正向，反之为负向（注意y轴方向与常规笛卡尔坐标系相反）。

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/f6d8522c2883ce82a70d.svg)

# Animations

## 1. Transforms

transform中文译为“转换”，但我更倾向于称呼它“变形”（大名鼎鼎的变形金刚叫transformer）。我们可以使用transform function使html元素产生各种各样的变形，比如平移、缩放、旋转、扭曲等，而且不会影响正常的文档流（document flow）。

### (1) Translate

Translate一般译为“翻译”，但在css里面一般用作“平移”，因为translate用于改变html元素的在3d坐标系位置。translate支持在坐标系内任意方向移动（通过任意组合x、y、z方向的向量），单位可以是长度单位和百分比（**百分比是相对于被平移的元素自身尺寸，x轴是相对于width，y轴是相对于height，而在z轴方向由于元素是没有‘厚度’的，所以对于z方向不能用百分比表示**），例如：

```
transform: translateX(100px) translateY(50%) translateZ(-100px);
// 或者简写
transform: translate3d(100px, 50%, 2em);
```

**注意：**

1. `translate`是xy平面内的2维平移，`translate3d`是xyz空间内的三维平移；

1. `translate`也可以单独书写，如 `translate: 50% 105px 5rem;`，但是该特性尚在实验阶段，很多浏览器不支持，所以现阶段还是配合`transform`使用。详情参考 [MDN translate](https://developer.mozilla.org/en-US/docs/Web/CSS/translate)。

### (2) Scale

Scale意为“缩放”，顾名思义，是用于改变元素的大小。例如：

```
transform: scaleX(2) scaleY(0.5) scaleZ(1);
// 或者简写
transform: scale3d(2, 0.5, 1);
```
`scale`方法接收任意数字（正负整数、小数、0）作为参数，该参数为缩放系数，系数&gt;1 效果为放大，0&lt;系数&lt;1 效果为缩小，系数=0 元素尺寸变为无限小而不可见，系数&lt;0 效果为 &gt;0 时的镜像（具体效果可自己实验）。

与`translate`类似，`scale`也有2维 `scale()` 和三维 `scale3d()`之分，也可以单独书写，此处不赘述。

### (3) Rotate

Rotate意为“旋转”，支持将元素以x、y、z为轴旋转，旋转正方向为面朝坐标轴正向逆时针方向，可参考上面坐标系示意图。`rotate`方法接收一个角度作为参数，角度>0 正向旋转，角度&lt;0 负向旋转，例如：

```
// 默认绕z轴旋转
transform: rotate(90deg);

transform: rotateX(30deg) rotateY(60deg) rotateZ(-90deg);
```

与translate和scale不同，rotate不能简写为`transform: rotate3d(30deg, 60deg, 90deg)`的形式，`rotate3d`的用法为：前三个参数为数字，代表x、y、z方向的向量，相加得到向量v，第四个参数为角度，表示以向量v为轴逆时针旋转的角度，语法如下：

```
transform: rotate3d(1, 2, 3, 90deg);
```

与`translate`和`scale`类似，`rotate`也可以作为单独的css属性，但还在实验阶段。

**出于篇幅考虑，我只列出三种最常用的tranform function，剩下的transform function请参考  [MDN transform function](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-function)。**

### (4) 组合

我们可以将不同的transform方法组合起来使用，如：

```
transform: translateY(200px) rotateZ(90deg) scale(3);
```

组合方法的执行顺序是从右往左，即先执行scale，然后rotate，最后translate，产生的效果是逐次累加的。方法书写的顺序对最后效果有很大的影响，看下面例子，沿y轴平移和放大，顺序不同，产生的结果有明显差别：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/eb206653fc5d4732448a.png)

如果先translate再scale，平移的距离也将被等比例缩放，而先scale再translate则不会。所以在使用transform需按照 `translate -> rotate -> scale` 的顺序书写，让scale先执行，以免放大translate的效果，而rotate先translate执行以防止带着平移的距离一起转动。我觉得这是transform目前不方便的地方，因为方法之间相互干扰并不容易理解，书写顺序也不容易记住。在未来有望通过使用独立的css transform属性解决这一问题，因为独立的transform属性对书写顺序没有依赖，方法之间彼此不会干扰。

## Transition

Transition翻译为“过渡”，强调的是过程。在css中指在一段时间内，元素从一个状态（对应一个css属性）过渡到另一个状态的动态过程。我们可以决定以何种方式过渡过渡和花费多少时间。

例如，我们把鼠标悬浮到云上面的时候使其变大一些可以这么写：

```
.cloud{
	width: 240px;
	transition: 1s;
}
.cloud:hover{
	width: 320px;
}
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/6f3af0717d2687268642.gif)


transition可以和transform结合使用，比如我们可以让云变大的同时转一圈：

```
.cloud:hover{
	width: 320px;
	transform: rotate(360deg);
}
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/c755efd873ec1b33f976.gif)

我们可以给不同的效果设置不同的过渡时间：

```
.cloud{
	width: 240px;
	transition: width 1s, transform 0.5s;
}
```


我们也可以给效果设置延时时间，比如我们等宽度增大之后再旋转：

```
.cloud{
	width: 240px;
	transition: width 1s, transform 0.5s 1s;
}
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/64d67266496ecc4c751e.gif)


我们还可以给每个效果设置不同的timing function，用于控制加速效果。

比如我们可以让旋转的速度逐渐加快：

```
.cloud{
	width: 240px;
	transition: transform 2s ease-in;
}

.cloud:hover{
	transform: rotate(1080deg);
}
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/f5866c1f3dee91b196c9.gif)

更多的timing function请后面会进一步讨论，也可以参考 [MDN transition-timing-functions](https://developer.mozilla.org/en-US/docs/Web/CSS/single-transition-timing-function)


## Keyframes

### (1) 基本用法

Keyframe中文译为“关键帧”，是animation中很强大的功能，通俗说就是我们可以通过定义一段动画中的关键点、关键状态来创建动画。Keyframes相比transition对动画过程和细节有更强的控制。

我们先看一个例子（部分代码省略）

html:

```
<div class="sky"></div>
<div class="grass"></div>
<div class="road">
  <div class="lines"></div>
  <img src="http://lc-jOYHMCEn.cn-n1.lcfile.com/a3f630c957a4b32d0221.png" class="mario animated">
</div>
```

css：

```
.mario{
  position: absolute;
  left: 0px;
  width: 100px;
}

.animated{
  animation-name: drive;
  animation-duration: 1s;
  animation-timing-function: linear;
}

@keyframes drive {
  from{ transform: translateX(0) }
  to{ transform: translateX(700px) }
}
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/3e4463d79bed71a070be.gif)

其中 `drive` 是该keyframes的名称，`from, to` 是keyframes播放过程的时间起点和终点，时间点也可以用百分比表示，如`50%`，`from， to` 等价于 `0%, 100%`。每个时间点对应一个css状态，代表一个关键帧（keyframe）。keyframes定义完成后使用方法如下：

animation也有简写形式，如：

```
animation: slidein 3s ease-in 1s infinite reverse both running;
```

但这种对书写顺序有一定要求（delay要写在duration后面，其他参数无顺序要求，css会通过传入的关键词识别）。

### (2) Animation Delay

通过`animation-delay`，我们可以给动画延迟执行：

```
animation-delay: 2s;
```

### (3) Animation Fill Mode

#### forwards

在上面的例子中可以看到马里奥运动到右边之后又回到了起点，如果我们想让他运动完成后就停留在右边呢？很简单，我们设置annimation fill mode就可以了：

```
animation-fill-mode: forwards
```

`forwards` 表示动画完成后，元素将保持最后一帧的状态。

#### backwards

与之相对的还有 `backwards`，`backwards`表示并不是动画完成后元素变回第一帧的状态，而是表示当设置了`animation-delay`时，在动画开始前的等待过程中，立刻给元素应用第一帧的状态。我们将上面的例子稍作修改看一下效果：

```
.animated{
  animation-name: drive;
  animation-duration: 1s;
  animation-fill-mode: backwords;
  animation-delay: 1s;
  animation-timing-function: linear;
}

@keyframes drive {
  from{ transform: translateX(350px) }
  to{ transform: translateX(700px) scale(2) }
}
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/3527b902dc1e9295bdd4.gif)

可以看到，动画开始之前小人立马移动到350px处，1s之后才开始动画。

#### both

显而易见，both会同时应用forwards和backwards两种规则，即在delay时先应用第一帧的状态，结束时保持最后一帧的状态。

### (3) Animation Repeat

我们可以通过 `animation-iteration-count` 设置动画循环播放的次数，比如：

```
animation-iteration-count: 3;

// 无限循环
animation-iteration-count：infinite;
```

就像这样：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/f0c8ab9f677a99ceebdf.gif)

### (4) Animation Direction

#### normal

正常方向，也是默认方向，即先from，再to。

#### reverse

与正常方向相反，即先to，再from。例如：

```
.animated{
  ...
  
  animation-direction: reverse;
}

@keyframes drive {
  from{ transform: translateX(-100px) rotateY(180deg) }
  to{ transform: translateX(862px) rotateY(180deg)}
}
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/419478c1d4f5f6732b5a.gif)

#### alternate

alternate意为“交替”，即normal和reverse交替之行，先normal再reverse。

#### reverse alternate

反向交替，先reverse再normal。


### (4) Animation Timing function

和transition一样，keyframes也可以设置timing function，常用的timing function归纳如下：

1. ease：默认方法，初速度较慢，然后加速再减速
2. ease-in：初速度最慢，然后一直加速
3. ease-out：初速度最快，然后一直减速
4. ease-in-out：初速度较慢，然后加速再减速，与ease的区别在于加速减速过程是对称的
5. linear：恒速运动

直观表现如下（[codepen](https://codepen.io/mudontire/pen/vvxbbE)）：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/7e256147ff8695cc3b98.gif)

除了上面现成的方法，我们可以通过贝塞尔曲线自定义速度曲线。我们可以在 [http://cubic-bezier.com](http://cubic-bezier.com) 可视化的创建我们自己的贝塞尔曲线。比如创建一个刚开始极慢，突然变得极快的曲线：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/6a743898fd73efb5946f.png)

css:

```
animation-timing-function: cubic-bezier(1,.03,1,-0.03);
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/c2a1c0ff4c9e4f50fe16.gif)

是不是挺神奇！

### (5) Chain Animation

我们可以将多个animation串联使用，比如我们想让小人在行驶的过程中跳跃，可以这么写：

css: 
```
.mario {
  ...
  
  animation: drive 3s both infinite linear, jump 0.5s 1.2s ease-in-out infinite;
}

@keyframes jump {
  0% { top: -40px; }
  50% { top: -120px; }
  100% { top: -40px; }
}
```

效果：

![image](http://lc-jOYHMCEn.cn-n1.lcfile.com/620b32b6f0abd6f50f52.gif)


## 实践

本文目的在于普及css3 animation的基础，并未完全覆盖animation的知识，未涉及和讲解的知识请大家见谅 。 掌握上述知识后，我们就已经可以用animation做出丰富的动画效果了，下面列出一些codepen上的小例子：

1. [full mario demo](https://codepen.io/mudontire/pen/rowOMr)
2. [animated popup](https://codepen.io/mudontire/pen/MZoavX)
3. [fly items to shopping cart](https://codepen.io/mudontire/pen/VqWebb)
4. [flipping cards](https://codepen.io/mudontire/pen/JwJGvE)