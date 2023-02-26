---
description: 目前的水平只能做点概览......
---

# 概览

## 浅谈

Editor类是用来编辑继承于Editor类的对象的编辑视图，而PropertyDrawer则是为没有继承此类的Class对象和Struct提供修改接口。

关于PropertyDrawer的论述参见[https://www.cnblogs.com/yangrouchuan/p/6698844.html](https://www.cnblogs.com/yangrouchuan/p/6698844.html) 。

关于编辑器的代码要么放在Editor文件夹下，要么使用预处理命令包裹编辑器代码。

```
#if UNITY_EDITOR #endif
```
