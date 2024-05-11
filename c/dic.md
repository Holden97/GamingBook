# Dic

```
        //if (GuidePointerManager.Instance.checkAreaGameObjPossInfo.ContainsKey(pointName))
        //{
        //    GuidePointerManager.Instance.checkAreaGameObjPossInfo[pointName] = true;
        //}
        //else
        //{
        //    GuidePointerManager.Instance.checkAreaGameObjPossInfo.Add(pointName, true);
        //}
        上面的写法重复了
        应该采用下面的
        GuidePointerManager.Instance.checkAreaGameObjPossInfo[pointName] = true;
```
