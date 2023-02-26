# Rimworld反编译源码阅读小记

Rimworld的窗口是如何实现的

所有的窗口继承抽象类Window

关于人物行为的窗口创建集中在ChoicesAtFor方法中。

关于地图的绘制是使用Graphics.DrawMesh接口完成的

游戏中关于用户选择的窗口使用了GUI的内容，从1.11开始学习相关的内容

到目前为止大部分的图形绘制均使用了Graphics.DrawMesh。

路径查找是集中在PathFinder.FindPath中。

队列是缓存信息的关键，有了队列，就能够延迟处理信息，而不必要在一帧中纠结。

public void SetTRS(Vector3 pos, Quaternion q, Vector3 s)如何使用？
