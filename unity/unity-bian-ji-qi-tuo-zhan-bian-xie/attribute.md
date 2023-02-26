---
description: 朝闻道，夕死可矣。
---

# Attribute

## Unity中的Attribute是如何实现的？

## Mirror中的SceneAttribute是如何将string类型在编辑器试图中转换成了可编辑的场景类型？

## 常用Attribute

### \[MenuItem]

#### 在主菜单中添加菜单

例：

<pre><code><strong>	[MenuItem("QTool/打包/自动打包")]
</strong><strong>	public static async Task Test()
</strong>	{
		while (switchValu)
		{
			Pack();
			await Task.Delay(1000 * 600);
			Debug.Log($"AutoPack");
		}
	}
</code></pre>

#### 在Project视图右键/主菜单Assets中添加菜单

```
	[MenuItem("Assets/测试MENU")]

```

#### 在Hierarchy视图右键/主菜单GameObject中添加菜单

```
        [MenuItem("GameObject/测试MENU")]
```

#### 在对应组件\[更多]菜单中添加菜单

```
        [MenuItem("CONTEXT/Transform/测试MENU")]
```

第一个参数在名称相同的情况下，只按函数编写顺序显示第一个。

第三个参数控制优先级顺序，int值越小，优先级越高；默认1000；优先级相差11以上，会创建一条分割线。

### \[Tooltip]

字段提示。

## 如何在菜单中创建可以修改对应bool值的菜单？

## 参考资料

1. C#必备核心语法 Attribute教程｜从基础到场景实操合集2022全新录制 [https://www.bilibili.com/video/BV1xS4y1m7LP?spm\_id\_from=333.337.search-card.all.click\&vd\_source=871f728d40a60dff8670a9af8d3e0076](https://www.bilibili.com/video/BV1xS4y1m7LP?spm\_id\_from=333.337.search-card.all.click\&vd\_source=871f728d40a60dff8670a9af8d3e0076)
