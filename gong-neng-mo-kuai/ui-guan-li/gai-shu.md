---
description: 博采众长
---

# 概述

参考1中的视频中问题过多，看了一般写了个开头决定不再照着它的内容写下去。先记下需要后续关注的点。

参考2在设计到具体代码时思想不够清晰，View层的代码目前不需要用到，它的设计让View层不继承MonoBehaviour，然而却完全仿照MonoBehaviour进行接口设计。

1. UIManager应该采用单例。
2. 使用CanvasGroup设置面板能否被点击。
3. 是否要关联场景管理？
4. 切场景时要注意UI设置。
5. 能否有一个通用的关闭回调作为BasePanel的虚方法被子类继承？
6. 要注意到窗口关闭后再打开时不是创建新窗口，而是从内存中设置已有窗口的可见性。
7. 遮挡点击。

UI的数据在内存中采用由List构成的Dictionary存储。

{% hint style="info" %}
为什么采用List而不是Stack？

因为关闭界面时，要确切的知道到底关闭哪一个界面，而Stack只提供Pop和Push方法，并不能精确控制，会增加程序出问题的风险。
{% endhint %}

## 像素计算

图像的实际大小 =图像Transform中的 Scale x 图像原始大小 / 图像的Pixel Per Unit

## 使用Full Rect还是Tight？

一般情况下使用Tight，UI在Image是Simple模式下可以通过勾选Use Sprite Mesh来使用Tight。

如果Image采用的不是Simple模式，那么采用了Full Rect。

## 动态修改UI到屏幕正上方/正下方

### 正下方

```
rectTransform.pivot=new Vector2(0.5f,0f);
rectTransform.anchorMin=new Vector2(0.5f,0f);
rectTransform.anchorMax=new Vector2(0.5f,0f);
rectTransform.anchoredPosition=new Vector2(0f,2.5f);
```

### 正上方

```
rectTransform.pivot=new Vector2(0.5f,1f);
rectTransform.anchorMin=new Vector2(0.5f,1f);
rectTransform.anchorMax=new Vector2(0.5f,1f);
rectTransform.anchoredPosition=new Vector2(0f,-2.5f);
```

## AnchoredPosition

## 参考资料

1. \[Unity编程]这大概是最好理解的UI框架了吧 [https://www.bilibili.com/video/BV1Bz4y1D7rL?p=10\&spm\_id\_from=pageDriver\&vd\_source=871f728d40a60dff8670a9af8d3e0076](https://www.bilibili.com/video/BV1Bz4y1D7rL?p=10\&spm\_id\_from=pageDriver\&vd\_source=871f728d40a60dff8670a9af8d3e0076)
2. unity基于MVC的UI框架 [https://www.bilibili.com/video/BV1KK41157By?p=7\&spm\_id\_from=pageDriver\&vd\_source=871f728d40a60dff8670a9af8d3e0076](https://www.bilibili.com/video/BV1KK41157By?p=7\&spm\_id\_from=pageDriver\&vd\_source=871f728d40a60dff8670a9af8d3e0076)&#x20;
3. Use Full-Rect or Tight? [https://thegamedev.guru/unity-gpu-performance/sprites-full-rect-or-tight/](https://thegamedev.guru/unity-gpu-performance/sprites-full-rect-or-tight/)
