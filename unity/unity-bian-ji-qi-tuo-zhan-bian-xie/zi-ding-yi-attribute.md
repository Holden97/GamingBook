# 自定义Attribute

## 实现自定义编辑器绘制步骤

### 定义自定义Attribute

例如：

```
public class ItemCodeDescriptionAttribute : PropertyAttribute
{

}
```

### 定义自定义绘制类

例如：

```
using UnityEngine;
using UnityEditor;
using System.Collections.Generic;

[CustomPropertyDrawer((typeof(ItemCodeDescriptionAttribute)))]
public class ItemCodeDescriptionDrawer : PropertyDrawer
{
    public override float GetPropertyHeight(SerializedProperty property, GUIContent label)
    {
        //指定属性在编辑器中的高度。
        return EditorGUI.GetPropertyHeight(property) * 2;
    }

    public override void OnGUI(Rect position, SerializedProperty property, GUIContent label)
    {
        //Using BeginProperty/EndProperty on the parent property means that prefab override logic works on the entire property.

        EditorGUI.BeginProperty(position, label, property);

        if (property.propertyType == SerializedPropertyType.Integer)
        {
            EditorGUI.BeginChangeCheck();
            //绘制itemCode
            var newValue = EditorGUI.IntField(new Rect(position.x, position.y, position.width, position.height / 2), label, property.intValue);

            //绘制itemDescription
            EditorGUI.LabelField(new Rect(position.x, position.y + position.height / 2, position.width, position.height / 2), "Item Description", GetItemDescription(property.intValue));

            if (EditorGUI.EndChangeCheck())
            {
                property.intValue = newValue;
            }
        }


        EditorGUI.EndProperty();
    }

    private string GetItemDescription(int itemCode)
    {
        SO_ItemList so_ItemList;
        so_ItemList = AssetDatabase.LoadAssetAtPath("Assets/ScriptableObjectAssets/Item/so_ItemList.asset",typeof(SO_ItemList)) as SO_ItemList;

        List<ItemDetails> itemDetailsList = so_ItemList.itemDetails;

        ItemDetails itemDetail = itemDetailsList.Find(x => x.itemCode == itemCode);

        if (itemDetail != null)
        {
            return itemDetail.itemDescription;
        }
        else
        {
            return "";
        }
    }
}

```

### 应用自定义特性

在对应属性前添加特性，即可应用对应绘制，例如：

```
[SerializeField,ItemCodeDescription]
private int itemCode;
```

