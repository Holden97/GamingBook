# Gizmos与Handles

## Handles和Gizmo在渲染顺序上谁在前渲染？

在Unity中，Handles 和 Gizmos 的渲染顺序是不同的：

1. **Handles**：Handles 是编辑器中用于场景视图中的交互和可视化的工具。Handles 总是渲染在 Scene 视图的最前面，并且可以通过用户的交互来选择和操作场景中的对象。Handles 在渲染顺序上位于 Gizmos 前面。
2. **Gizmos**：Gizmos 用于在 Scene 视图中绘制轻量级的图形元素，如线、球体、立方体等。Gizmos 通常用于可视化调试信息，但不支持与用户的交互操作。Gizmos 会在 Handles 之后渲染。

这意味着如果你在编辑器中同时使用 Handles 和 Gizmos，Handles 的可交互性会位于 Gizmos 的可视化之上。这有助于确保你可以选择和操作场景中的对象，同时还能够在场景中绘制有关这些对象的信息。

在Unity中，Handles 和 Gizmos 的渲染顺序实际上是 Gizmos 在前，Handles 在后。这意味着 Gizmos 绘制的内容会覆盖 Handles 绘制的内容。

所以，如果你同时使用 Handles 和 Gizmos，在渲染顺序上，Gizmos 会位于 Handles 之前。这一点需要注意，因为它可能影响到你在场景中进行可视化调试和编辑时的体验。

&#x20;"前后" 是指在场景中的渲染顺序，即渲染的层级关系，而不是时间上的前后。在 Unity 中，渲染的顺序决定了哪个元素会覆盖哪个元素，这是空间上的概念，与时间无关。所以，在这个上下文中，"前" 意味着渲染在更前面，会覆盖渲染在更后面的内容。我希望这能够解释清楚。



例如，希望绘制一个正方体，同时在上方显示文字时，可以这样写

```
    private void OnDrawGizmos()
    {
        Vector3 position = transform.position;
        // 定义正方体的位置和尺寸
        Vector3 size = Vector3.one;

        // 定义正方体的颜色
        Color fillColor = Color.white;
        Color outlineColor = Color.black;

        // 计算正方体的八个顶点
        Vector3[] vertices = new Vector3[8];
        vertices[0] = position + new Vector3(-size.x, -size.y, -size.z) * 0.5f;
        vertices[1] = position + new Vector3(size.x, -size.y, -size.z) * 0.5f;
        vertices[2] = position + new Vector3(size.x, -size.y, size.z) * 0.5f;
        vertices[3] = position + new Vector3(-size.x, -size.y, size.z) * 0.5f;
        vertices[4] = position + new Vector3(-size.x, size.y, -size.z) * 0.5f;
        vertices[5] = position + new Vector3(size.x, size.y, -size.z) * 0.5f;
        vertices[6] = position + new Vector3(size.x, size.y, size.z) * 0.5f;
        vertices[7] = position + new Vector3(-size.x, size.y, size.z) * 0.5f;

        // 绘制有填充颜色的正方体
        Handles.DrawSolidRectangleWithOutline(vertices, fillColor, outlineColor);
        Handles.Label(new Vector3(position.x, 2f, position.y), "Hi!!!!!", style);
    }
```

但如果使用Gizmos的接口绘制正方体，那么文字会被正方体覆盖

```
    private void OnDrawGizmos()
    {
        Vector3 position = transform.position;
        //// 绘制正方体
        Gizmos.color = Color.cyan;
        Gizmos.DrawCube(transform.position, Vector3.one);

        Handles.Label(new Vector3(position.x, 2f, position.y), "Hi!!!!!", style);
    }
```

\
