# 渲染

Graphics->Scriptable Render Pipeline Settings可以修改渲染管线。

### 一、Camera Depth

相机组件上设置的相机深度，深度越大越靠后渲染。

### 二、透明、不透明物体分隔

RenderQueue 2500是透明与不透明的分水岭。

同一个相机下

Renderqueue小于2500的物体 始终在 Renderqueue大于2500之前绘制。

### 三、Sorting Layer

1. 物体的SortingLayer。 根据 Project Setting - Tags & Layers 中的顺序，越靠上的SortingLayer 越先渲染。
2. 物体的OrderInLayer。 数字越小越先渲染。
3. 物体距离摄像机的距离。 越远的越先渲染。

在Tags & Layers设置中可见

如果Camera相同，那接下来就看Sorting Layers，越低越早绘制。

Order In Layer 越小越早绘制。

Sorting Layers是通过Renderer的sortingLayerName属性设置的。

全局Soring Layers在Edit->ProjectSettings->Tags\&Layers中设置

\


<figure><img src="https://pic2.zhimg.com/80/v2-9b32c026dbd3d92d87881c0a288e80bd_1440w.webp" alt=""><figcaption></figcaption></figure>

\


可以通过代码设置render.sortingLayerName=”Effect”运行时设置物体的sortinglayer，如果在粒子系统中看，可以看到Render项的Sorting Layers下拉表单中会列出所有的Sorting Layers的名字

### 四、Order In Layer

相对于Sorting Layer的子排序，用这个值做比较时只有都在同一层时才有效。

soringOrder也是Render的一个属性，这个order是设置一个数字，数字越大，越在上面显示。

<figure><img src="https://pic2.zhimg.com/80/v2-2729938e3a91d74a551eb6c9ac17809d_1440w.webp" alt=""><figcaption></figcaption></figure>

\


### 五、RenderQueue

Shader中对Tags设置的“Queue”。

RenderQueue 2500是透明与不透明的分水岭。优先绘制不透明物体，再绘&#x5236;_&#x900F;&#x660E;_&#x7269;体。 通过RenderQueue来判断。小于等&#x4E8E;_&#x32;50&#x30;_&#x7684;是不透明物体，大于的&#x662F;_&#x900F;&#x660E;_&#x7269;体。

如果上述几项都相同，那就要看renderQueue了，renderQueure是Material的一个属性，其实就是Shader中的renderQueue，这个也是一个int属性，数值越小，越靠前渲染。

\


<figure><img src="https://pic4.zhimg.com/80/v2-9f4a2677d8d1baed9861d6473cd5816b_1440w.webp" alt=""><figcaption></figcaption></figure>

Unity5以后，每个材质中都可以设置renderq参数。

<figure><img src="https://pic4.zhimg.com/80/v2-b2ea1628f35eeba6cdb7676b038046ab_1440w.webp" alt=""><figcaption></figcaption></figure>

\


### 六、深度排序。按照包围盒的深度进行排序

不透明物体由近到远排序优先

透明物体由远到近排序优先

按照包围盒的中心点的深度进行排序。

<figure><img src="https://pic1.zhimg.com/80/v2-a84d29e55691ea9918dc36d310cf8f2c_1440w.webp" alt=""><figcaption></figcaption></figure>

### 深度补间

ZBias，当两个包围盒中心点深度值相同或者很近的物体在一起时，排序有可能出错（不一定哪个在前）所以对于这个点的z值可以进行微调。（修改物体的轮廓或移动位置。。。）

\


### 一图总结：

<figure><img src="https://pic1.zhimg.com/80/v2-66ae7f08336ff2580e4dcc32b8877f34_1440w.webp" alt=""><figcaption></figcaption></figure>

### 常规方案

### 明确分层：

例如UI上的半透明（特效、面片）始终在3D场景之上，则一般分多个相机来绘制。

大多适用于有明确分层的游戏。例如横版游戏，明确近景远景

大片填充率的物件，例如地形，天空等，一般为提高深度命中，都会选择在延后批次绘制。

Draw character before the terrain.

### UGUI方案

* 同一个canvas，使用Hierarchy的顺序
* 不同的canvas之间使用sort order来排序

\


### NGUI

不同的pannel按照depth数值排序，同一个pannel中的结点按照深度排序。

\


### 其他排序手段

OIT

也没有太适合的移动解决方案。。。

Unity2018后：

可编程渲染管线——LWRP轻量级渲染管线。可以自定义管线控制排序。。。

三角形排序：

主流引擎都不支持，可使用的场景较少，限制较多。曾在Unity引擎中增加了三角形重心快速排序，效率也不高。。。



## 透明物体的渲染

总结一下Shader开启透明渲染后的一些显示问题的解决方案，考虑Zwrite，Ztest，Queue这三个属性的设置问题。

首先需要记住的是：

对于不透明物体，渲染的正确顺序是从前往后；

对于半透明物体。渲染的顺序是从后往前。

这样才能实现正确的渲染输出。

注意：Alpha Test，Alpha to Coverage不在讨论之列。这里涉及到的是使用Alpha Blend的物体。

其次是，Unity Shader中需要注意的三个属性。

Zwirte：是否开启深度缓存，默认为On。

Ztest：深度测试的模式，默认为Lequal（小于等于）。

Queue：渲染队列，默认为Geometry。

常规来说：

对于不透明物体，默认的设置为Zwrite On，Ztest Lequal，Queue=“Geometry”。

对于半透明物体，默认的设置为Zwrite Off，Ztest Lequal，Queue=“Transparent”。

对于半透明物体，这样的设置在一般的情况下是没有问题的，但是在某些情况下会出现问题。

先解释一下相关的概念：

Queue：最好理解，也就是物体的在渲染队列中的顺序。

Zwrite On：会开启深度缓存和深度测试，即在绘制物体表面上的一个像素时，会对像素的深度值和深度缓存中对应点的值进行比较，如果该像素的深度值小于或等于(Ztest Lequal)深度缓存中的值，就用该像素的颜色和深度替换掉原有的像素。

Zwrite开启之后，图形渲染将按照深度测试的结果来进行，不会因为渲染顺序不一致而导致前面的物体被后面的物体覆盖的问题。

半透明物体关闭了Zwrite，这时候渲染队列对于正确的显示就十分重要了。必须严格的按照后面的先渲染，前面的后渲染的顺序，否则会出现不正确的遮挡关系。

那么回到上面的问题，什么情况下半透明物体的默认设置表现不正确？

&#x20;举个简单的例子，十字交叉的面片草渲染，通常情况下会使用Alpha Test的Shader。但是Alpha Test并不支持抗锯齿，所以有时候考虑到锯齿问题OR性能问题使用Alpha Blend。

如果使用默认设置，那么会观察到交叉的重叠效果不正确。这是因为Zwrite Off，两个面片按照先后顺序分别渲染，导致错误的重叠效果。

相反，如果开启了Zwrite，则会发现，由于开启了深度缓存测试，前面的透明像素成功的替换掉后面的不透明像素。导致前面面片的透明像素没有与后面的不透明像素混合，而是直接显示了背景像素。

对于UI之上有模型使用半透明材质，导致与场景进行半透明混合而不是与UI进行半透明混合的解决方案则是：使用脚本调整模型的Sorting Order，使其略大于UI组件Canvas的Sorting Order即可。

&#x20;

除了Shader层面的解决方式，半透明还可以通过修改顶点ID的方式来进行渲染顺序的修改，这里的具体方式不详细说明。



### SoringLayer 和 Layer 的区别

SoringLayer ：排序层级。影响物体渲染的顺序，具体的规则见后文。

Layer：层级。用于物体的逻辑分层。

* 在物理相关的射线检测等时，可以指定忽略/只关注某些层级的物体。
* 相机可以设置只渲染特定层级的物体。

### 相机

Unity 中的相机会影响渲染顺序和射线检测。

默认情况下相机的 rotation 为(0, 0, 0)，朝向 z 轴正方向。 在 2D 游戏中，如果相机的 rotation 设置为（180, 0, 0），则画面上下颠倒； 如果设置为 （0, 180, 0），则画面上下颠倒。

本文以默认情况，即相机 rotation（0, 0, 0）、position（0, 0, -10）、scale（1, 1, 1）为例。

### 渲染顺序

#### 基本规则

按照如下规则**依次**来确定渲染排序 （先渲染的会被后渲染的覆盖。仅当前一个条件相同时，才会比较下一个条件。）

* 物体的 SortingLayer。根据 `Project Setting` - `Tags & Layers` 中的顺序，越靠上的 SortingLayer 越先渲染。
* 物体的 OrderInLayer。数字越小越先渲染。
* 物体距离摄像机的距离。越远的越先渲染。在 2D 游戏中（默认设置下摄像机朝向 z 轴正方向时），这通常等价于 z 坐标，越大的越先渲染。

## 参考资料

1.[https://zhuanlan.zhihu.com/p/55762351](https://zhuanlan.zhihu.com/p/55762351)

2.[https://www.cnblogs.com/jaffhan/p/7381295.html](https://www.cnblogs.com/jaffhan/p/7381295.html)

3.[https://juejin.cn/post/6844904121753927693](https://juejin.cn/post/6844904121753927693)
