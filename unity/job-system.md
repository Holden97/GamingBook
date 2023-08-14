# Job System

Job System是Unity的并行解决方案。

几点限制

1. JobSystem只能够在主线程使用job.Schedule()和job.Complete()。
2. job应该继承IJob接口，实现Execute()方法。
3. 多个job不能够同时使用同一个数据对象，解决办法用两种，一种是等待第一个job完成，第二种是将第一个job作为参数传入第二个job的Schedule方法中。
4. job.Complete()对已经完成的job再次执行该方法是无害的。
5. jobs直接不能循环依赖。
6. 对CPU中数据内存的争夺有时候可能会使得并行job的效率不比单job的效率更高。
