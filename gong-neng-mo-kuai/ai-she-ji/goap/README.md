# GOAP

## Goap简易指南

## 概述

Goap全称为Goal Oriented Action Planning，是一种以目标为导向的模拟行为框架。

ReGoap具体实现了该框架，并使用A\*算法倒推行为。

### 名词解释

#### 感知器\[Sensor]

AI对于具体世界状态的感知器，代表了AI对世界状态的认知，可以通过调整刷新间隔来调整AI对世界状态改变的灵敏程度。

#### 记忆\[Memory]

AI对于感知器的管理器，由其管理AI所有的感知器，并汇总成AI感知的所有的世界状态。

世界状态由Dictionary\<string,object>结构表示，一个元素代表一个关注的世界状态，例如可以用 "attackingHero,true"的键值对来表示正在攻击玩家，而"attackingHero,false"则反之表示未攻击的状态。

#### 目标\[Goal]

设置的目标，用于驱动Planner管理器形成行动队列。

目标也是由Dictionary\<string,object>结构构成的，用于表示目标中的世界状态。

#### 行动\[Action]

主要由前置，后置，设置和执行方式组成。

前置，后置，设置都是由Dictionary\<string,object>结构表示的状态，与世界状态不同的是，此三者中的状态只用于形成行动队列的规划流程之中，而不会真正影响当前认知状态，而在Sensor中修改的memory的世界状态才会实际的更改认知状态。

### 举例

以近战怪物攻击玩家举例。

近战怪物A的目标是杀死玩家B，它有行走、攻击两种行为。那就可以对这一个目标和两种行为进行抽象。

#### 目标抽象

**目标\[Goal]：攻击玩家**

前置条件\[Pre]：无

效果\[Effect]：attackingHero true

优先级\[Priority]：1

**行为\[Action]：行走**

设置\[Setting]：从当前目标中找到"atTransform X"词条，将X作为目标点存储->targetTransform X

前置条件\[Pre]：无

运行\[Run]：执行A怪物蓝图的Run方法，朝目标移动。

效果\[Effect]：atTransform X

**行为\[Action]：攻击任意英雄**

设置\[Setting]：以玩家感知器为参照，设置一个目标玩家作为攻击目标X

前置条件\[Pre]：atTransform X

运行\[Run]：执行A怪物蓝图的Run方法。

效果\[Effect]：attackingHero true

#### 形成行动队列的过程概述

1\.  管理器查找到玩家身上的目标，根据优先级查找排序，获取优先级最高的目标，目前只有一个目标\[Goal]：攻击玩家，所以将其设置为当前目标。

2\.  检查当前状态是否和目标状态一致，当前状态是空，攻击玩家这个目标的效果是attackingHero true，两者不一致，所以管理器查找玩家身上所有的Action，根据当前目标的效果开始倒推。目前的目标状态是attackingHero true，而当前两个action中攻击任意英雄这个Action的效果和这个最终目标一致，所以将该Action放入栈中。同时更新当前目标（由attackingHero true变为攻击任意英雄的前置条件atTransform X）。

3\.  不断重复第二步，当前目标是atTransform X，而当前两个action中行走这个action的效果和这个当前目标一致，所以将Action放入栈中。同时更新当前目标（由atTransform X变为无）。

4\.  当前状态和目标状态一致，结束倒推。将栈中的Actions取出。形成行动队列。即行走->攻击任意英雄。

## 参考资料

1\.  GitHub Regoap [https://github.com/luxkun/ReGoap](https://github.com/luxkun/ReGoap)

2\.  [目标导向的AI系统（Goal Oriented Action Planning）技术分享 - 知乎](https://zhuanlan.zhihu.com/p/138003795)

3\.  [https://www.youtube.com/watch?v=gm7K68663rA\&t=463s](https://www.youtube.com/watch?v=gm7K68663rA\&t=463s)
