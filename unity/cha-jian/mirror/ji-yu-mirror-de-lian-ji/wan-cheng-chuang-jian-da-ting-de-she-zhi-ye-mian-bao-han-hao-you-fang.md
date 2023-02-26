# 完成创建大厅的设置页面\[包含好友房]

UI部分使用TextMeshPro，需要通过导入额外字体的方式添加对中文显示的支持，具体参见[https://blog.csdn.net/zhunju0089/article/details/103125168](https://blog.csdn.net/zhunju0089/article/details/103125168)

Unity的Silder是支持只使用整数的，只要勾选**Whole Numbers**即可。具体参见[https://docs.unity3d.com/cn/2020.3/Manual/script-Slider.html](https://docs.unity3d.com/cn/2020.3/Manual/script-Slider.html)

之后应该实现一个类似于\[SyncVar(hook="")]一样的属性，来实现只有属性值变化时才会调取方法的效果。这一点其实也可以通过设置属性时向其中添加方法来实现。现在这件事单纯变成了一种技术学习而不是必需。

目前UGUI已经替我考虑到了这一点，在Slider，Toggle这种涉及到数值变化的UI中都会提供一个添加数值变化的回调入口。
