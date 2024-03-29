# 设计原则

### 设计模式的七大原则 <a href="#8qe13" id="8qe13"></a>

面向对象的设计模式有七大基本原则：

* 开闭原则（Open Closed Principle，OCP）
* 单一职责原则（Single Responsibility Principle, SRP）
* 里氏代换原则（Liskov Substitution Principle，LSP）
* 依赖倒转原则（Dependency Inversion Principle，DIP）
* 接口隔离原则（Interface Segregation Principle，ISP）
* 合成/聚合复用原则（Composite/Aggregate Reuse Principle，CARP）
* 最少知识原则（Least Knowledge Principle，LKP）或者迪米特法则（Law of Demeter，LOD）

| 标记   | 设计模式原则名称  | 简单定义                     |
| ---- | --------- | ------------------------ |
| OCP  | 开闭原则      | 对扩展开放，对修改关闭              |
| SRP  | 单一职责原则    | 一个类只负责一个功能领域中的相应职责       |
| LSP  | 里氏代换原则    | 所有引用基类的地方必须能透明地使用其子类的对象  |
| DIP  | 依赖倒转原则    | 依赖于抽象，不能依赖于具体实现          |
| ISP  | 接口隔离原则    | 类之间的依赖关系应该建立在最小的接口上      |
| CARP | 合成/聚合复用原则 | 尽量使用合成/聚合，而不是通过继承达到复用的目的 |
| LOD  | 迪米特法则     | 一个软件实体应当尽可能少的与其他实体发生相互作用 |

## 参考资料&#x20;

1. [https://cloud.tencent.com/developer/article/1650116](https://cloud.tencent.com/developer/article/1650116)
