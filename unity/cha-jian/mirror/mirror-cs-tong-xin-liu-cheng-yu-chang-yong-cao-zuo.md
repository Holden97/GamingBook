# Mirror C/S通信流程与常用操作

## 客户端的连接与断开

服务器可以将一个角色(character)的conn添加到现有客户端连接(抽象的gameplayer)

```
		if (this.character.TryGetComponent(out NetworkIdentity identity))
		{
			NetworkConnectionToClient conn = identity.connectionToClient;
			if (conn == null) yield break;

			// Tell client to unload previous subscene. No custom handling for this.
			conn.Send(new SceneMessage { sceneName = gameObject.scene.path, sceneOperation = SceneOperation.UnloadAdditive, customHandling = true });

			yield return waitForFade;

			if (character != null)
			{
				var preCharacterObject = this.character.gameObject;
				NetworkServer.Destroy(preCharacterObject);
				this.character = null;
			}

			NetworkServer.RemovePlayerForConnection(conn, false);

			// Move player to new subscene.
			SceneManager.MoveGameObjectToScene(player, SceneManager.GetSceneByPath(sceneName));

			// Tell client to load the new subscene with custom handling (see NetworkManager::OnClientChangeScene).
			conn.Send(new SceneMessage { sceneName = sceneName, sceneOperation = SceneOperation.LoadAdditive, customHandling = true });

			//Debug.Log($"SendPlayerToNewScene AddPlayerForConnection {conn} netId:{conn.identity.netId}");
			NetworkServer.AddPlayerForConnection(conn, player);

			// host client would have been disabled by OnTriggerEnter above
			if (NetworkClient.localPlayer != null && NetworkClient.localPlayer.TryGetComponent(out LocalPlayerController playerController))
				playerController.enabled = true;
		}
```

这其中添加连接关键的一句为

```
		NetworkServer.AddPlayerForConnection(conn, player);
```

对应的gameplayer则将会在稍后调用

<pre><code><strong>                public override void OnStartLocalPlayer()
</strong></code></pre>

服务器可以删除连接对应的gameplayer，第二个参数为是否摧毁对应的gameplayer

```
		NetworkServer.RemovePlayerForConnection(conn, false);
```

已知的删除客户端连接的办法只有删除已有连接的对象，因为connectionToClient在外部只读。

{% hint style="info" %}
这里的gameplayer指在NetworkManager中指定的GamePlayerPrefab
{% endhint %}

物体的NetworkIdentity只有在NetworkServer.Spawn后可用于ClientRpc和Server的传输中。

## 权限(Authority)查找

### 如何在服务器查找物体的权限拥有者？

首先被查找物体应继承NetworkBehaviour组件，拥有者的NetworkIdentity组件表示为this.netIdentity.connectionToClient.identity。

### 如何在服务器查找权限拥有者拥有权限的所有物体？

首先被查找物体应继承NetworkBehaviour组件，所有物体的NetworkIdentity的HashSet表示为this.connectionToClient.clientOwnedObjects。注意，connectionToClient只在服务器生效。与之对应的在客户端生效的connectionToServer目前已被官方弃用。

### 如何在客户端查找是否拥有单个网络物体的权限？

首先被查找物体应继承NetworkBehaviour组件，则权限表示为netIdentity.hasAuthority。

目前针对无法在客户端获取连接拥有权限的所有物体的问题，所有获取物体的操作均在服务器端进行（直接在Server中或者通过\[Command]）。

## Spawn,ObserverClient与ClientRpc

服务器在发送ClientRpc时，只会将ClientRpc发送给该物体准备好了的观察者们。

Mirror关于发送Rpc的方法摘录如下：

```
    public static void SendToReadyObservers<T>(NetworkIdentity identity, T message, bool includeOwner = true, int channelId = Channels.Reliable)
        where T : struct, NetworkMessage
    {
        // Debug.Log($"Server.SendToReady {typeof(T)}");
        if (identity == null || identity.observers == null || identity.observers.Count == 0)
            return;

        using (NetworkWriterPooled writer = NetworkWriterPool.Get())
        {
            // pack message only once
            MessagePacking.Pack(message, writer);
            ArraySegment<byte> segment = writer.ToArraySegment();

            int count = 0;
            foreach (NetworkConnection conn in identity.observers.Values)
            {
                bool isOwner = conn == identity.connectionToClient;
                if ((!isOwner || includeOwner) && conn.isReady)
                {
                    count++;
                    conn.Send(segment, channelId);
                }
            }

            NetworkDiagnostics.OnSend(message, channelId, segment.Count, count);
        }
    }
```

在调用Spawn的过程中，服务器会将`public static void Spawn(GameObject obj, GameObject ownerPlayer)`的第二个参数所属连接的注册为观察者，如果在连接被注册为Rpc之前使用Rpc，那么Rpc无效。

根据实验，在OnStartServer时，客户端还没有来得及被注册为Observer。而在客户端OnStartAuthority时，客户端才被注册为了Observer。

## Command

command中是可以使用async/await的。例如

```
[Command]
private async void GetCaster()
{
	var player = netIdentity.connectionToClient.identity.GetComponent<GamePlayer>();
	await QTask.Wait(() =>  player.character != null);
	this.caster = player.character.netIdentity;
}
```
