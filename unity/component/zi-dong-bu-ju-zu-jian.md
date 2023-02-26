# 自动布局组件

Content Size Fitter的尺寸计算有延迟。

Content Size Fitter有一个当尺寸变化时的回调，但是类型是protected的，创建一个继承于该类的组件，然后调用此方法并添加额外的回调。



 前言：大家在Unity里做文字的自动换行与图片文字适配是怎么做的？

    今天在游戏项目中有个需求，想让任务的每个子任务框都随着任务描述的文字来动态修改。对于这种需求，我有两个解决方案：\
    1、使用Unity的布局组件 Vertical Layout Group 与 ContentSizeFitter 一起控制，这种方式轻松省力，但对于某些特殊需求不好拓展。\
    2、自己根据内容算大小，根据每一行多少字得到总共多少行。然后设置文本框的 sizeDelta 和对应的背景框的 sizeDelta 。这种可拓展性好很多，但是需要自己解析以及控制长宽，比较麻烦。

    我今天碰到的这个我就是用第一种办法来做的，但是第一种方法不适合在满足适配的情况下做一些其他操作，因为我们通常做操作是在赋值之后立马去做的，而我们给 Text 的 text 赋值时，并不会立马得到真实的 text 大小（猜测 text 的值是这一帧就赋好了，但是 preferredWidth 没有立即计算改变，可能是在下一帧才计算的，而此时布局组件也就没有针对真正的大小来布局，所以当前这一帧的大小有可能是不对的）。而在这里赋值后我还需要得到这个 Element 的大小，所以拿到的也不对。

    我开始冒出的想法就是使用协程来处理这个延迟问题，在下一帧拿到正确的 Element 大小时再做操作，一帧玩家应该看不太出。但协程也有几个问题：\
    1、协程每次运行都开了一个线程，很费。\
    2、你没办法完美控制好在一帧后的执行，时间设置久了太明显，短了受限于机器可能拿到的数据还是错误数据，不是很稳妥。（那个 Text 大小也不一定是一帧后就正确了）。\
    3、代码很难理解日后不好维护。

    在我和同事的思考下，决定从那个控制大小的布局组件入手。我们发现 ContentSizeFitter 当改变大小的时候有一个回调函数，有了这个，我们可以写一个类去继承这个 ContentSizeFitter 类，并重写父类的方法，同时在子类中添加一个委托，当我们在改变 Element 大小的时候还需要 Element 正确的大小去做其他事情时，可以自己给这个委托赋值让其执行。这样这个布局组件的拓展性就好很多了，玩家可以自由订制。实际验证了一下，还是比较好用。

\


**ContentSizeFitter 改变大小的回调函数**

![](https://oss-emcsprod-public.modb.pro/wechatSpider/modb\_20211213\_94db2404-5c1d-11ec-be0f-fa163eb4f6be.png)

\


**复写的子类CustomContentSizeFitter**

![](https://oss-emcsprod-public.modb.pro/wechatSpider/modb\_20211213\_9515891e-5c1d-11ec-be0f-fa163eb4f6be.png)

\


**游戏的实际效果 下面为改变文字后的效果（额外的操作也顺利完成）**

![](https://oss-emcsprod-public.modb.pro/wechatSpider/modb\_20211213\_955b804a-5c1d-11ec-be0f-fa163eb4f6be.png)  ****  ![](https://oss-emcsprod-public.modb.pro/wechatSpider/modb\_20211213\_9597a962-5c1d-11ec-be0f-fa163eb4f6be.png)

\


**Element 的设置界面**

![](https://oss-emcsprod-public.modb.pro/wechatSpider/modb\_20211213\_95c83672-5c1d-11ec-be0f-fa163eb4f6be.png)

\


**Element 子物体文字设置**

![](https://oss-emcsprod-public.modb.pro/wechatSpider/modb\_20211213\_9615de72-5c1d-11ec-be0f-fa163eb4f6be.png)

\


**题外话**

    以前我做聊天的适配时，需要根据每个发言玩家的称号适配称号背景，并根据发的文字适配聊天框带大小。当时我是用的第二种方法，自己去计算大小再手动改变图片和文本框的 sizeDelta ，实时计算的大小同样是有问题的，为了赶进度图方便当时直接用了协程，在0.2s后再重新计算大小并设置大小做其他操作，所以玩家在发言后的短暂空隙内，可以看到自己的称号背景图大小被动态改变，虽然勉强满足策划的需求但用户体验不会很好。



参考资料

1. [https://www.modb.pro/db/198418](https://www.modb.pro/db/198418)
