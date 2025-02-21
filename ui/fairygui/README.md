# FairyGUI

## 动态创建UI资源

### 设置适配相关

```
GRoot.inst.SetContentScaleFactor(1365, 768, UIContentScaler.ScreenMatchMode.MatchHeight);
```

### 添加包

```
        UIPackage package = UIPackage.AddPackage("UI/Teach");
```

### 添加依赖包

```
        foreach (var item in package.dependencies)
        {
            UIPackage.AddPackage("UI/" + item["name"]);
        }
```

### 同步加载元件

```
    GComponent view = UIPackage.CreateObject("Teach", "TeachPanel").asCom;
    GRoot.inst.AddChild(view);
```

### 动态同步加载图片

```
GImage img = UIPackage.CreateObject("Teach", "TeachPanel").asImage;
view.AddChild(img);
```

### 获取图片

```
GImage img = view.GetChild("imgTest").asImage//参数为图片名
```

### 获取图形元件

```
GGraph graph = view.GetChild("graphTest").asGraph//参数为图形名，获取图形元件
graph.DrawRect(100,100,4,Color.red,Color.white);//将图形绘制为矩形
```

### 动态创建图形

```
GGraph graph2 = new GGraph();
graph.SetPosition(50,50,0);
graph2.DrawRect(200,200,5,Color.white,Color.red);//创建一个矩形
view.AddChild(graph2);
```

### 异步加载

```
    UIPackage.CreateObjectAsync("Teach", "TeachPanel", (obj) =>
    {
        GComponent v = obj.asCom;
        GRoot.inst.AddChild(v);
    });
```

### 销毁

```
view.Dispose();
```

## GComponent/GObject常用API

### 位置

x,y,SetPosition

### 宽高

width,height,SetSize

### 轴心点

轴心点是旋转中心点。锚点是UI位置点。

pivotX,pivotY,pivotAsAnchor(设置轴心点是否同时为锚点),SetPivot

### 角度

rotation

### 可见性

visible

### 置灰

grayed

### 缩放

scaleX,scaleY

### 排序层(子对象层级)

sortingOrder

### 点击不穿透

opaque

### 子对象数量

numChildren

### 根据名称获取子对象

GetChild()

### 根据索引获取子对象

SetChildIndex

### 子对象排序规则

childrenRenderOrder，默认升序排列，从小到大依次渲染，序号大的显示在前面。

### 原生对象

displayObject.gameObject

displayObject就类似于挂在在场景对象上的一个组件，通过其gameObject属性可以获取到对应挂载的gameObject。

### 自定义数据

data

## 元件

元件是GObject,组件是GComponent，组件是元件的一种。

元件是舞台上的最小组成单位。



FGUI图集模糊的处理

选中模糊的图集文件

1. 不勾选图集的Generate Mipmaps
2. 勾选Override For Windows,Mac,Linux；将Max Size改成FGUI中的纹理集的最大尺寸。
