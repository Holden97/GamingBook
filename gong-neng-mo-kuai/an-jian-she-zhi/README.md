# 按键设置

在OnGUI中监听当前的Event，从而使用当前的Event。

## 如何修改Unity中启用的按键系统

修改位置：Project Setting->Player->Other Settings->Configuration->Active Input Handling。

## Unity的事件系统

在场景中添加EventSystem后，可以在UI中添加EventTrigger组件，来定制对事件的响应。

## 按键重新绑定

使用PerformInteractiveRebinding方法，示例代码

```
playerInputActions.Player.Disable();
playerInputActions.Player.Jump.PerformInteractiveRebinding()
    .OnComplete(callback=>{
        Debug.Log(callback);
        callback.Dispose();
        playerInputActions.Player.Enable();
    })
    .Start();
```

产生的callback需要手动释放以避免内存泄漏。

WithControlsExcluding()可以指定无法被重新绑定的按键。例如：

```
playerInputActions.Player.Disable();
playerInputActions.Player.Jump.PerformInteractiveRebinding()
      .WithControlsExcluding("Mouse")
      .OnComplete(callback=>{
        Debug.Log(callback);
        callback.Dispose();
        playerInputActions.Player.Enable();
    })
    .Start();
```

此时，鼠标无法被指定为重新绑定的按键。

## 持久化保存按键修改

在Input System1.1之后，可以通过

```
void SaveUserRebinds(PlayerInput player)
{
    var rebinds = player.actions.SaveBindingOverridesAsJson();
    PlayerPrefs.SetString("rebinds", rebinds);
}
 
void LoadUserRebinds(PlayerInput player)
{
    var rebinds = PlayerPrefs.GetString("rebinds");
    player.actions.LoadBindingOverridesFromJson(rebinds);
}
```

RebindSaveLoad组件已经应用此代码持久化保存按键修改。

需要注意在删除改键时删除对应PlayerPref。

## 改键功能

改键功能使用了官方示例中的RebindActionUI脚本。改键中需要注意去除重复按键。

InputBinding newBinding = action.bindings\[bindingIndex];

```
    private bool CheckDuplicateBindings(InputAction action, int bindingIndex, bool allCompositeParts = false)
    {
        //单个按键判断是否与同一ActionMap中的其他按键重复。
        foreach (InputBinding binding in action.actionMap.bindings)
        {
            if (binding.action == newBinding.action)
            {
                continue;
            }
            if (binding.effectivePath == newBinding.effectivePath)
            {
                Debug.LogWarning($"Duplicate binding found:{newBinding.effectivePath}");
                return true;
            }
        }
        //复合按键额外判断复合按键中是否与其他在此之前设置的按键重复
        if (allCompositeParts)
        {
            for (int i = 1; i < bindingIndex; i++)
            {
                if (action.bindings[i].effectivePath == newBinding.effectivePath)
                {
                    Debug.Log($"Duplicate binding found:{newBinding.effectivePath}");
                    return true;
                }
            }
        }
        return false;
    }
```

## 参考资料

1. Unity输入系统 [https://www.youtube.com/watch?v=3NBYqPAA5Es](https://www.youtube.com/watch?v=3NBYqPAA5Es)
2. How to use NEW Input System Package! (Unity Tutorial - Keyboard, Mouse, Touch, Gamepad) [https://www.youtube.com/watch?v=Yjee\_e4fICc\&t=1432s](https://www.youtube.com/watch?v=Yjee\_e4fICc\&t=1432s)
