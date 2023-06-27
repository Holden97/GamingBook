# 闭包

闭包的概念：内层函数引用的外层函数的变量的最终值。

```
        for (int i = 0; i < Buttons.value.Count; i++)
        {
            int index = i;
            Buttons.value[i].onClick.AddListener(() =>
            {
                nowButton = Buttons.value[index];
                OutputLogic.Call(new Flow());
            });
        }
```

i的最终值是Buttons.value.Count-1，而index的最终只是循环到当前的i。

参考资料

1.[https://gwb.tencent.com/community/detail/127827](https://gwb.tencent.com/community/detail/127827)
