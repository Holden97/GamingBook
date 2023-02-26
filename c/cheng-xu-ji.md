---
description: 理解TA的动作，让大家更高效的协作
---

# 程序集

## 什么是程序集？

**程序集是代码进行编译是的一个逻辑单元**，把相关的代码和类型进行组合，然后生成PE文件（例如可执行文件\*\*.exe**和类库文件**.dll\*\*）。由于程序集在编译后并不一定会生成单个文件，而可能会生成多个物理文件，甚至可能会生成分布在不同位置的多个物理文件，所以**程序集是一个逻辑单元，而不是一个物理单元**。即程序集在逻辑上是一个编译单元，但在物理储存上可以有多种存在形式。对于静态程序集可以生成单个或多个文件，而动态程序集是存在于内存中的。在C#中程序集处处可见，因为**任何基于.NET的代码在编译时都至少存在一个程序集**。\
基于.NET框架的.dll库是一个完整的程序集，需要事先引用对应的类库。从代码的结构上看，**一个程序集可以包含一个或多个命名空间**，而**每个命名空间中又可以包含子命名空间或类型列表**。由于程序集在编译后可以生成多个模块文件，因此一个物理文件并不代表它就是一个程序集，一个程序集并不一定只有一个文件。在VS开发环境中，一个解决方案可以包含多个项目，而每个项目就是一个程序集。

应用程序结构：包含 应用程序域（AppDomain），程序集（Assembly），模块（Module），类型（Type），成员（EventInfo、FieldInfo、MethodInfo、PropertyInfo） 几个层次。

DLL文件的生成路径：**项目路径**\Library\ScriptAssemblies\xxxx.dll。\\

![](https://img-blog.csdnimg.cn/20200329102627414.png?x-oss-process=image/watermark,type\_ZmFuZ3poZW5naGVpdGk,shadow\_10,text\_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzQwNTg0NQ==,size\_16,color\_FFFFFF,t\_70)

## 为什么使用程序集？

1. 开发者可以自定义程序集，定义清晰的依赖关系，可以确保脚本更改后，只会重新生成必需的程序集，减少编译时间。
2. 可以跨项目进行程序的复用，加快开发效率。
3. 支持跨语言的编程，例如可以在unity中使用C++语言编辑的DLL文件。

## 预定义程序集

1.在Assets文件夹中编写脚本，如果没有进行自定义操作，会默认编译到 Assembly-CSharp.dll 中。

![](../.gitbook/assets/20200329103901836.png)

2.开发过程中如果进行了编辑器扩展，创建了**Editor**文件夹，并在该目录下编写了脚本，则默认编译到Assembly-CSharp-Editor.dll文件中。

![](<../.gitbook/assets/20200329104201481 (1).png>)

## 自定义程序集

创建命令路径：右键->Create->Assembly Definition

与新建的程序集处于同一层级或处于子层级的所有脚本都会被编译到该程序集中。

如果子文件夹又自己的程序集定义，那么子文件夹的所有脚本以及子文件夹的子文件夹会被单独编译到它自己的程序集中。

## 两种程序集资源

### 程序集定义资源

在文件夹中创建一个程序集定义资源后，Unity 会从该文件夹中的所有脚本编译一个单独的托管程序集。除非子文件夹有自己的程序集定义，否则将包含子文件夹中的脚本。这些托管程序集充当 Unity 项目中的单个库。

### 程序集定义引用资源

为了将非子文件夹下的脚本添加到一个已存在的程序集中，可在此文件夹下创建程序集引用资源。

每个文件夹中，只能创建一个程序集定义或程序集定义引用。

如果在已经具有程序集定义或程序集定义引用的文件夹的子文件夹中创建程序集定义或程序集定义引用，则 Unity 会将子文件夹中的所有脚本及其子项编译到子文件夹中定义的程序集内，而不是父文件夹中定义的程序集内。

## 参考资料

1. Unity关于程序集（Assembly ）的那些事\_DwarfTitan的博客-程序员宅基地[https://www.cxyzjd.com/article/weixin\_43405845/105174096](https://www.cxyzjd.com/article/weixin\_43405845/105174096)。
2. 官方关于程序集的论述[https://docs.unity3d.com/cn/2020.3/Manual/ScriptCompilationAssemblyDefinitionFiles.html](https://docs.unity3d.com/cn/2020.3/Manual/ScriptCompilationAssemblyDefinitionFiles.html)
3. 泽梅尔的驻留地[https://www.zling.site/post/Unity%E4%B8%AD%E7%9A%84%E7%A8%8B%E5%BA%8F%E9%9B%86%E5%AE%9A%E4%B9%89/](https://www.zling.site/post/Unity%E4%B8%AD%E7%9A%84%E7%A8%8B%E5%BA%8F%E9%9B%86%E5%AE%9A%E4%B9%89/)
