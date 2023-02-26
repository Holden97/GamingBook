# 存档系统

可以使用FileSteam/BinaryFormatter，配合被\[System.Serializable]标签序列化/反序列化数据。但这种方法不够安全。

持久化数据应该被存在Application.persistentDataPath中。

不使用JsonUtility，而是使用Json.net（Newtonsoft.Json）。

使用Path.Combine连接路径

Resources.UnloadUnusedAssets

Rimworld的存档采用XML的方式进行存储。

## 参考资料

1. SAVE & LOAD SYSTEM in Unity [https://www.youtube.com/watch?v=XOjd\_qU2Ido](https://www.youtube.com/watch?v=XOjd\_qU2Ido)
2. Data Persistence - Save & load your game state while avoiding common mistakes | Unity Tutorial [https://www.youtube.com/watch?v=mntS45g8OK4](https://www.youtube.com/watch?v=mntS45g8OK4)
3. Rimworld源码
