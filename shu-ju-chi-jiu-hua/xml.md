# XML

文本编辑器

XMLSpear

Sublime Text



XML是一种树形结构

创建XML：将文档后缀改成.xml即可

注释

```xml
<!--注释内容-->
```

```xml
<!--
    hello
    多行注释
-->
```

固定内容

```xml
<?xml version="1.0" encoding="UTF-8"?>
```

编码格式是读取文件时，解析字符串使用的编码，UTF-8是国际通用的一种编码格式。

XML文件必须有一个根节点。

基本语法

```
<PlayerInfo>	
	<name>唐老师</name>
	<atk>10</atk>
	<ItemList>
		<Item>
			<id>1</id>
			<num>10</num>
		</Item>
	</ItemList>
</PlayerInfo>  
```

XML标签对大小敏感

有且只有一个根节点

特殊符号的使用

\&lt -> <

\&gt -> >

\&amp -> &

\&apos -> '

\&quot -> "



属性

```
	<Item id="1" num="1"/>
```

```
	<Item id="1" num="1">
		莓果
	</Item>
```

id和num都是属性，莓果是属性名

属性和元素节点只是写法上的区别而已

```
		<Item>
			<id>1</id>
			<num>10</num>
		</Item>
		
```

```
	<Item id="1" num="1"/>
```

只是写法上的区别

XML文件存放的位置

1.只读不写的XML文件可以放在Resources或者StreamingAssets文件夹下

2.动态存储的XML文件放在Application.persistentDataPath路径下

Resources和StreamingAssets路径下只读不写，可以在项目中直接手动创建对应路径。

Application.persistentDataPath路径下可读可写，不可以在项目中创建对应路径。不同系统具体的路径不一样。



C#读取XML的几种方式

XmlDocument

一般使用

XmlTextReader

以流形式加载，但是是单向只读，一般不使用

Linq



Resources文件夹下加载

TextAsset assset=Resources.Load\<TextAsset>("TextXml");

print(asset.text);

xml.LoadXml(asset.text);

打包后Resources文件夹被压缩，不可用。



streamimgAssetsPath路径下加载

xml.Load(Application._streamimgAssetsPath_+"/TestXml.xml");



XmlDocument.SelectSingleNode()

XmlNode.SelectSingleNode()

XmlNode.SelectNodes()

XmlNode.InnerText



获取属性

XmlNode.Attributes\["属性名"].Value

XmlNode.Attributes.GetNamedItem("属性名").Value



XmlNodeList可以使用循环遍历

## 在Unity中读取Xml文件范例



```
       public static void ReadConfigXml(string path) 
       { 
       XmlDocument xml = new XmlDocument(); 
       xml.Load($"{Application.streamingAssetsPath}/{path}.xml");
         XmlNode root = xml.SelectSingleNode("items");
        XmlNodeList itemsList = root.SelectNodes("item");
        foreach (XmlNode item in itemsList)
        {
            if (item.SelectSingleNode("itemType").InnerText == "Plant")
            {
                var plant = new PlantInfo();
                plant.itemCode = int.Parse(item.SelectSingleNode("itemCode").InnerText);
                plant.itemName = item.SelectSingleNode("itemName").InnerText;
                plant.mass = float.Parse(item.SelectSingleNode("mass").InnerText);
                plant.cutWorkAmount = int.Parse(item.SelectSingleNode("cutWorkAmount").InnerText);
                plant.yieldCount = int.Parse(item.SelectSingleNode("yieldCount").InnerText);
                plant.woodCount = int.Parse(item.SelectSingleNode("woodCount").InnerText);
                plant.fruitItemCode = int.Parse(item.SelectSingleNode("fruitItemCode").InnerText);
                plant.maxHealth = int.Parse(item.SelectSingleNode("maxHealth").InnerText);
                plant.seedItem = int.Parse(item.SelectSingleNode("seedItem").InnerText);
                plant.nutrition = float.Parse(item.SelectSingleNode("nutrition").InnerText);
                plant.growingTime = float.Parse(item.SelectSingleNode("growingTime").InnerText);
                plant.itemSprites = CreateItemSpritesList(item, 6);
                ObjectConfig.plantInfoDic.Add(plant.itemCode, plant);
            }

            if (item.SelectSingleNode("itemType").InnerText == "Seed")
            {
                var seed = new SeedInfo();
                seed.itemCode = int.Parse(item.SelectSingleNode("itemCode").InnerText);
                seed.itemName = item.SelectSingleNode("itemName").InnerText;
                seed.mass = float.Parse(item.SelectSingleNode("mass").InnerText);
                seed.sowWorkAmount = int.Parse(item.SelectSingleNode("sowWorkAmount").InnerText);
                seed.maxHealth = int.Parse(item.SelectSingleNode("maxHealth").InnerText);
                seed.plantItem = int.Parse(item.SelectSingleNode("plantItem").InnerText);
                seed.nutrition = float.Parse(item.SelectSingleNode("nutrition").InnerText);
                seed.itemSprites = CreateItemSpritesList(item, 6);
                ObjectConfig.seedInfo.Add(seed.itemCode, seed);
            }
        }
    }
```
