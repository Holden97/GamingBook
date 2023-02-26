# XNode源码阅读

```
private class PortDataCache : Dictionary<System.Type, Dictionary<string, NodePort>> { }
```

这是一种隐藏长类型的好方法。

```
public abstract class NodeEditorBase<T, A, K> 
	where A : Attribute, NodeEditorBase<T, A, K>.INodeEditorAttrib
	where T : NodeEditorBase<T, A, K> 
	where K : ScriptableObject 
```

约束可以约束类，也可以约束接口。多个where中间不需要逗号。
