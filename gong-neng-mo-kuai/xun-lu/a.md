# A\*

## 时间复杂度

考虑到算法性能，外循环中每次从OPEN表取一个元素，共取了n次（共n个结点），每次展开一个结点的后续结点时，需O(n)次，同时再对OPEN表做一次排序，OPEN表大小是O(n)量级的，若用快排就是O(nlogn)，乘以外循环总的复杂度是O(n^2 \* logn)， 　　如果每次不是对OPEN表进行排序，因为总是不断地有新的结点添加进来，所以不用进行排序，而是每次从OPEN表中求一个最小的，那只需要O(n)的复杂度，所以总的复杂度为O(n\*n)，这相当于Dijkstra算法。

## 参考资料

1. [https://gwb.tencent.com/community/detail/106845](https://gwb.tencent.com/community/detail/106845)
