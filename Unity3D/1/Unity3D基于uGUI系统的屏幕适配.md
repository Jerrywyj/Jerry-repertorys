# Unity3D基于uGUI系统的屏幕适配

## 1 屏幕适配问题简述
现在的GUI系统基本已经做好缩放算法，因此长宽比相同但大小不同的屏幕适配问题很容易解决，GUI系统的默认配置往往都可以满足要求。因此这里主要说的是由屏幕长宽比的不同而带来的适配问题。
当长宽比不同时，UI元素往往要以屏幕的长或者宽为基准进行缩放，尔后再根据开发者的配置决定是否裁剪，我们不妨把这时有可能产生的问题归为两类：
#### 1.1 位置问题
当屏幕的长宽比改变后，UI元素有可能会显示到不恰当的位置上，如跑出屏幕以外；重叠；布局比例不协调等。
#### 1.2 缩放比例问题
当屏幕的长宽比改变后，当前的UI元素就可能存在过大或者过小的情况。

## 2 解决思路
这里我们只讨论同样是竖屏或者横屏情况下的解决办法。
试想一下，假如某一页UI画面的背景及其各种可交互UI元素是一整张图片，这张图片直接根据不同的屏幕以长或者宽进行缩放就好了，这样就不会出现UI元素重叠或者跑出屏幕以外的情况了。顺着这个思路，我们可以想到若要实现以上目的，GUI系统就必须做到以下两点：
#### 2.1必须保证UI元素在屏幕中的相对位置不变。
#### 2.2必须保证UI元素的缩放比例和UI画面的背景相同。
以下就以uGUI系统为例，实现以上两个目的。

## 3 uGUI简介
uGUI是Unity3D推出的官方GUI系统，借鉴了NGUI的很多设计，而且制作更加简单，拥有更好更直观的组件深度值解决方案。

## 4 uGUI配置说明
uGUI中的每个UI元素都必须放置于某个Canvas之下，也就是说，我们可以创建多个Canvas。下面说说Canvas的Render Mode一共有三种：
#### 4.1 Screen Space-Overlay
该模式下Canvas会填满整个屏幕空间，并将画布下的所有UI元素置于屏幕的最上层，如果屏幕尺寸被改变，Canvas将自动改变尺寸来匹配屏幕。这种模式主要适用于单纯的UI界面制作，如登陆、创建游戏等。

![CanvasOverlay](https://github.com/Jerrywyj/Learn-way/blob/master/Unity3D/1/CanvasOverlay.png)

#### 4.2 Screen Space-Camera
该模式下Canvas会被放置在指定Camera前方的某个距离内（该距离可以配置），当指定的Camera是主Camera时，如果屏幕尺寸被改变，画布将自动改变尺寸来匹配屏幕。当Camera和Canvas存在其它GameObject时，Canvas会被遮盖。这种模式适用的开发情景广，既可以用于单纯的UI界面制作，也可以用做头瞄等。

![CanvasCamera](https://github.com/Jerrywyj/Learn-way/blob/master/Unity3D/1/CanvasCamera.png)

#### 4.3 World Space
该模式下UI元素将被当作场景中的plane object，Canvas不会一直呈现在Camera前面。这种模式适用于UI本身就是3D场景的开发情景。并且不存在适配问题。

![CanvasWorldSpace](https://github.com/Jerrywyj/Learn-way/blob/master/Unity3D/1/CanvasWorldSpace.png)

## 5 例子
以Screen Space-Overlay模式为例，首先应当将UI Scale Mode设为Scale With Screen Size即根据屏幕尺寸进行缩放，然后设定参照分辨率，如1920X1080，这得根据游戏策划或者美工师的设计来确定。Screen Match Mode使用默认即可，这一属性决定Canvas的缩放基准以及是否进行裁剪，具体可看官方文档。以上设定可解决UI元素的缩放比例问题，设定完成后，该Canvas下的UI元素将根据屏幕尺寸进行缩放。

接着我们来解决另一个问题——保证UI元素的锚点在屏幕中的相对位置不变。首先将UI背景画面的Rect Transform配置如下：

![stretch](https://github.com/Jerrywyj/Learn-way/blob/master/Unity3D/1/stretch.png)

这里可以把Anchors理解为容纳UI背景画面的“盒子”，Min X ＝ 0表示“盒子”的左边离屏幕左边框的距离为屏幕总长的0%，Max X ＝ 0表示“盒子”的右边离屏幕左边框的距离为屏幕总长的100%，垂直方向的设定依此类推。既然Anchors是容纳UI背景画面的"盒子"，那么我们就可以设定UI背景图片与"盒子"边界的距离——内边距（确切的说应该是pivot点离“盒子”边界的距离），这和html dom模型中的padding的概念是一样的，图中上下左右内边距都设置为0，也就是说UI背景画面将铺满整个Canvas。

写到这里其实已经实现了保证UI元素在屏幕中的相对位置不变的目的，但UI背景图片的设定算是特殊的例子，下面再以一个Button为例。假设有一个Button位于450X500的Canvas中，设定如下图所示：

![Anchors](https://github.com/Jerrywyj/Learn-way/blob/master/Unity3D/1/Anchors.png)

"盒子"的左边离屏幕左边框的距离为屏幕总长的30%，"盒子"的右边离屏幕左边框的距离为屏幕总长的70%，上下边以此类推，按钮铺满“盒子”。

## 6 总结
以上办法可以解决大部分的屏幕适配问题，当然具体方案还需要结合项目的需求进行细微的修改，例如在某些大屏幕情景下按钮不再继续放大等等。值得注意的是，通过以上办法解决屏幕适配的前提条件是需要有一个详细的UI系统设计方案，特别是UI元素距离画面边缘的像素在设计方案中必须给出，这样开发人员才好在开发时将其换算为百分比的相对距离并进行设定。


