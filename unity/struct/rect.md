# Rect

在RectTransform中，如果想获得其在屏幕空间下的矩形表示。操作如下：

1. 将锚点和位置放置于左下角
2.

````
```
````

````
        var rect = item.GetComponent<RectTransform>();
        new Rect(rect.position.x, rect.position.y, rect.rect.width, rect.rect.height);
```
````

官方文档中说GUI的Y轴坐标向下增长，但从上述表示来看，RectTransform的Y轴坐标似乎仍是向上增长。

GUI和相机下的屏幕空间原点位置不一样，GUI原点在左上角，而屏幕空间原点在左下角

鼠标的空间原点在屏幕的左下角。

## 参考资料

1. Rect [https://docs.unity3d.com/ScriptReference/Rect.html](https://docs.unity3d.com/ScriptReference/Rect.html)
