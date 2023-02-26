# 性能分析

Window->Analysis->Profiler 打开分析工具。



U3D的Profiler中的GC ALLOC 项让人很麻烦，一直搞不清楚它是什么，因为 GC 是垃圾回收，而alloc是内存分配，那么 GC ALLOC 是 垃圾回收内存分配？

这个名字起的太TM烂了，其实这是U3D的不知哪个二货程序员起的，除了U3D中，其它任何文献中都没有这个名词。

**GC\_FOR\_MALLOC** 这是安卓中的名词，它表示 means that the GC was triggered because there wasn't enough memory left on the heap to perform an allocation. Might be triggered when new objects are being created.，完全不是一个意思。

其实U3D的GC ALLOC就是 heap alloc，只要new了对象，就会有heap alloc

## 参考资料

1.[https://www.cnblogs.com/timeObjserver/p/9600877.html](https://www.cnblogs.com/timeObjserver/p/9600877.html)
