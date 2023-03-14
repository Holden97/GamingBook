# 元组(Tuple)

Tuple是一种轻型数据结构

使用范例

```
...
using VertexData = System.Tuple<UnityEngine.Vector3, UnityEngine.Vector3, UnityEngine.Vector2>;
namespace ProcedualWorld { 
public static class MeshUtils { 
    public static Mesh MergeMeshes(Mesh[] meshes) { 
        Mesh mesh = new Mesh(); 
        ...
        for (int i = 0; i < meshes.Length; i++)
        {
            ...
            for (int j = 0; j < meshes[i].vertices.Length; j++)
            {
                Vector3 v = meshes[i].vertices[j];
                Vector3 n = meshes[i].normals[j];
                Vector2 u = meshes[i].uv[j];
                
                //元组数据的添加
                
                VertexData p = new VertexData(v, n, u);
                ...
            }
            ...
        }
        ...

    }

    public static void ExtractArrays(Dictionary<VertexData, int> list, Mesh mesh)
    {
        List<Vector3> verts = new List<Vector3>();
        List<Vector3> norms = new List<Vector3>();
        List<Vector2> uvs = new List<Vector2>();
        
        //元组数据的读取
        
        foreach (var v in list.Keys)
        {
            verts.Add(v.Item1);
            norms.Add(v.Item2);
            uvs.Add(v.Item3);
        }
        ...
    }
}
}
```

## ValueTurple

## 参考

1. [https://learn.microsoft.com/en-us/dotnet/api/system.valuetuple?view=net-7.0](https://learn.microsoft.com/en-us/dotnet/api/system.valuetuple?view=net-7.0)
