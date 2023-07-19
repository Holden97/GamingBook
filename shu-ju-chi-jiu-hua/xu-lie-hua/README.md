# 序列化

## 简介

Unity只序列化公有字段，以及被标记为\[SerializedField]的私有字段。

Unity序列化所有脚本组件，然后以这些已经序列化后的脚本版本重新载入程序集和脚本组件。序列化的过程采用Unity自己的序列化系统而不是.NET的序列化功能。

不会序列化静态字段和任何属性。

## 可序列化类型

所有继承了UnityEngine.Object的类，例如GameObject,MonoBehaviour,Component,Texture2D,AnimationClip。

所有基础数据类型，例如int，string，bool，float。

所有内置类型，Vector2，Vector3，Vector4, Quaternion, Matrix4x4, Color, Rect, LayerMask.

可序列化类型的数组和List。

枚举。

结构体。

## 注意

你如果在list或array中两次放入同一元素，那么当序列化时，你会获得两份该元素的拷贝，而不是两次同样的一份拷贝。

如果想要序列化自定义结构，那么使用\[System.Serializable]属性，并且该结构非静态。

Unity不会序列化Dictionary，但是你可以将键和值分别存储为List<>，并在Awake()上将它们“组合”在一个非序列化的字典中。这并不能解决您想要修改字典并将其“保存”回来的问题，但在许多其他情况下，这是一个方便的技巧。

## unity没有序列化字典会产生什么后果

在 Unity 中，如果你使用了未序列化的字典数据结构，并尝试将其保存到磁盘或通过网络传输，你可能会遇到以下问题：

1. 无法保存和加载字典数据

Unity 的序列化系统只支持序列化某些特定类型的数据，例如基本数据类型、数组、列表等。如果你使用了未序列化的字典数据类型，你无法将它们保存到磁盘或从磁盘加载。这意味着，当你重新启动应用程序或重新加载场景时，这些未序列化的字典数据将被丢失。

2. 无法在 Inspector 窗口中编辑字典数据

Unity 的 Inspector 窗口提供了一种方便的方式来查看和编辑对象的属性。然而，如果你使用了未序列化的字典数据类型，你将无法在 Inspector 窗口中编辑字典数据，因为 Unity 不知道如何显示和编辑这些数据类型。

3. 可能导致运行时错误

如果你使用了未序列化的字典数据类型，并尝试在代码中对其进行操作，可能会出现运行时错误。这是因为 Unity 的序列化系统无法正确地跟踪和管理未序列化的数据类型。

综上所述，如果你需要在 Unity 中使用字典数据结构，并希望能够将它们保存到磁盘或通过网络传输，你应该使用已序列化的字典数据类型。如果你必须使用未序列化的字典数据类型，则需要手动实现字典的序列化和反序列化逻辑，并确保它们能够正确地工作。

## unity如何序列化一个字典

在 Unity 中，如果你需要序列化一个字典数据结构，可以使用以下方法：

1. 将字典转换为可序列化的数据类型

Unity 的序列化系统只支持序列化某些特定类型的数据，例如基本数据类型、数组、列表等。因此，如果要序列化一个字典，首先需要将它转换为一个可序列化的数据类型。

可以使用自定义类来表示字典，并在该类中添加成员变量来存储字典的键和值。然后，将字典的键和值存储到这些成员变量中，就可以将这个自定义类序列化并保存到磁盘或通过网络传输。

例如，下面是一个示例代码，将字典转换为可序列化的 MyDictionary 类型：

```
using System;
using System.Collections.Generic;

[Serializable]
public class MyDictionary
{
    public List<string> Keys = new List<string>();
    public List<object> Values = new List<object>();

    public void Add(string key, object value)
    {
        this.Keys.Add(key);
        this.Values.Add(value);
    }

    public object GetValue(string key)
    {
        var index = this.Keys.IndexOf(key);
        if (index >= 0 && index < this.Values.Count)
        {
            return this.Values[index];
        }
        return null;
    }
}
```

在上面的示例代码中，我们创建了一个名为 MyDictionary 的类，并使用 Keys 和 Values 成员变量来存储字典的键和值。我们还添加了一个 Add 方法来向 MyDictionary 中添加新的键值对，以及一个 GetValue 方法来获取指定键所对应的值。

2. 序列化和反序列化字典数据

当你需要保存或加载字典数据时，你可以将字典转换为可序列化的 MyDictionary 类型，并将其序列化为二进制数据。在需要时，再将这些二进制数据反序列化为 MyDictionary 类型，并使用它来获取字典数据。

例如，下面是一个示例代码，演示如何将 Dictionary\<string, object> 类型的字典序列化为 MyDictionary 类型：

```
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;

public static class DictionarySerialization
{
    public static byte[] Serialize(Dictionary<string, object> dictionary)
    {
        var myDict = new MyDictionary();
        foreach (var kvp in dictionary)
        {
            myDict.Add(kvp.Key, kvp.Value);
        }

        BinaryFormatter formatter = new BinaryFormatter();
        using (MemoryStream stream = new MemoryStream())
        {
            formatter.Serialize(stream, myDict);
            return stream.ToArray();
        }
    }

    public static Dictionary<string, object> Deserialize(byte[] data)
    {
        BinaryFormatter formatter = new BinaryFormatter();
        using (MemoryStream stream = new MemoryStream(data))
        {
            MyDictionary myDict = (MyDictionary)formatter.Deserialize(stream);
            var dict = new Dictionary<string, object>();
            for (int i = 0; i < myDict.Keys.Count; i++)
            {
                dict[myDict.Keys[i]] = myDict.Values[i];
            }
            return dict;
        }
    }
}
```

在上面的示例代码中，我们创建了一个名为 DictionarySerialization 的静态类，并添加了一个 Serialize 方法和一个 Deserialize 方法。Serialize 方法将 Dictionary\<string, object> 类型的字典转换为 MyDictionary 类型，并序列化为二进制数据。Deserialize 方法则将二进制数据反序列化为 MyDictionary 类型，再将其转换为 Dictionary\<string, object> 类型。

总之，在 Unity 中序列化一个字典需要一些额外的努力，但可以使用自定义类和二进制序列化来实现这个功能。注意，如果你需要序列化大量的字典数据，可能会导致性能问题和高存储成本。

## 在Unity中如何使Dictionary在编辑器窗口中可编辑并能够正确地存储？

在 Unity 中，如果要使 Dictionary 在编辑器窗口中可编辑并正确存储，可以使用以下步骤：

1. 创建一个自定义 Serializable Dictionary 类

首先，需要创建一个自定义的 Serializable Dictionary 类，这个类需要继承 Dictionary，并实现 ISerializationCallbackReceiver 接口。在这个类中，你需要添加两个列表来分别存储字典的键和值，然后通过实现 OnBeforeSerialize 和 OnAfterDeserialize 方法来将键和值转换为列表，以便在序列化和反序列化时使用。

例如，下面是一个示例代码，演示如何创建一个自定义的 Serializable Dictionary 类：

```
csharp复制代码using System;
using System.Collections.Generic;
using UnityEngine;

[Serializable]
public class SerializableDictionary<TKey, TValue> : Dictionary<TKey, TValue>, ISerializationCallbackReceiver
{
    [SerializeField] private List<TKey> keys = new List<TKey>();
    [SerializeField] private List<TValue> values = new List<TValue>();

    public void OnBeforeSerialize()
    {
        this.keys.Clear();
        this.values.Clear();

        foreach (KeyValuePair<TKey, TValue> kvp in this)
        {
            this.keys.Add(kvp.Key);
            this.values.Add(kvp.Value);
        }
    }

    public void OnAfterDeserialize()
    {
        this.Clear();

        if (this.keys.Count != this.values.Count)
        {
            throw new Exception($"There are {this.keys.Count} keys and {this.values.Count} values after deserialization. Make sure that both key and value types are serializable.");
        }

        for (int i = 0; i < this.keys.Count; i++)
        {
            this[this.keys[i]] = this.values[i];
        }
    }
}
```

在上面的代码中，我们创建了一个泛型 SerializableDictionary\<TKey, TValue> 类，并实现了 ISerializationCallbackReceiver 接口。在 OnBeforeSerialize 方法中，我们将字典的键和值存储到列表中，在 OnAfterDeserialize 方法中，我们将这些键和值转换回字典类型。

## 参考资料

1. [https://docs.unity3d.com/ScriptReference/SerializeField.html](https://docs.unity3d.com/ScriptReference/SerializeField.html)
2. ChatGPT
