# 加入星球时的场景同步

## 简述

在Mirror框架下的多人游戏中，最简单的切换切换场景的方式便是NetworkManger.singleton.ServerChangeScene.然而这种方式无法满足项目需求，即当一部分玩家在星球探索时想进入战斗/建造场景时，另一部分玩家应该不受其干扰的继续在星海中遨游。

官方关于这种情况的案例参见[https://mirror-networking.gitbook.io/docs/examples/multiple-additive-scenes](https://mirror-networking.gitbook.io/docs/examples/multiple-additive-scenes) 。

目前采用[https://mirror-networking.gitbook.io/docs/examples/multiple-additive-scenes](https://mirror-networking.gitbook.io/docs/examples/multiple-additive-scenes) 这个例子，即多人分别进入不同场景。

官方关于Scene Interest Management的介绍，这里的interest我倾向于翻译成团体。 [https://mirror-networking.gitbook.io/docs/interest-management/scene](https://mirror-networking.gitbook.io/docs/interest-management/scene)

使用添加子场景的方式加载。

主要流程为

1. 服务器加载子场景
2. 确认服务器加载结束后，服务器通过对应的连接(NetworkConnectionToClient)发送SceneMessage这个网络消息，使客户端自行加载对应子场景。
3. 在断开C/S时使用SceneManager卸载对应子场景，只保留当前的“容器场景”。

{% hint style="info" %}
不要忘记为子场景编写PhysicsSimulator(物理模拟)组件，Unity不会为子场景自动进行物理模拟。

Unity不开启自动模拟意味着射线检测等等一切与物理场景相关的功能都需要特别处理。具体API参见[https://docs.unity3d.com/ScriptReference/PhysicsScene.html](https://docs.unity3d.com/ScriptReference/PhysicsScene.html)

PhysicsScene的API中，只有

`public bool Raycast(`[`Vector3`](https://docs.unity3d.com/ScriptReference/Vector3.html) `origin,` [`Vector3`](https://docs.unity3d.com/ScriptReference/Vector3.html) `direction, float maxDistance = Mathf.Infinity, int layerMask = Physics.DefaultRaycastLayers,` [`QueryTriggerInteraction`](https://docs.unity3d.com/ScriptReference/QueryTriggerInteraction.html) `queryTriggerInteraction = QueryTriggerInteraction.UseGlobal);`

`可以使用之外（参见` [`https://docs.unity3d.com/ScriptReference/PhysicsScene.Raycast.html`](https://docs.unity3d.com/ScriptReference/PhysicsScene.Raycast.html)`），球形，盒体碰撞检测都不知道如何使用（合理怀疑无法使用）。`
{% endhint %}



```
//物理模拟组件示例
public class PhysicsSimulator : MonoBehaviour
{
	PhysicsScene physicsScene;
	PhysicsScene2D physicsScene2D;

	bool simulatePhysicsScene;
	bool simulatePhysicsScene2D;

	void Awake()
	{
		if (NetworkServer.active)
		{
			physicsScene = gameObject.scene.GetPhysicsScene();
			simulatePhysicsScene = physicsScene.IsValid() && physicsScene != Physics.defaultPhysicsScene;

			physicsScene2D = gameObject.scene.GetPhysicsScene2D();
			simulatePhysicsScene2D = physicsScene2D.IsValid() && physicsScene2D != Physics2D.defaultPhysicsScene;
		}
		else
		{
			enabled = false;
		}
	}

	void FixedUpdate()
	{
		if (!NetworkServer.active) return;

		if (simulatePhysicsScene)
			physicsScene.Simulate(Time.fixedDeltaTime);

		if (simulatePhysicsScene2D)
			physicsScene2D.Simulate(Time.fixedDeltaTime);
	}
}
```

注意，SceneInterestManagement的SetHostVisibility默认实现中只隐藏Renderer，可以重写SetHostVisibility以实现更多自定义细节。

## 几个问题

### 如何使多个场景正确的切换？

这个问题要结合[https://mirror-networking.gitbook.io/docs/examples/additive-scenes](https://mirror-networking.gitbook.io/docs/examples/additive-scenes) 这个AdditiveScene 多场景切换的例子来看。

从这个例子中我们可以知道两点

1.  服务器加载使用的是SceneManager.LoadAsync，可以使用yield return达到等待的效果。例如

    ```
            foreach (string additiveScene in additiveScenes)
                yield return SceneManager.LoadSceneAsync(additiveScene, new LoadSceneParameters
                {
                    loadSceneMode = LoadSceneMode.Additive,
                    localPhysicsMode = LocalPhysicsMode.Physics3D // change this to .Physics2D for a 2D game
                });
    ```
2.  客户端加载使用的是SceneMessage(第三个参数的具体含义不清楚，第一个参数是操作的场景名，第二个参数是操作内容)。例如

    ```
    conn.Send(new SceneMessage { sceneName = additiveScenes[0], sceneOperation = SceneOperation.LoadAdditive, customHandling = true });
    ```
3. 需要注意一个问题，场景在默认情况下，在何种状态才会切换？\
   在默认实现中，当一个场景里有物品能被代表玩家的connection观测到时，那么就意味着所有场景里的物品都能被玩家观测到，从而这个场景就不会被认为可以被切换。
4. 哪些GameObject是可以被禁用的？\
   那些不需要同步的物品是可以被直接禁用(.SetActive(false))的，禁用的物体无法被同步，所以那些需要同步的网络物体则需要在主机上做特殊处理。例如，可以通过禁用其MeshRenderer组件使其看上去“被禁用了”。
5. 技能系统同步NetworkIdentity，而位移技能因为使用ClientAuthority模式，所以技能图在客户端生成。
