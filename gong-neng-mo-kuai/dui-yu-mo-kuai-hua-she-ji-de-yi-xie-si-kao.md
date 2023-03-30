# 对于模块化设计的一些思考

松耦合，高内聚

1.子物体不应引用父节点，应使用事件或者委托，例如，点击事件应通过注册回调（委托）的方式在父节点实现。

简单来说，子级不调用父级，应在父级注册子级的回调（让父级调用子级注册回调）。

对象之间的四种交互方式

1.方法调用，A直接调用B的方法

2.委托/回调 A持有B，注册B中声明的委托，例如点击事件的监听

3.消息/事件 A,B之间不需要相互了解

4.Command

模块化一般也有三种

单例

IOC

分层 例如MVC，三层架构，领域驱动分层

交互逻辑

view->model

表现逻辑

model->view

成型的框架研究

PureMVC,StrangelOC,uFrame,.Net Core的DDD实现

数据是底层，表现是顶层

struct相比class有更好的内存管理效率

命令模式可以让逻辑的调用和执行在空间、时间都实现分离。

表现层到底层用Command

底层到表现层用委托/事件，游戏状态事件只能通过底层发送给表现层，反之不行

表现层能够查询数据，可以被替换。

格式上，对不同的模块进行命名空间的区分是一个好习惯。

接口的显式实现

void ICanSayHello.SayHello(){}

不要无序的引用模块，更不要循环引用模块，因为引用模块意味着当被引用的模块改动时，需要对当前模块做出相应的修改。

参考资料

1. Unity 游戏框架搭建 决定版 [https://learn.u3d.cn/tutorial/framework\_design?chapterId=63562b29edca72001f21d172#](https://learn.u3d.cn/tutorial/framework\_design?chapterId=63562b29edca72001f21d172)
2. RPG开发 [https://www.udemy.com/course/unityrpg/learn/lecture/13845040#overview](https://www.udemy.com/course/unityrpg/learn/lecture/13845040#overview)
