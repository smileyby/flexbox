flexbox 全揭秘
=============

原文：[http://css-tricks.com/snippets/css/a-guide-to-flexbox/](http://css-tricks.com/snippets/css/a-guide-to-flexbox/)

弹性布局（弹性盒子，现今仍是w3c的候选推荐），目标在于，对于一个容器中各个项目块之间能够有效地防止空白的区域，并且不管空白区域是不定宽，动态的。

弹性布局背后的思想就是，使得容器中的项目快能够改变高度和宽度来最佳地填充可用空间（为了适应不同类型的设备和屏幕宽度）。一个弹性的盒子能够扩展它里面的项目块来填充空间，或者压缩他们防止溢出。

最重要的是，弹性布局与传统的布局相比是方向无关的（传统中垂直布局是块状布局，水平布局是内联布局）。传统布局能够很好的为网页工作，但是缺少弹性来支持大型和复杂的应用（特别当涉及到方向的变化、调整、拉伸和收缩等）。

注意：弹性布局主要适用于应用中的组件，是小规模的布局。而山歌布局更适合用来做大规模的布局。

## 基础知识

flexbox是一个模块而不是属性，它涉及到了一系列的属性。其中一些属性是设置在容器上的（父元素，被称为弹性容器），而一些属性是设置在子元素上的（被称为弹性项目、弹性内容）。

如果谁常规的布局是基于块状元素和内联元素的流式方向，弹性布局就是基于“弹性的流式方向”。

![弹性布局背后的主要思想](/img/1.png)

基本上，项目块要么布局在主轴（上图中从main start开始到main end结束）；
要么布局在交叉轴上（上图中从cross start开始到cross end结束）。

*	main axis-弹性容器的主轴是弹性项目块布局的重要数轴。注意，主轴不一定是水平的；它依赖于flex-direction这个属性的设置
*	main-start|main-end -弹性项目块在主轴上布局，从main-start方向开始到main-end结束
*	main-size-弹性项目块在主轴上的宽度或者高度
*	cross axis-垂直于主轴的轴线叫做交叉轴。它的方向依赖于主轴的方向
*	cross-start|cross-end -弹性项目块在交叉轴上布局，从cross-start开始到cross-end结束
*	cross size 他行项目快在交叉轴上的宽度或者高度

## 属性

display:flex | inline-flex; （运用在父元素弹性容器上）

这个属性定义出一个弹性容器，容器以内敛或者块级方式布局取决于给定的值

```css

	display: other values | flex | inline-flex;

```

请注意的是：
*	CSS3的columns属性对弹性容器是没有影响的
*	float，clear和vertical-align对弹性项目块没有影响

#### flex-direction(应用在弹性容器的父元素上)

这个属性决定主轴的方向，因为它确定了弹性项目块在弹性容器中的方向

```css

	flex-direction: row | row-reverse | column | column-reverse;

```

*	row （默认）：从左到右是ltr；从右到左是rtl
*	row-reverse：从右到左是ltr；从左到右是rtl
*	column：内容按从上到下排列
*	column-reverse：内容按从下到上排列

#### flex-flow(应用在弹性容器的父元素上)

是属性`flex-direction`和`flex-wrap`的缩写，能够同时定义弹性容器的主轴和交叉轴。
默认值是`row nowrap`

```css

	flex-flow: <'flex-direction'> || <'flex-wrap'>;

```

#### justify-content（应用在弹性容器的父元素上）

定义了主轴方向的布局，它的作用是分发除了弹性内容以外的空白区域

```css

	justify-content: flex-start | flex-end | center | space-between | space-around;

```

*	flex-start(默认)：内容从开始方向排列
*	flex-end：内容从结束方向开始排列
*	center：内容集中在中间
*	space-between：内容散落分布在基准线上；第一个内容及爱着开始线，最后一个内容紧挨着结束线
*	space-around：内容散落分布在基准线上；内容之间以相同的宽度隔开

![](/img/2.png)

#### align-item（应用在弹性容器的父元素上）

这个属性定义的是弹性项目块在交叉轴上的排列方式，可以认为是justify-content这个属性在交叉轴上的应用（前者是在主轴上）

```css

	align-item: flex-start | flex-end | center | baseline | stretch;

```

*	flex-start：内容排列在交叉轴的开始基准线处
*	flex-end：内容排列在交叉轴的结束基准线处
*	center：排列在交叉轴的中部
*	baseline：内容以他们的极基线对齐
*	stretch：上下铺满整个父容器（同样也会遵循min-width或者max-width的设置）

![](/img/3.png)

#### align-content（用用于弹性元素上）

当交叉轴方向上有多余的空白部分的时候，此属性定义了多行之间的排列方式，类似于justify-content在主轴上对单个内容的影响

注意：当容器的内容只有一行的时候，此属性没有任何影响

```css

	align-content: flex-start | flex-end | center | space-between | space-around | strech;

```

*	flex-start：多行内容排列在容器的开始基线
*	flex-end：多行内容排列在容器的结束基线处
*	center：内容排列在容器的中部
*	space-between：每行聚云分布；第一行分布在容器的开始处，最后一行分布在容器的结束处
*	strech(默认)：默认内容沾满整个剩余空间

![](/img/4.png)

#### order（应用在弹性内容上）

默认情况下，弹性项目块按原始顺序布局，不过，order这个属性可以控制项目快在容器中的顺序。

#### flex-grow（应用在弹性内容上）

定义了一个弹性项目快的弹性增长情况。该属性的值是数字，用数字来作为比例，决定弹性容器的项目块占领的可用空间大小。

如果所有的弹性项目块都设置了`flex-grow:1;`，那么每个项目快在容器中都会被设置成相同的大小。如果你给其中一个项目块设置了`flex-grow:2;`,那么这个项目快占用的空间就会是别的项目块的两倍。

```css

	flex-grow: <number> (default 0);

```

注意：负值不允许使用。

#### flex-shrink（应用在弹性内容上）

定义在弹性内容的收缩情况；同样负值也是不允许的。

```css

	flex-shrink: <number> (default 1);

```

#### flex-basis（应用在弹性内容上）

在剩余空间分配之前，定义了元素的基准 伸缩值

```css

	flex-basis: <length> | auto (default auto);

```

#### flex（应用在弹性内容上）

这个内容是flex-grow，flex-shrink，flex-basis这是哪个属性的缩写形式。第二个和第三个参数是可选的。默认是 0 1 auto。

```css
	
	flex: none | [<'flex-grow'> <'flex-shrink'>? || <'flex-basis'>];	
	
```

#### align-self （应用在弹性内容上）

这个属性能够覆盖默认的对其方式或者是align-items属性定义的方式

具体的属性value值得含义可以参考align-items的解释

```css

	align-self: auto | flex-start | flex-end | center | baseline | stretch;

```

### 例子

[http://codepen.io/HugoGiraudel/pen/LklCv](http://codepen.io/HugoGiraudel/pen/LklCv)

[http://codepen.io/HugoGiraudel/pen/pkwqH](http://codepen.io/HugoGiraudel/pen/pkwqH)

[http://codepen.io/HugoGiraudel/pen/qIAwr](http://codepen.io/HugoGiraudel/pen/qIAwr)



