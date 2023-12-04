# 一些注意事项



```
        outHits = new NativeList<DistanceHit>(100, Allocator.Temp);
```

当给NativeList赋初值时，不要忘记赋予其初始长度和Allocator类型。
