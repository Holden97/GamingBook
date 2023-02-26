# 选择模式的列表的交互

## 选择模式的循环列表

在坦克大战的选择模式的列表中，使用了FGUI的循环列表。

如果要设置类似的循环列表，可以参照如下代码

```
		//set virtual list
		fuiMain.GameModeList_Com.SelectMode_List.SetVirtualAndLoop();
		//set itemRenderer 
		fuiMain.GameModeList_Com.SelectMode_List.itemRenderer = RenderListItem;
		fuiMain.GameModeList_Com.SelectMode_List.numItems = config_GameModes.Count; 
		fuiMain.GameModeList_Com.SelectMode_List.scrollPane.onScroll.Add(DoSpecialEffect);
		fuiMain.GameModeList_Com.SelectMode_List.touchable = true;
		fuiMain.GameModeList_Com.SelectMode_List.scrollPane.onScrollEnd.Add(onScrollEnd);
		fuiMain.GameModeList_Com.SelectMode_List.ScrollToView(config_GameModes.Count*3-1);
```

同时FGUI中的列表设置需要将溢出处理修改为“滚动”，例如：

![](<../../.gitbook/assets/image (4).png>)

而普通的，不需要滚动的列表设置，将溢出处理修改为“可见”即可，例如：

![](<../../.gitbook/assets/image (1).png>)
