# 工作队列

目前工作队列的设计如下

添加时分两种工作模式

1. 完全顺序的工作队列
2. 树结构的复杂工作逻辑

第一种没有什么理解成本，就是普通的队列，在预定的当前工作完成的情况下，将当前工作移出队列，并开始执行队列中的下一个工作。

为什么要设计第二种呢？因为发现单纯的顺序执行并不能满足“收割当前区域所有小麦，并将他们放入存储区”这样的工作需求。

我从行为树的思想中获得了一些启发，并结合已有的工作队列，形成了一种“基于工作队列和行为节点”的工作驱动模式。即行为节点可以包含封装好的简单顺序工作（注意，目前只能是顺序工作，而不能有循环工作，循环逻辑应该交给行为树节点实现）。

有两个队列，一个是行为树节点队列， 一个是工作节点队列。

对于简单的顺序工作，可以直接操作工作节点队列，它是顺序执行的。

对于复杂一些的工作（包含循环，选择等），则使用行为树节点队列编写逻辑，当执行到对应的行为树节点时，将其中的顺序工作交给工作节点队列，由工作节点队列来确定具体执行的工作。
