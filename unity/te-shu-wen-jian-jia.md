# 特殊文件夹

## 特殊文件夹名称

通常可为创建的文件夹选择任何名称来组织 Unity 项目。但是，Unity 会将许多文件夹名称解释为应以特殊方式处理文件夹内容的指令。例如，必须将 Editor 脚本放在名为 **Editor** 的文件夹中才能使这些脚本正常工作。

本页面包含 Unity 使用的特殊文件夹名称的完整列表。

### Assets

**Assets** 文件夹是包含 Unity 项目使用的资源的主文件夹。Editor 中的 Project 窗口的内容直接对应于 Assets 文件夹的内容。大多数 API 函数都假定所有内容都位于 Assets 文件夹中，因此不要求显式提及该文件夹。但是，有些函数需要将 Assets 文件夹作为路径名的一部分添加（例如，[AssetDatabase](https://docs.unity3d.com/cn/2018.4/ScriptReference/AssetDatabase.html) 类中的一些函数）。

### Editor

放在名为 **Editor** 的文件夹中的脚本被视为 Editor 脚本而不是运行时脚本。这些脚本在开发期间向 Editor 添加功能，并在运行时在构建中不可用。

可在 Assets 文件夹中的任何位置添加多个 Editor 文件夹。应将 Editor 脚本放在 Editor 文件夹内或其中的子文件夹内。

Editor 文件夹的具体位置会影响其脚本相对于其他脚本的编译时间（有关此方面的完整说明，请参阅[特殊文件夹和脚本编译顺序](https://docs.unity3d.com/cn/2018.4/Manual/ScriptCompileOrderFolders.html)的相关文档）。使用 Editor 脚本中的 [EditorGUIUtility.Load](https://docs.unity3d.com/cn/2018.4/ScriptReference/EditorGUIUtility.Load.html) 函数可从 Editor 文件夹中的 Resources 文件夹加载资源。这些资源只能通过 Editor 脚本加载，并会从构建中剥离。

**注意：**如果脚本位于 Editor 文件夹中，Unity 不允许将派生自 MonoBehaviour 的组件分配给游戏对象。

**注意**：Editor文件夹中的脚本应放于Editor文件夹的根目录下，嵌套文件夹目录下的脚本不会被Unity读取。

### Editor Default Resources

Editor 脚本可以使用通过 [EditorGUIUtility.Load](https://docs.unity3d.com/cn/2018.4/ScriptReference/EditorGUIUtility.Load.html) 函数按需加载的资源文件。此函数在名为 **Editor Default Resources** 的文件夹中查找资源文件。

只能有一个 Editor Default Resources 文件夹，且必须将其放在项目的根目录；直接位于 Assets 文件夹中。将所需的资源文件放在此 Editor Default Resources 文件夹内或其中的子文件夹内。如果资源文件位于子文件夹中，请始终在传递给 [EditorGUIUtility.Load](https://docs.unity3d.com/cn/2018.4/ScriptReference/EditorGUIUtility.Load.html) 函数的路径中包含子文件夹路径。

### Gizmos

[Gizmos](https://docs.unity3d.com/cn/2018.4/ScriptReference/Gizmos.html) 允许将图形添加到 Scene 视图，以帮助可视化不可见的设计细节。[Gizmos.DrawIcon](https://docs.unity3d.com/cn/2018.4/ScriptReference/Gizmos.DrawIcon.html) 函数在场景中放置一个图标，作为特殊对象或位置的标记。必须将用于绘制此图标的图像文件放在名为 **Gizmos** 的文件夹中，这样才能被 DrawIcon 函数找到。

只能有一个 Gizmos 文件夹，且必须将其放在项目的根目录；直接位于 Assets 文件夹中。将所需的资源文件放在此 Gizmos 文件夹内或其中的子文件夹内。如果资源文件位于子文件夹中，请始终在传递给 [Gizmos.DrawIcon](https://docs.unity3d.com/cn/2018.4/ScriptReference/Gizmos.DrawIcon.html) 函数的路径中包含子文件夹路径。

### Plug-ins

可为项目添加[插件](https://docs.unity3d.com/cn/2018.4/Manual/Plugins.html)来扩展 Unity 的功能。插件是通常用 C/C++ 编写而成的本机 DLL。这些插件可以访问第三方代码库、系统调用和其他 Unity 内置功能。请始终将插件放在名为 **Plugins** 的文件夹中，这样才能被 Unity 检测到。

只能有一个 Plugins 文件夹，且必须将其放在项目的根目录；直接位于 Assets 文件夹中。

请参阅[特殊文件夹和脚本编译顺序](https://docs.unity3d.com/cn/2018.4/Manual/ScriptCompileOrderFolders.html)以了解有关此文件夹如何影响脚本编译的更多信息；并参阅 [Plugin Inspector](https://docs.unity3d.com/cn/2018.4/Manual/PluginInspector.html) 以了解有关管理不同目标平台的插件的更多信息。

### Resources

可从脚本中按需加载资源，而不必在场景中创建资源实例以用于游戏。为此，应将资源放在一个名为 **Resources** 的文件夹中。通过使用 [Resources.Load](https://docs.unity3d.com/cn/2018.4/ScriptReference/Resources.Load.html) 函数即可加载这些资源。

可在 Assets 文件夹中的任何位置添加多个 Resources 文件夹。将所需的资源文件放在 Resources 文件夹内或其中的子文件夹内。如果资源文件位于子文件夹中，请始终在传递给 [Resources.Load](https://docs.unity3d.com/cn/2018.4/ScriptReference/Resources.Load.html) 函数的路径中包含子文件夹路径。

请注意，如果 Resources 文件夹是 Editor 的子文件夹，则其中的资源可通过 Editor 脚本加载，但会从构建中剥离。

### Standard Assets

导入标准资源包（菜单：\_\_Assets\_\_ > **Import Package\_\_）时，资源将放在一个名为** Standard Assets\_\_ 的文件夹中。除了包含资源之外，这些文件夹还会对脚本编译顺序产生影响；请参阅关于[特殊文件夹和脚本编译顺序](https://docs.unity3d.com/cn/2018.4/Manual/ScriptCompileOrderFolders.html)的页面以了解更多详细信息。

只能有一个 Standard Assets 文件夹，且必须将其放在项目的根目录；直接位于 Assets 文件夹中。将所需的资源文件放在此 Standard Assets 文件夹内或其中的子文件夹内。

### StreamingAssets

尽管将资源直接合并到构建中更为常见，但有时可能希望资源以其原始格式作为单独的文件提供。例如，需要从文件系统访问视频文件，而不是用作 MovieTexture 在 iOS 上播放该视频。将一个文件放在名为 **StreamingAssets** 的文件夹中，这样就会将其按原样复制到目标计算机，然后就能从特定文件夹中访问该文件。请参阅关于[流媒体资源 (Streaming Assets)](https://docs.unity3d.com/cn/2018.4/Manual/StreamingAssets.html) 的页面以了解更多详细信息。

只能有一个 StreamingAssets 文件夹，且必须将其放在项目的根目录；直接位于 Assets 文件夹中。将所需的资源文件放在此 StreamingAssets 文件夹内或其中的子文件夹内。如果资源文件位于子文件夹中，请始终在用于引用流媒体资源的路径中包含子文件夹路径。

### 隐藏的资源

在导入过程中，Unity 完全忽略 **Assets** 文件夹（或其子文件夹）中的以下文件和文件夹：

* 隐藏的文件夹。
* 以“**.**”开头的文件和文件夹。
* 以“**\~**”结尾的文件和文件夹。
* 名为 **cvs** 的文件和文件夹。
* 扩展名为 **.tmp** 的文件。

这是为了防止导入由操作系统或其他应用程序创建的特殊文件和临时文件。

参考资料

1. [https://docs.unity3d.com/cn/2018.4/Manual/SpecialFolders.html](https://docs.unity3d.com/cn/2018.4/Manual/SpecialFolders.html)
