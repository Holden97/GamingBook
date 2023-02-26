# UI管理中的MVC思想

## 什么是MVC?

MVC是Model（数据模型），View（视图），Controller（控制器）三者的缩写。

### Model

软件数据。

### View

软件视图

### Controller

软件控制器，业务逻辑通常写在这一侧。

## 实际用例

该例出自于维基百科，这是一个通过 [JavaScript](https://zh.m.wikipedia.org/wiki/JavaScript) 所实现的基于 MVC 模型，需要注意的是：MVC 不是一种技术，而是一种理念。

```
/** 模拟 Model, View, Controller */
var M = {}, V = {}, C = {};

/** Model 负责存放资料 */
M.data = "hello world";

/** View 负责将资料输出给用户 */
V.render = (M) => { alert(M.data); }

/** Controller 作为连接 M 和 V 的桥梁 */
C.handleOnload = () => { V.render(M); }

/** 在网页读取的时候呼叫 Controller */
window.onload = C.handleOnload;
```

## 参考资料

1. 维基百科 MVC [https://zh.m.wikipedia.org/zh-hans/MVC](https://zh.m.wikipedia.org/zh-hans/MVC)
