# 一些注意事项

1. 当给NativeList赋初值时，不要忘记赋予其初始长度和Allocator类型。

```
        outHits = new NativeList<DistanceHit>(100, Allocator.Temp);
```

2. 使用QueryBuilder时，应该尽量缓存Query；并尽量在System中使用Query。
3. 提交之前，删除无用log。

