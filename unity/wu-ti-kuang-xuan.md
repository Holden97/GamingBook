# 物体框选

## 几个问题

### 在新的InputSystem中，用什么方法代替GetMouseDown？

目前无论是否框选，都会update选择框信息。在按键的InputAction下，有isPressed()判断是否按下。可以将二者结合起来判断，只在按下时update。

### 如何将现在在世界坐标中的UI正确的移植到UI层？

将选择框放在UI层更新，然后将2D物体的Collider的Bounds转换成矩形，与选择框的矩形比较是否有重叠部分，如果有就加入选择列表。

## 用到的一些API

Camera.main.ScreenToViewportPoint 返回0-1的二维屏幕坐标。

Camera.main.ScreenToWorldPoint(mousePosition)返回鼠标在屏幕上的坐标对应的世界坐标。

## 一些细节

框选区域要避免和2D物体z轴坐标相同，因为相同的坐标渲染后的前后关系具有一定随机性。

可以通过改变渲染顺序和物体z轴坐标规避。

RectTransform的Rect起始位置始终是(0,0)，这意味着需要另一个矩形区域表示框选范围。

目前的框选有几个问题

### 单击

单击的方法可以和框选方法合并，不需要单独写处理。

### 范围

目前使用的是人物Collider2D.Bounds来表示人物范围，可以改成Sprite的重叠范围么？

### 更多操作

应该支持Shift添加/删除选择，双击快速选择屏幕中的同类型物品，选择时按照选择范围中的等级优先选择选择等级高的一类物品。
