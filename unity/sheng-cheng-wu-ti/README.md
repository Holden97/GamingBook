# 生成物体

## Instantiate和Resources.Load生成的区别

后者生成在内存中，而前者将内存中的物体生成到场景中.

## 物体查找

查找场景中所有物体

```
		var allObjects = GameObject.FindObjectsOfType<GameObject>();
```
