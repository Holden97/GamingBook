# Job System

Job System是Unity的并行解决方案。

几点限制

1. JobSystem只能够在主线程使用job.Schedule()和job.Complete()。
2. job应该继承IJob接口，实现Execute()方法。
3. 多个job不能够同时使用同一个数据对象，解决办法用两种，一种是等待第一个job完成，第二种是将第一个job作为参数传入第二个job的Schedule方法中。
4. job.Complete()对已经完成的job再次执行该方法是无害的。
5. jobs直接不能循环依赖。
6. 对CPU中数据内存的争夺有时候可能会使得并行job的效率不比单job的效率更高。

### 不要从一个job中访问静态的数据

在所有的安全性系统中你都应当避免从一个job中访问静态数据。如果你访问了错误的数据，你可能会使Unity崩溃，通常是以意想不到的方式。举例来说，访问一个MonoBehaviour可以导致域重新加载时崩溃。

注意：因为这个风险，未来版本的Unity会通过静态分析来阻止全局变量在job中的访问。如果你确实在job中访问了静态数据，你应当预见到你的代码会在Unity未来的版本中报错。

### 刷新已调度的批次

当你希望你的job开始执行时，你可以通过JobHandle.ScheduleBatchedJobs来刷新已调度的批次。注意调用这个接口时会对性能产生负面的影响。不刷新批次将会延迟调度job，直到主线程开始等待job的结果。在任何其他情况中，你应当调用JobHandle.Complete来开始执行过程。

注意：在[Entity Component System](https://link.zhihu.com/?target=https%3A//github.com/Unity-Technologies/EntityComponentSystemSamples)(ECS)中批次会暗中为你进行刷新，所以调用JobHandle.ScheduleBatchedJobs是不必要的。

### 不要试图去更新NativeContainer的内容

由于缺乏引用返回值，不可能去直接修改一个NativeContainer的内容。例如，nativeArray\[0]++ ;和 var temp = nativeArray\[0]; temp++;一样，都没有更新nativeArray中的值。

你必须从一个index将数据拷贝到一个局部临时副本，修改这个副本，并将它保存回去，像这样：

```
MyStruct temp = myNativeArray[i];
temp.memberVariable = 0;
myNativeArray[i] = temp;
```

### 调用JobHandle.Complete来重新获得归属权

在主线程重新使用数据前，追踪数据的所有权需要依赖项都完成。只检查JobHandle.IsCompleted是不够的。你必须调用JobHandle.Complete来在主线程中重新获取NaitveContainer类型的所有权。调用Complete同时会清理安全性系统中的状态。不这样做的话会造成内存泄漏。这个过程也在你每一帧都调度依赖于上一帧job的新job时被采用。

### 在主线程中调用Schedule和Complete

你只能在主线程中调用Schedule和Complete方法。如果一个job需要依赖于另一个，使用JobHandle来处理依赖关系而不是尝试在job中调度新的job。

### 在正确的时间调用Schedule和Complete

一旦你拥有了一个job所需的数据，尽可能快地在job上调用Schedule，在你需要它的执行结果之前不要调用Complete。一个良好的实践是调度一个你不需要等待的job，同时它不会与当前正在运行的其他job产生竞争。举例来说，如果你在一帧结束和下一帧开始之前拥有一段没有其他job在运行的时间，并且可以接受一帧的延迟，你可以在一帧结束的时候调度一个job，在下一帧中使用它的结果。或者，如果这个转换时间已经被其他job占满了，但是在一帧中有一大段未充分利用的时段，在这里调度你的job会更有效率。

### 将NativeContainer标记为只读的

记住job在默认情况下拥有NativeContainer的读写权限。在合适的NativeContainer上使用\[ReadOnly]属性可以提升性能。

### 检查数据的依赖

在Unity的Profiler窗口中，主线程中的"WaitForJobGroup"标签表明了Unity在等待一个工人线程上的job结束。这个标签可能意味着你以某种方式引入了一个资源依赖，你需要去解决它。查找JobHandle.Complete来追踪你在什么地方有资源依赖，导致主线程必须等待。

### 调试job

job拥有一个Run方法，你可以用它来替代Schedule从而让主线程立刻执行这个job。你可以使用它来达到调试目的。

### 不要在job中开辟托管内存

在job中开辟托管内存会难以置信得慢，并且这个job不能利用Unity的Burst编译器来提升性能。Burst是一个新的基于LLVM的后端编译器技术，它会使事情对于你更加简单。它获取C# job并利用你平台的特定功能产生高度优化的机器码。

参考

1. [https://zhuanlan.zhihu.com/p/58125078](https://zhuanlan.zhihu.com/p/58125078)
