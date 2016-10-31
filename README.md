#Project of Exteta
#### 前言
>[exteta](http://wonderff.github.io/exteta/)是一个采用 `css3` + 	`javascript`完成的项目，当第一次见到这个网站的时候觉得这个设计实在是惊为天人，作为一个前端也确实应该阶段性的做一点自己的东西，从国庆回来敲下第一行代码,到现在把它部署在github上，确实是一件很有成就感的事情。也所以，写一写关于这个项目的碎碎念，正如我之前所说的，打个路标，告诉别人自己的路也曾经兜兜转转来过这里。哈哈有点文青了，好了我们走入正题吧。

## canvas和svg

如你所见，这个项目的主页其实最主要的就是中间的那个钟表，纠结了很长时间要不要用插件，后来想想，无非就是270条线，那么我自己来搞吧。
首先我敲定的方案是`canvas`来解决这个钟，所以我在09.20前后都在慕课网学习canvas，并且完成了一个比较有模有样的[粒子时钟](https://wonderff.github.io/demos/aCanvasClock/canvasClock.html),并且在这基础之上完成了一个另外一个略复杂一点的[粒子图片动画](https://wonderff.github.io/demos/canvasformPic/canvasPic.html),其实我对前端的理解就是确实有很多的新技术在推出来，可是只要玩好`javascript`，那么不管出来什么东西我们都能很容易的上手，我们应该理解一个新技术是基于什么理由才推出来的，并根据自己的需求再来决定要不要学习它，盲目的追新只能是东一榔头西一棒槌。就自己而言，目前会的东西太少，尽可能多的深入学习项目中会用的东西提升自己才是首要的。
扯远了，项目的第一个版本我是拿`canvas`绘制了中间的钟盘，但是随着项目的进展，我发现了可能`canvas`并不能完成我的这个项目，因为在`canvas`中提供的监测鼠标的api非常有限,基于插件的话，本身就并不是一个很大的效果，个人还是倾向于写原生。
	
	context.isPointInPath(x, y)

所以在这个时候，我查了一些很多关于`html`绘图的一些文章，当然也给出了很多的解决方案，不过提到的最多的是`svg`。

啥？这是什么鬼？我听都没听过，`w3c`是这样说的：

* SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
* SVG 用来定义用于网络的基于矢量的图形
* SVG 使用 XML 格式定义图形

`xml`我听过啊，既然是基于`xml`的话，那么肯定可以选中啊什么的，毕竟是标签啊，有`DOM`模型啊，所以呢，我就打消了拿`canvas`绘制钟盘的念头，转向了`svg`的怀抱。

	console.log("节操");
	
其实看文档完全是可以学会`svg`的，不过我更倾向于看视频的学习方式，毕竟有人说话，我就知道自己跑偏了没有，以及一个简单的标签，人家是怎样玩出花来的。额，好像回到了上大一学高数的时候，上课讲的东西就那么一会事，课后练习各种不会的黑暗岁月。

哪有这么惨，只不过跟着人家搞一个实例的帮助是很大的，毕竟眼睛好用，手却并不总是好用。同样的，在慕课网学习svg的过程中间我也搞了一个高大上的demo，包震撼，[高大上svg效果](https://wonderff.github.io/demos/svg_06text.html)。

别走啊，看官，开个玩笑不行吗？[svg灯塔](https://wonderff.github.io/demos/svg_10starSky.html)。完成了这个练习之后，我才开始写整个项目，写整个项目的时候，我脑子里还没有回过神来，一直想着别人拿svg写的神奇的效果，经常在睡不着的时候脑补，加一个怎样的效果比较炫酷并且和我项目比较贴近。毕竟好用的小插件实在是太多，甚至想着svg既然是处理线条的能力这么强大，我可不可以在我的项目上做一点优化，做一个加载动画，把本人的照片处理成线稿，导出svg，然后绘制它？额 ，人丑还是不要吓人了，以后有空了找一个海贼王路飞的图片，处理成线稿试一下，看看效果怎么样。

##走进误区

敲下这个小标题，忽然感觉有点伤。

基于上述的一直脑补效果的后遗症，我在这个项目上犯了一个致命的错误，因为我想着这个项目也就是一些动画的叠加才出来的效果，那么我是不是可以做一个加法，先写好各种小的效果，最后搭积木一样把它拼接起来？

当然是可行的，我就开始愉快的写项目了，先写个表盘呗。

* 注意，svg的rotate等变换和css3 的rotate的变换有很大的类似，但是略有区别，如果直接用css3的rotate思维写svg的相关样式，会掉坑的。

另外有一点值得注意的是，虽然svg 有一套`SMIL`动画实现方案，但是chrome等浏览器会报如下的错误

>SVG's SMIL animations (<animate>, <set>, etc.) are deprecated and will be removed. Please use CSS animations or Web animations instead.

意思是`SMIL`动画和 `css3`动画有很多相似的地方，所以chrome建议我们在css中写svg动画，或者拿脚本控制，旧有的规范将被淘汰。

例如css结构如下：
	
	@keyframes move {
		from {
			stroke-dashoffset: 1000;
			stroke: #000;
			fill:rgba(0,0,0,.4);
		}
		50%{
			stroke: #000;
			fill:rgba(0,0,0,.3);
		}
		to {
			stroke-dashoffset: 0;
			fill:rgba(0,0,0,1);
			stroke: rgba(0,0,0,1);
		}
	}
	
好吧，这种写法是不是很熟悉？

当然我不是说svg这种动画写法的优越性，我只是在说我掉坑的故事不是？

基于我做加法的项目思路，我把项目进行了拆分，在没有完成主引导动画的前提下，我开始写各种小的模块，钟表，背景的过渡动画，按钮的拖拽，按钮的自动旋转，标题栏的进场动画，按钮的绘制动画，钟盘中心icon的扇形动画，等等全都逐一实现，最后导致了一个问题，即动画事件激发的先后顺序，以及相互调用的问题，这导致我的项目动画会结成一个蛇环，天哪，我怎么干了这么蠢的一件事情，为什么我要用组装的思路来实现这个项目？

我抱着侥幸心理，采用`setTimeout`延时定时器来处理这种先后顺序，并且也取得了不错的效果，可是帅不过3秒，当浏览器处于后台执行并再次切换回来的时候，会出现动画的错乱，并且钟盘指针无法正确的着色。

好吧，为了提高页面性能，我们必须尽可能少的使用`setTimeout`，按需使用，不能为了实现一个奇怪的逻辑而写一段奇怪的代码。

## 圆形轨道拖拽

好吧，其实这也是一个坑，我掉进去的坑，两个坑放在一起。

拖拽是很好写的，这种拖拽根本不需要什么技术含量。

可惜啊，这个拖拽是按照圆形轨道运动的。一开始写这个拖拽的效果的时候我是想骂人的，因为我的`余弦定理`已经还给体育老师了，而且这个拖拽还涉及到了`象限`，对对对对对，就是你想起来的那个象限

>奇变偶不变，符号看象限

额好吧，我才认识到数学很重要，可是这个认识未免太晚了些。当我因为数学痛不欲生的时候，我才发现因为自己没文化，做了件很蠢的事情。

	Math.atan2(y,x);
	//返回从 x 轴到点 (x,y) 的角度（介于 -PI/2 与 PI/2 弧度之间）。
	
好吧，再见，体育老师，再见，智商。

智商税务局，你好，我来交税啦！

## 重构javascript和优化

基于`走进误区`描述的路线性的错误，我无法在项目上添加引导动画，而且动画极容易报错，在崩溃的边缘，我一度想通过`try` `catch`来阻止浏览器报错。

	console.dir("节操");

终于做了这个艰难的决定，我花了两天的时间，把整个javascript的代码重写了一下，并且在一些地方使用了模版引擎`Arttemplate.js`，虽然本身也没有多少相同的DOM结构需要引擎生成。

在完成了引导动画的基础上，我再开始堆积木一样做着一些加法，因为有了之前的教训，我在页面中也尽可能少的避免使用`setTimeout`，以避免浏览器后台运行时容易产生的动画错乱。

一回生，二回熟不是。

好吧，我以为我会有很多的牢骚，来写一下这两天的苦大仇深，事情上，这两天蛮开心的，毕竟之前的`version1.0`各种功能的实现大都是能用的，而且看着自己的代码行数越来越少也是一件不错的事情，此外 `transitionDelay` `transitionEnd` 这俩货真的好强啊。

另外我理解的前端都是花20%的时间去实现某个功能，80%的时间是在做优化，数据方面或者动画方面的优化。所以在一些很细节的地方去花费时间，在别人看不见的地方做一些付出，到底值不值得？我认为是值得的。

将页面中仅有的几个小图标剔除，引入了`svg图标字体`，几个细节处的动画效果，页面加载时400ms的3d效果，放弃字符串拼接的方式而采用模版引擎生成DOM，CSS样式的分离，等等。

至少，我认为这80%的付出是值得的。

## 如果还有80%

很多时候，我们都要在时间的面前做出取舍，像这个项目还有很多不完善的地方。这里或许用一个列表来写比较清晰：

* 未加入首页轮播的暂停效果

* 钟盘的指针的动画做的是同步的呼吸动画，并没有做不同步的伸长收缩的跳跃动画

* 未做移动端的适配和低版本的兼容

* 没有通过`localStorage` 或者 `localSession` 做一些跨页面的交互效果

* 没有做资源的预加载，网速不好的话，页面打开较慢

那么这些不完善的地方，大概会在以后的时间里完成，所以这里的说法是：`如果还有80%`

## 后记

对于 `canvas` ＋ `webGL`,我是有很大的兴趣的，这个项目也以为会拿`canvas`来完成，直到写完了，自己的`svg`有了很大的提高，对于`css3`动画也有了进一步的理解，可是对于`webGL`的学习还没有开始。

感觉拿`webGL`渲染3d场景是一件很炫酷的事情，如果有机会的话会考虑做一个这一类型的项目出来，并学习`three.js`

不过接下来的个人修炼会侧重于对于框架的学习，最近好多框架都翻新了不是？ `vue.js2.0` `Angular.js2.0`，至于那个先来，看需求吧。



















	




























