# I/O

文件

```
    string path = "c:/test/1.txt";
    //判断文件是否存在
    if (!File.Exists(path))
    {
        //创建文件
        try
        {
            File.Create(path);
        }
        catch (Exception e)
        {
        }
    }
```

目录

```
    string path = "c:/test";
    //判断文件夹是否存在
    if (!Directory.Exists(path))
    {
        //创建文件夹
        try
        {
            Directory.CreateDirectory(path);
        }
        catch (Exception e)
        {
        }
    }
```

最后不要忘记刷新项目目录。

```
AssetDatabase.Refresh()
```
