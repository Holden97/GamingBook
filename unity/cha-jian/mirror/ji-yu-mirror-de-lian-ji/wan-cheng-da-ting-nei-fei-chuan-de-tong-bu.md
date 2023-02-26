# 完成大厅内飞船的同步

飞船模型是飞船的子物体，但TA的实现是让模型独自运动，所以要在飞船根节点中添加Network Transform Child组件，控制模型移动。

一个严重的问题是在此之前，我一直认为如下的写法没有问题。

```
			var go = Instantiate(item, ShipRespawnPoint, transform.rotation);
			NetworkServer.Spawn(go, gameObject);
			HandlePlayerNameUpdate(this.playerName, newPlayerName);
			SceneManager.MoveGameObjectToScene(go, this.gameObject.scene);
			RpcSetPlayerProperty(go);
```

这样的写法将新生成的物体直接用于ClientRpc当中，单机和双人（一台host一台client）模式下的确没有问题，但连接第三个人（即第二台客户端）的时候，第一台连接的客户端响应`NetworkServer.Spawn(go, gameObject);`的创建会有延迟，Rpc中的参数如果是GameObject，是要通过NetworkIdentity去寻找对应的物体的，如果创建不及时，那么在这个客户端中就没有对应NetworkworkIdentity的物体，从而会造成`RpcSetPlayerProperty(go);`当中的参数`go`为null。

目前规避此问题的方法就是不用Rpc做创建后的操作，而是化整为零，将创建后的操作分到各个网络物体的OnStartLocalPlayer方法中去。

所有的子弹物体在**服务器和客户端**的三个子场景中应始终保持正确位置，而不能像官方示例AddActiveScene中一样不管客户端在什么场景。因为子弹非网络物体，所以不能通过服务器控制客户端字典的销毁，而必须是依靠客户端自身的物理系统检测是否碰撞到物体然后销毁。子弹的射线检测靠的是PhysicsScene.Raycast，这个方法仅在同一物理场景内生效，假设客户端生成位置不正确，比如他需要在战斗场景，但客户端没有做处理而在容器场景内， 那么子弹和敌人不在同一个场景，那么就无法检测，自然就会出错。

飞船需要添加Network Transform组件，并且勾选Client Authority来降低延迟感。
