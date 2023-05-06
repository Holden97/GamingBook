# UGUI

UI元素可响应的充要条件

1. 使用旧InputSystem时，场景中存在一个EventSystem,以及Input Module。
2. 在层级中，没有更上层的，例如Image一样可以勾选raycast target选项的组件遮挡点击事件。
3. 添加Image，并勾选raycast target。
