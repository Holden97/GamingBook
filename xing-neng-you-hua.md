# 性能优化

Camera.main有CPU性能开销，应将其缓存。

Lerp和+=，谁更好？

Lerp的意义在于时间t可以作为一个参数传入，这也是Dotween和iTween等补间库的基本实现原理。

Enum.ToString()的性能开销较大，应避免在Update方法中使用。

## 参考资料

1. Lerp分析 [https://zhuanlan.zhihu.com/p/577458058](https://zhuanlan.zhihu.com/p/577458058)
2. Unity手册 [https://docs.unity3d.com/ScriptReference/Camera-main.html](https://docs.unity3d.com/ScriptReference/Camera-main.html) 。
