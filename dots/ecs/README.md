# ECS

ECS是**一种软件架构模式，由三个元素组成：实体(Entity)，组件(Component)和系统(System)**（看起来和MVC很相似）。 游戏程序分为这三个主要元素，并且通过定义每个系统的责任和关系来管理游戏。 实体代表游戏世界中的事物。 实体本身没有特定功能，它们将会被组件填充来成为一个实体。

## 滞后初始化技巧

查询不带需要初始化Component的实体，然后依次初始化该Component。

```
        //滞后初始化技巧
        Entities.WithoutBurst().WithAll<LocalTransform>().WithNone<ORCAAgent>().ForEach((Entity entity, LocalTransform lt) =>
            {
                var agentId = Simulator.Instance.addAgent(new Vector2(lt.Position.x, lt.Position.z));
                ecb.AddComponent(entity, new ORCAAgent { id = agentId });
            }).Run();
```
