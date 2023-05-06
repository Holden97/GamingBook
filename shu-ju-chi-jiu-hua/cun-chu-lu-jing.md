# 特殊路径

## Resources 可读 不可写 打包后找不到

## Application.steamingAssetsPath 可读 PC端可写 打包后找得到 但其他平台无法写入

## Application.dataPath 打包后找不到

## Application.persistentDataPath 可读可写 打包后找得到

### 描述

包含持久数据目录的路径（只读）。

该值是目录路径；此目录中可以存储每次运行要保留的数据。在 iOS 和 Android 上发布时，persistentDataPath 指向设备上的公共目录。应用程序更新不会擦除此位置中的文件。用户仍然可以直接擦除这些文件。\
\
构建 Unity 应用程序时，将基于 Bundle ID 生成一个 GUID。此 GUID 是 persistentDataPath 的一部分。如果在将来的版本中保留相同的 Bundle ID，该应用程序将在每次更新时访问相同位置。\
\
**Windows 应用商店应用程序**：[Application.persistentDataPath](https://docs.unity3d.com/cn/2019.4/ScriptReference/Application-persistentDataPath.html) 指向 %userprofile%\AppData\Local\Packages\\\<productname>\LocalState。\
\
**iOS**：[Application.persistentDataPath](https://docs.unity3d.com/cn/2019.4/ScriptReference/Application-persistentDataPath.html) 指向 /var/mobile/Containers/Data/Application/\<guid>/Documents。\
\
**Android**：[Application.persistentDataPath](https://docs.unity3d.com/cn/2019.4/ScriptReference/Application-persistentDataPath.html) 指向大多数设备上的 /storage/emulated/0/Android/data/\<packagename>/files（一些旧款手机可能会指向 SD 卡（如果存在）上的位置），并使用 [android.content.Context.getExternalFilesDir](https://developer.android.com/reference/android/content/Context#getExternalFilesDir\(java.lang.String\).) 来解析该路径。

## 参考资料

1. [https://docs.unity3d.com/cn/2019.4/ScriptReference/Application-persistentDataPath.html](https://docs.unity3d.com/cn/2019.4/ScriptReference/Application-persistentDataPath.html)
