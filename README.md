# DOTS

## 什么是DOTS

DOTS全程为Data-Oriented Technology Stack，翻译为多线程式数据导向型技术堆栈。

DOTS包含以下元素：

1. 实体组件系统(ECS) - 提供使用面向数据的方法进行编码的框架。它通过Entities软件包进行分发，您可以通过Package Manager来添加编辑器。
2. C#作业系统 - 提供一种生成多线程代码的简单方法。它通过Jobs软件包进行分发。
3. Burst编译器 - 可生成快速、优化的本机代码。它通过Burst软件包进行分发，可通过Package Manager在编辑器中使用。
4. 本机容器 - 属于ECS数据结构，可提供对内存的控制。

元素1-3通常被称为DOTS的三个支柱，可作为单独的Unity软件包使用。它们共同构成了交付面向数据的高性能解决方案的基础。

## 什么是面向数据的设计？它与面向对象的设计有何不同？

面向数据的设计将编码的重点放在了通过优先排列和组织数据以尽可能提高其内存访问效率来解决问题上。这与面向对象的原理截然不同，后者强调代码的设计应由您所创建的事物模型来主导。在DOTS之前，面向对象的编程模型是Unity工作的全部。典型的面向对象的工作流程是：

1. 创建游戏对象
2. 为其添加组件
3. 编写用于更改这些组件属性的MonoBehaviour脚本

在运行时，游戏对象依赖于对组件的引用。当MonoBehaviour脚本查找组件数据时，由于它们分散在内存中，因此需要花费一些时间才能找到。采用面向对象的方法时，代码的结构通常基于以下几点：事物的构造（对象，构建为“类”）、事物的构成（数据对象内容）、事物行为内容的定义，以及它们如何与其他事物进行交互。这几乎是我们在现实世界中创造事物的自然延伸。但是，就结构化对象所含的信息而言，该方法在底层数据上增加了一个抽象层，这可能会扭曲数据的组织方式并减慢其访问速度。例如，如果您希望函数对某个对象内的另一段数据进行操作，那么该函数就需要从父类进行继承或者需要重写。在面向数据的设计中，您需要将所有内容都视为数据，而不是对象。这样做有助于确保所有数据都易于访问，并且在使用时可以不受层级类的限制。对于面向数据的方法，典型的工作流程是先确定然后组织支持您要实现的最常见任务的数据。最常见的问题就是在运行时最需要解决的问题，因此在开发中优先考虑这些问题正是关键所在。围绕数据来组织代码以及数据的流动，意味着在运行时对其访问要比通过多个类来获取数据更加有效。

参考

1. [https://learn.unity.com/tutorial/shi-yao-shi-dots-wei-shi-yao-shuo-dotsfei-chang-zhong-yao?language=zh#](https://learn.unity.com/tutorial/shi-yao-shi-dots-wei-shi-yao-shuo-dotsfei-chang-zhong-yao?language=zh)
