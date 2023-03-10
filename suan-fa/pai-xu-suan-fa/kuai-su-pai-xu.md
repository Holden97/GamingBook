# 快速排序

## 过程

1.从所给的要排序的数中先随机抽出一个数(一般选择第一个数作为基数)。

2.遍历输入的数(将每个数和抽出的数比较，比它小的放左边，比大它的放右边)，该过程的时间复杂度为n(第一步)

3.递归的对此时基数两边的部分完成上述操作2，该递归的操作时间复杂度是logn(与遍历二叉树的操作类似)(第二步)

## 分析

第一步和第二步合起来。最终快排的时间复杂度是nlogn。

它是不稳定算法。

首先，排序算法的稳定性大家应该都知道，通俗地讲就是能保证排序前2个相等的数其在序列的前后位置顺序和排序后它们两个的前后位置顺序相同。在简单形式化一下，如果Ai = Aj, Ai原来在位置前，排序后Ai还是要在Aj位置前。 其次，说一下稳定性的好处。排序算法如果是稳定的，那么从一个键上排序，然后再从另一个键上排序，第一个键排序的结果可以为第二个键排序所用。基数排序就 是这样，先按低位排序，逐次按高位排序，低位相同的元素其顺序再高位也相同时是不会改变的。

快速排序有两个方向，左边的i下标一直往右走，当a\[i] <= a\[center\_index]，其中center\_index是中枢元素的数组下标，一般取为数组第0个元素。而右边的j下标一直往左走，当a\[j] > a\[center\_index]。如果i和j都走不动了，i <= j, 交换a\[i]和a\[j],重复上面的过程，直到i>j。 交换a\[j]和a\[center\_index]，完成一趟快速排序。在中枢元素和a\[j]交换的时候，很有可能把前面的元素的稳定性打乱，比如序列为 5 3 3 4 3 8 9 10 11， 现在中枢元素5和3(第5个元素，下标从1开始计)交换就会把元素3的稳定性打乱，所以快速排序是一个不稳定的排序算法，不稳定发生在中枢元素和a\[j] 交换的时刻。