# 物品掉落

物品掉落位置选择思路

1. 确定掉落位置和掉落半径，将MapGridDetail中满足半径要求的点加入数组。
2.  重写List.Sort(Comparison comparison)方法排序，例如：\


    ```
            result.Sort((g1, g2) =>
            {
                return Vector2Int.Distance(g1.pos, pos).CompareTo(Vector2Int.Distance(g2.pos, pos));
            });
    ```
3. 选择下标最小的一个满足要求的MapGridDetail返回，即可得到当前最合适的物品掉落的位置。

参考资料

1. [https://www.cnblogs.com/SouthBegonia/p/12055328.html](https://www.cnblogs.com/SouthBegonia/p/12055328.html)
